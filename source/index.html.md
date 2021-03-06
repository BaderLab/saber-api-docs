---
title: Saber - API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
#  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
#  - errors

search: true
---

# Introduction

Welcome to the Saber API! You can use our API to annotate text for **biomedical concepts** and **entities**.

Each recognized instance of a concept in text is called an **entity**. Given the inherent ambiguity of natural language and the fuzzy boundaries between concepts and entity classes, some entities will be tagged with _multiple_ concepts, sometimes from different concept groups.

Parameters should be provided in a JSON encoded `POST` payload. All API calls accept an optional parameter named `ents`, specifying the types of entities you want to annotate. It is an object where keys are case-sensitive semantic group identifiers and values are booleans. By default (and if omitted), entities from all groups are annotated:

`"ents": { "CHED": true, "DISO": true, "LIVB": true, "PRGE": true, ... }`

If you do specify an `ents` object, any omitted entity class will be assumed to be `false`. For example, if you were only interested in tagging disorders (`DISO`) and genes/proteins (`PRGE`):

`"ents": { "DISO": true, "PRGE": true }`

The following table lists all supported semantic groups, their corresponding identifiers, the types of identified entities and the namespace the entities will be grounded to.

Identifier | Semantic Group | Identified entity types | Namespace |
---------- | -------------- | ----------------------- | --------- |
`CHED` | Chemicals | Abbreviations and Acronyms, Molecular Formulas, Chemical database identifiers, IUPAC names, Trivial (common names of chemicals and trademark names), Family (chemical families with a defined structure) and Multiple (non-continuous mentions of chemicals in text) | [PubChem Compounds](https://pubchem.ncbi.nlm.nih.gov/)
`DISO` | Disorders | Acquired Abnormality, Anatomical Abnormality, Cell or Molecular Dysfunction, Congenital Abnormality, Disease or Syndrome, Mental or Behavioral Dysfunction, Neoplastic Process, Pathologic Function, Sign or Symptom | [Disease Ontology](http://disease-ontology.org/)
`LIVB` | Organisms | Species, Taxa | [NCBI Taxonomy](https://www.ncbi.nlm.nih.gov/taxonomy)
`PRGE` | Genes and Gene Products | Genes, Gene Products | [STRING](https://string-db.org/)

We have language bindings in Shell, Ruby, Python, and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Annotate

## Annotate Raw Text

```ruby
TODO
```

```python
import requests # assuming you have requests package installed!

url = "http://localhost:5000/annotate/pmid"
payload = {"text": "The phosphorylation of Hdm2 by MK2 promotes the ubiquitination of p53."}
response = requests.post(url, json=payload)

print(response.text)
print(response.status_code, response.reason)
```

```shell
curl -X POST 'http://localhost:5000/annotate/text' \
--data '{"text": "The phosphorylation of Hdm2 by MK2 promotes the ubiquitination of p53."}'

```

```javascript
TODO
```

> The above command returns JSON structured like this:

```json
{
  "ents": [
    {
      "start": 23,
      "end": 27,
      "label": "PRGE",
      "text": "Hdm2"
    },
    {
      "..."
    }
  ],
  "text": "The phosphorylation of Hdm2 by MK2 promotes the ubiquitination of p53."
}
```

This endpoint annotates raw text.

### HTTP Request

`POST http://localhost:5000/annotate/text`

### Query Parameters

Param | Type | Default | Description
----- | ---- | ------- | -----------
`text` | String | None (required) | Raw text to annotate.
`ents` | Object: `"ents": {"<ENT>": true/false, ...}` | If omitted, all entities are `true`. If some entities are present, all others are `false`. | Which biomedical entities to annotate.
`coref` | Boolean: `true`/`false` | `false` | `true` if coreference resolution should be performed before annotating the text.
`ground` | Boolean: `true`/`false` | `false` | `true` if entities should be "grounded", i.e. linked to database identifiers.

<aside class="notice">
For example, to annotate some given <code>text</code> for the entity class <code>PRGE</code>, you would provide <code>"ents": {"PRGE": true}</code> in your JSON payload.
</aside>

## PubMed Articles

```ruby
TODO
```

```python
import requests # assuming you have requests package installed!

url = "http://localhost:5000/annotate/pmid"
payload = {"pmid": 29970521}
response = requests.post(url, json=payload)

print(response.text)
print(response.status_code, response.reason)
```

```shell
curl -X POST 'http://localhost:5000/annotate/pmid' \
--data '{"pmid": 29970521}'
```

```javascript
TODO
```

> The above command returns JSON structured like this:

```json
{
  "ents": [
    {
      "start": 14,
      "end": 16,
      "label": "PRGE",
      "text": "p53"
    },
    {
      "..."
    }
  ],
  "text": "Since most cancers are associated with alterations of the p53 and Rb pathways ..."
}
```

This endpoint annotates a PubMed article, given its PubMed identifier ([**PMID**](https://en.wikipedia.org/wiki/PubMed#PubMed_identifier)).

### HTTP Request

`POST http://localhost:5000/annotate/pmid`

### Query Parameters

Param | Type | Default | Description
----- | ---- | ------- | -----------
`pmid` | Integer | None (required) | [PMID](https://en.wikipedia.org/wiki/PubMed#PubMed_identifier) of article to annotate.
`ents` | Object: `"ents": {"<ENT>": true/false, ...}` | If omitted, all entities are `true`. If some entities are present, all others are `false`. | Which biomedical entities to annotate.
`coref` | Boolean: `true`/`false` | `false` | `true` if coreference resolution should be performed before annotating the text.
`ground` | Boolean: `true`/`false` | `false` | `true` if entities should be "grounded", i.e. linked to database identifiers.
