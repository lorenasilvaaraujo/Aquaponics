---
title: "{{title}}"
authors: {{authors}}
year: {{date | format("YYYY")}}
publisher:  {{publicationTitle}} 
zotero_keywords: [{{allTags}}]
citekey: {{citekey}}
type:
region:
plant:
fish:
---

> [!meta]- Metadata
> zotero_link:: {{pdfZoteroLink}}
> Related:: {% for relation in relations -%} {%- if relation.citekey -%} [[{{relation.citekey}}]], {% endif -%} {%- endfor%}
> url:: {{url}}
> doi:: {{doi}}
> bibliography:: {{bibliography}}


#### Webpage
<iframe src="{{url}}" style="height:30%;width:100%; aspect-ratio: 16 / 10"></iframe>

#### Opinion
> [!tip] Opinion
>
>

#### Abstract
>{{abstractNote}}



#### Self Notes

> [!info] Experiment data
>

|  |  |  |  |
| :--- | :--- | :--- | :--- |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |


```math 
 A = 1/1 
 B = 1/1
 C = 1/1
```



#### Highlights
{% persist "annotations" %}

{%-
    set zoteroColors = {
	    "#2ea8e5": "blue",
        "#5fb236": "green",
        "#a28ae5": "purple",
        "#ff6666": "red",
        "#ffd400": "yellow",
        "#f19837": "orange",
        "#aaaaaa": "grey",
        "#e56eee": "magenta"
    }
-%}

{%-
   set colorHeading = {
		"yellow": "⭐ Overall Notes",
		"grey": "🧪Experimental details or Methods",
		"blue": "ℹ Background information, Prerequisites",
		"red": "⚠️ Disagree with author",
		"green": "❓ Follow-up on Reference",		
		"magenta": "📄 Critique",
		"orange": "📊 Citable"
	}
-%}

{%- macro calloutHeader(type) -%}
    {%- switch type -%}
        {%- case "highlight" -%}
        Highlight
        {%- case "image" -%}
        Image
        {%- default -%}
        Note
    {%- endswitch -%}
{%- endmacro %}

{%- set newAnnot = [] -%}
{%- set newAnnotations = [] -%}
{%- set annotations = annotations | filterby("date", "dateafter", lastImportDate) %}

{% if annotations.length > 0 %}
*Imported: {{importDate | format("YYYY-MM-DD HH:mm")}}*

{%- for annot in annotations -%}

    {%- if annot.color in zoteroColors -%}
        {%- set customColor = zoteroColors[annot.color] -%}
    {%- elif annot.colorCategory|lower in colorHeading -%}
    	{%- set customColor = annot.colorCategory|lower -%}
    {%- else -%}
	    {%- set customColor = "other" -%}
    {%- endif -%}

    {%- set newAnnotations = (newAnnotations.push({"annotation": annot, "customColor": customColor}), newAnnotations) -%}

{%- endfor -%}

{#- INSERT ANNOTATIONS -#}
{#- Loops through each of the available colors and only inserts matching annotations -#}
{#- This is a workaround for inserting categories in a predefined order (instead of using groupby & the order in which they appear in the PDF) -#}

{%- for color, heading in colorHeading -%}
{%- for entry in newAnnotations | filterby ("customColor", "startswith", color) -%}
{%- set annot = entry.annotation -%}

{%- if entry and loop.first %}

##### {{colorHeading[color]}}
{%- endif %}

> [!quote{{"|" + color if color != "other"}}]+ {{calloutHeader(annot.type)}} ([page. {{annot.pageLabel}}](zotero://open-pdf/library/items/{{annot.attachment.itemKey}}?page={{annot.pageLabel}}&annotation={{annot.id}}))

{%- if annot.annotatedText %}
> {{annot.annotatedText|nl2br}} {% if annot.hashTags %}{{annot.hashTags}}{% endif -%}
{%- endif %}

{%- if annot.imageRelativePath %}
> ![[{{annot.imageRelativePath}}]]
{%- endif %}

{%- if annot.ocrText %}
> {{annot.ocrText}}
{%- endif %}

{%- if annot.comment %}
> - **{{annot.comment|nl2br}}**
{%- endif -%}

{%- endfor -%}
{%- endfor -%}
{% endif %}
{% endpersist %}