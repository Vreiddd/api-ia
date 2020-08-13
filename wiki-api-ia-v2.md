### API-IA v2 Documentation

#### Routes

###### Envoyer un compte rendu

- [<code>POST</code> /report](#post-report)

###### Diagnostics

- [<code>POST</code> /diagnostics](#post-diagnostics)
- [<code>GET</code> /diagnostics](#get-diagnostics)
- [<code>PUT</code> /diagnostics/:id](#put-diagnosticsid)
- [<code>PATCH</code> /diagnostics/:id](#patch-diagnosticsid)
- [<code>DELETE</code> /diagnostics/:id](#delete-diagnosticsid)

###### Feedbacks

- [<code>POST</code> /feedbacks](#post-feedbacks)
- [<code>GET</code> /feedbacks](#get-feedbacks)
- [<code>PUT</code> /feedbacks/:id](#put-feedbacksid)
- [<code>PATCH</code> /feedbacks/:id](#patch-feedbacksid)
- [<code>DELETE</code> /feedbacks/:id](#delete-feedbacksid)

###### Symptoms

- [<code>GET</code> /symptoms](#get-symptoms)

#### <code>POST</code> /report

Permet d'envoyer le compte rendu du médecin à l'intelligence artificielle et de récupérer son résultat.

###### Entrée (body)

>Token d'authentification nécessaire dans les headers

Compte rendu en raw bytes.

###### Sortie
Retourne les maladies potentielles et leurs probabilités ainsi les symptômes associés.
```JSON
{
    "diseases": [
        {
            "disease": "Adenoma - Basophil",
            "probability": 0.10171734599604695
        },
        {
            "disease": "Coproporphyria - Hereditary",
            "probability": 0.10171734599604695
        },
        {
            "disease": "Chondrosarcoma - Mesenchymal",
            "probability": 0.10048067243340904
        },
        {
            "disease": "Papilloma - Choroid Plexus",
            "probability": 0.10048067243340904
        },
    ],
    "symptoms": [
        "Headache",
        "Vomiting"
    ]
}
```
#### <code>POST</code> /diagnostics

Création d'un diagnostic.

###### Entrée (body)

Nécessite l'entièreté des champs du diagnostic sous format JSON.
```JSON
{
}
```
###### Sortie

Retourne le diagnostic créé avec notamment son id.
```JSON
{
}
```

#### <code>GET</code> /diagnostics

Obtention d'un diagnostic précédement créé.
- Par id: GET /diagnostics/:id
- Par queries: GET /diagnostics?uuid_doctor=34&limit=15...

###### Queries

* uuid_doctor: *sélectionne seulement les diagnostics avec l'uuid du médecin correspondant*
* uuid_patient: *sélectionne seulement les diagnostics avec l'uuid du patient correspondant*
* limit: *nombre maximum de résultat*

###### Entrée

>Token d'authentification nécessaire dans les headers

*Aucune donnée dans le body de la requête nécéssaire*

###### Sortie
Retourne une liste de diagnostics correspondant à la requête.

```JSON
{
  "diagnostics": [
    {
      "id": "114a4527",
      "uuid_doctor": "123e4567-e89b-12d3-a456-426655440002",
      "uuid_patient": "123e4567-e89b-12d3-a456-42665544021",
      "report": "some headache with vomiting",
      "date": "08/03/2020",
      "diseases": ["Schizophrenia", "Breast Neoplasms"],
      "symptoms": ["Headache", "Vomiting"]
    },
    {
      "id": "567493s",
      "uuid_doctor": "123e4567-e89b-12d3-a456-426655440002",
      "uuid_patient": null,
      "report": "back pain with headache",
      "date": "02/03/2020",
      "diseases": ["Schizophrenia", "Breast Neoplasms"],
      "symptoms": ["Headache", "Back Pain"]
    }
  ]
}
```

#### <code>PUT</code> /diagnostics/:id

Mise à jour complète d'un diagnostic par son id.

###### Entrée (body)

>Token d'authentification nécessaire dans les headers

Nécessite l'entièreté des champs du diagnostic sous format JSON.
```JSON
{
}
```
###### Sortie

Retourne le diagnostic mis à jour.
```JSON
{
}
```

#### <code>PATCH</code> /diagnostics/:id

Mise à jour partielle d'un diagnostic par son id.

###### Entrée (body)

>Token d'authentification nécessaire dans les headers

Envoi des champs spécifiques à modifier sous format JSON.
```JSON
{
}
```
###### Sortie

Retourne le diagnostic mis à jour.
```JSON
{
}
```

#### <code>DELETE</code> /diagnostics/:id

Supprime un diagnostic par son id.

###### Entrée (body)

>Token d'authentification nécessaire dans les headers

*Aucune donnée dans le body de la requête nécéssaire*

#### <code>POST</code> /feedbacks

Création d'un feedback.

###### Entrée (body)

Nécessite l'entièreté des champs du feedback sous format JSON.
```JSON
{
  "report_id": "74201r2",
  "text": "New back pain",
  "diseases": ["Back Pain", "Headache"]
}
```
###### Sortie

Retourne le feedback créé avec notamment son id.
```JSON
{
  "id": "419h23",
  "report_id": "74201r2",
  "text": "New back pain",
  "diseases": ["Back Pain", "Headache"]
}
```

#### <code>GET</code> /feedbacks

Obtention d'un feedback précédement créé.
- Par id: GET /feedbacks/:id
- Par queries: GET /feedbacks?id_report=74201r2&disease=headache&limit=15...

###### Queries

* id_report: *sélectionne seulement les feedbacks reliés au diagnostic spécifié par l'id*
* disease: *sélectionne seulement les feedbacks reliés à la maladie précisée*
* limit: *nombre maximum de résultat*

###### Entrée

>Token d'authentification nécessaire dans les headers

*Aucune donnée dans le body de la requête nécéssaire*

###### Sortie
Retourne une liste de feedback correspondant à la requête.

```JSON
{
  "feedbacks": [
    "id": "419h23",
    "report_id": "74201r2",
    "text": "New back pain",
    "diseases": ["Back Pain", "Headache"]
  ]
}
```

#### <code>PUT</code> /feedbacks/:id

Mise à jour complète d'un feedback par son id.

###### Entrée (body)

>Token d'authentification nécessaire dans les headers

Nécessite l'entièreté des champs du feedback sous format JSON.
```JSON
{
    "report_id": "73403d1",
    "text": "Break leg after surgery",
    "diseases": ["Leg pain"]
}
```
###### Sortie

Retourne le feedback mis à jour.
```JSON
{
    "id": "251f233",
    "report_id": "73403d1",
    "text": "Break leg after surgery",
    "diseases": ["Leg pain"]
}
```

#### <code>PATCH</code> /feedbacks/:id

Mise à jour partielle d'un feedback par son id.

###### Entrée (body)

>Token d'authentification nécessaire dans les headers

Envoi des champs spécifiques à modifier sous format JSON.
```JSON
{
    "text": "Break leg after car accident",
}
```
###### Sortie

Retourne le feedback mis à jour.
```JSON
{
    "id": "251f233",
    "report_id": "73403d1",
    "text": "Break leg after car accident",
    "diseases": ["Leg pain"]
}
```

#### <code>DELETE</code> /feedbacks/:id

Supprime un feedback par son id.

###### Entrée (body)

>Token d'authentification nécessaire dans les headers

*Aucune donnée dans le body de la requête nécéssaire*

#### <code>GET</code> /symptoms

###### Entrée

>Token d'authentification nécessaire dans les headers

*Aucune donnée dans le body de la requête nécéssaire*

###### Sortie
Retourne les archives de tout les symptomes.

```JSON
{
  "symptoms": [
    "Head Ache",
    "Vomiting",
    "Depression"
  ]
}
```
