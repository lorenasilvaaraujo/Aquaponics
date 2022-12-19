
citekey: {{citekey}}  
alias: [{% if shortTitle %}"{{shortTitle | safe}}"{% else %}"{{title | safe}}"{% endif %}]  
title: "{{title}}"  
authors: {{authors}}  
tags: [{% for t in tags %}{{t.tag}}{% if not loop.last %}, {% endif %}{% endfor %}]  
year: {{date | format("YYYY")}}  
publisher: "{{publicationTitle}}"  

  
## {{title}}  
>[!info]-  
>{% if bibliography %}  
>**Bibliography:** {{bibliography}}  
>{% endif %}  
>{% if hashTags %}  
>**Tags:** {{hashTags}}  
>{% endif %}  
>{%- for attachment in attachments | filterby("path", "endswith", ".pdf") %}  
> **Link:** [{{attachment.title}}](file:///{{attachment.path | replace(" ", "%20")}}){%- endfor -%}  
>  
>[**Open in Zotero**]({{desktopURI}})  
>[**Open DOI**]([https://doi.org/](https://doi.org/){{DOI}})  
  
  
---  
  
  
> [!abstract]-  
> {% if abstractNote %}  
>{{abstractNote}}  
>{% endif %}  
  
---  
  
{% if markdownNotes %}  
{{markdownNotes}}{% endif %}