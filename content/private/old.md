---
title: {{title}}
Type:
Region:
Fish:
Plant:
---

{{title}}

## Metadata:

type: 📥
{{citekey}}
{{tags}}
{{itemType}}
{{author}}
{{date}}
{{dateAdded}}
{{proceedingsTitle}}
{{DOI}}
zotero_url: {{localLibrary}}
{{pdfAttachments}}

> [!tip] Opinion
>
>

>{{abstractNote}}

> [!info] Experiment data


{% for annotation in annotations %}
{% if annotation.annotatedText %}

> {{annotation.annotatedText}}
> {% endif %}
> {% if annotation.comment %}
> {{annotation.comment}}
{% endif %}
{% endfor %}