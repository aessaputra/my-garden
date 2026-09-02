---
{"dg-publish":true,"permalink":"/craft/","title":"Craft","tags":["categories"],"dg-note-properties":{"title":"Craft","tags":["categories"]}}
---

# Craft


```base
filters:
  and:
    - '!file.name.contains("Template")'
    - note.categories.contains(link("Craft"))
properties:
  note.status:
    displayName: Status
  note.url:
    displayName: URL
  file.name:
    displayName: Name
  note.type:
    displayName: Type
  note.year:
    displayName: Year
views:
  - type: table
    name: Table
    order:
      - file.name
      - type
      - year
      - status
      - url
    sort:
      - property: file.name
        direction: DESC
      - property: status
        direction: ASC
      - property: year
        direction: DESC
    columnSize:
      file.name: 209
      note.type: 199

```

