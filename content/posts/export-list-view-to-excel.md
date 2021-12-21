+++
title = "Export list view in Django admin to Excel file"
description = "Export list view in Django admin to Excel file"
tags = [
    "Django",
    "backend",
    "Export"
]
date = "2021-12-21"
categories = [
    "Development", 
    "Django",
]
+++ 
The goal is whaterver data is changed in list view by (search, filter, sort ) and you can export result list in same view as you see and you don't need to manually code every column and data in Excel file.

Firstly, create share mixin to export to Excel file

```py
from jutil.openpyxl_helpers import rows_to_workbook
from uuid import uuid1
from django.utils.text import capfirst

class ExportExcelMixin:
    """
    Export current result list to Excel file with data is same as list view.
    """
    def get_head(self):
        map_field_label = {}
        head_row = []
        for f in self.export_model._meta.fields:  # type : ignore
            if hasattr(f, "verbose_name"):
                map_field_label[f.name] = f.verbose_name

        for column in self.list_display:  # type: ignore
            if column in map_field_label:
                head_row.append(capfirst(map_field_label[column]))
            else:
                method_to_call = getattr(self, column)
                head_row.append(capfirst(method_to_call.short_description))

        return head_row

    def get_data_rows(self, query_set, ):
        rows = []
        for model in query_set:
            item = []
            for column in self.list_display:  # type: ignore
                if hasattr(model, column):
                    item.append(getattr(model, column))
                else:
                    method_to_call = getattr(self, column)
                    item.append(method_to_call(model))

            rows.append(item)

        return rows

    def admin_export_excel(self, request):
        cl = self.get_changelist_instance(request)  # type: ignore
        queryset = cl.get_queryset(request)
        keys = self.get_head()
        rows = [keys]
        rows = rows + self.get_data_rows(queryset)
        wb = rows_to_workbook(rows)
        download_file_name = f"{settings.WEBSITE_NAME}-{self.export_model._meta.verbose_name}-{now().date()}.xlsx"  # type: ignore
        real_filename = f"/tmp/{uuid1}.xlsx"
        wb.save(real_filename)
        with open(real_filename, "rb") as fh:
            response = HttpResponse(fh.read(), content_type="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet")
            response["Content-Disposition"] = f"inline; filename={download_file_name}"
            return response

```

Secondly, add that Mixin class to admin class that you want to export list view and define export Model and overide get_url method

For example:

```py
class PostAdmin(admin.ModelAdmin, ExportExcelMixin):

    export_model = Post

    def get_urls(self):
        return [
            re_path(
                r"^export-excel/$",
                self.admin_site.admin_view(self.admin_export_excel),
                name="post_export_excel",
            ),
        ] + super().get_urls()

```

Next, you define web filter called "append_get_params", the purpose is append all search params and filter params and sort param to specific give url

```py
from urllib.parse import urlparse, urlencode, urlunparse
from django.template.defaultfilters import register

@register.simple_tag(takes_context=True)
def append_get_params(context, url):
    """
    Append all GET params to passed url
    """
    request = context.get('request')
    parsed_url = list(urlparse(url))
    parsed_url[4] = urlencode(request.GET)
    return urlunparse(parsed_url)
```

Finally, you need to verride changelist.html for that Admin model to add export link
For example post model in my app that templated will be located at( or you need to create it)

```
my/templates/admin/my/post/change_list.html
```

Here is the code for that file.

```html
{% block object-tools-items %}
    <li>
        {% url 'admin:post_export_excel' as export_url %}
        <a href="{% append_get_params export_url %}" class="downloadlink" >
            {% trans "Export to Excel" %}
        </a>
    </li>
    {% if has_add_permission %}
    <li>
        <a href="{% url 'admin:my_post_add' %}" class="addlink">{% trans "Add" %}</a>
    </li>
    {% endif %}

{% endblock %}
```

Combined with custom style, Here is the result

![export-to-excel.png](/wiki/images/export-to-excel.png)
