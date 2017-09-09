# Langkah REST services

Langkah REST services is a fork from [spacy-services](https://github.com/explosion/spacy-services).

It also uses the [FrenchLefffLemmatizer library](https://github.com/ClaudeCoulombe/FrenchLefffLemmatizer) by @ClaudeCoulombe to perform the lemma extraction of french words.

All requests and responses are JSON-encoded as `text/string`, so all requests require the header `Content-Type: text/string`.

## [Langkah server](langkah)

A simple [Falcon](https://falconframework.org/) app for exposing a spaCy model as a REST microservice, formatted for the [langkah](https://github.com/campdav/langkah-front) visualiser.

The service exposes endpoints that accept POST requests, and endpoints that accept GET requests to describe the available models and schemas.

---

### `POST` `/annot/`

Example request:

```json
{
    "text": "La souris est mangée par le chat.",
    "model": "fr"
}
```

| Name | Type | Description |
| --- | --- | --- |
| `text` | string | text to be parsed |
| `model` | string | identifier string for a model installed on the server  |

Example request using the Python [Requests library](http://docs.python-requests.org/en/master/):

```python
import json
import requests

url = "http://localhost:8001/ent"
message_text = "La souris est mangée par le chat."
headers = {'content-type': 'application/json'}
d = {'text': message_text, 'model': 'fr'}

response = requests.post(url, data=json.dumps(d), headers=headers)
r = response.json()
```

Example response:

```json
[
    { "end": 20, "start": 10,  "type": "passive" }
]
```

| Name | Type | Description |
| --- | --- | --- |
| `end` | integer | character offset the entity ends **after** |
| `start` | integer | character offset the entity starts **on** |
| `type` | string | entity type |



```
curl -s localhost:8001/ent -d '{"text":"La souris est mangée par le chat.", "model":"fr"}'
```

```json
[
  {
    "end": 20,
    "start": 10,
    "type": "passive"
  }
]
```


---

### `GET` `/models`

List the names of models installed on the server.

Example request:

```
GET /models
```

Example response:

```json
["en", "de"]
```

---

### `GET` `/{model}/schema/`

Example request:

```
GET /en/schema
```

| Name | Type | Description |
| --- | --- | --- |
| `model` | string | identifier string for a model installed on the server |

Example response:

```json
{
  "dep_types": ["ROOT", "nsubj"],
  "ent_types": ["PERSON", "LOC", "ORG"],
  "pos_types": ["NN", "VBZ", "SP"]
}
```
