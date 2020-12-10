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

##### Entrée

###### Request params

name         | example value        | rule     |
-------------|----------------------|----------|
uuid_doctor  | 90456543-gdbnj-55    | required |
uuid_patient | 5664322-ttgvs-78g    | optional |


###### Body
Compte rendu en raw bytes.
```
overweight and obesity, fever with cyanosis and an edema, eye manifestations and fatigue with flushing
```

##### Sortie

* 200 Ok

Retourne les maladies potentielles et leurs probabilités ainsi les symptômes associés.
```JSON
{
    "id": 4806,
    "uuid_doctor": "123e4567-e89b-12d3-a456-426655440002",
    "uuid_patient": null,
    "prediction": {
        "diseases": [
            {
                "disease": "Albuminuria",
                "probability": 0.14112404609491871
            },
            {
                "disease": "Pericarditis - Constrictive",
                "probability": 0.13488754115798504
            },
            {
                "disease": "Cushing Syndrome",
                "probability": 0.119588179644354
            },
            {
                "disease": "Primary Ovarian Insufficiency",
                "probability": 0.0997746547086913
            },
            {
                "disease": "Hemorrhagic Disorders",
                "probability": 0.09686901193848944
            },
            {
                "disease": "Aortic Valve Insufficiency",
                "probability": 0.08700586331927278
            },
            {
                "disease": "Heart Valve Diseases",
                "probability": 0.08163271932029344
            },
            {
                "disease": "Paraneoplastic Endocrine Syndromes",
                "probability": 0.08093434727000175
            },
            {
                "disease": "Lymphedema",
                "probability": 0.0799254302387167
            },
            {
                "disease": "Hepatomegaly",
                "probability": 0.07825820630727674
            }
        ],
        "symptoms": [
            "overweight",
            "obesity",
            "fever",
            "cyanosis",
            "edema",
            "eye manifestations",
            "fatigue",
            "flushing"
        ]
    },
    "report": "overweight and obesity, fever with cyanosis and an edema, eye manifestations and fatigue with flushing"
}
```

###### Code d'erreurs

* 400 Bad Request
```
Missing doctor's uuid in headers
```

* 500 Internal Server Error
```
Internal Server Error
```

#### <code>POST</code> /diagnostics

Création d'un diagnostic.

##### Entrée

###### Request params

*Aucune donnée dans les headers de la requête nécéssaire*

###### Body

Nécessite l'entièreté des champs du diagnostic sous format JSON.
```JSON
{
    "uuid_doctor": "123e4567-e89b-12d3-a456-426655440002",
    "uuid_patient": null,
    "prediction": {
        "diseases": [
            {
                "disease": "Albuminuria",
                "probability": 0.14112404609491871
            },
            {
                "disease": "Pericarditis - Constrictive",
                "probability": 0.13488754115798504
            }
        ],
        "symptoms": [
            "overweight",
            "obesity"
        ]
    },
    "report": "overweight and obesity"
}
```
##### Sortie

* 200 Ok

Retourne le diagnostic créé avec notamment son id.
```JSON
{
    "id": 1349,
    "uuid_doctor": "123e4567-e89b-12d3-a456-426655440002",
    "uuid_patient": null,
    "prediction": {
        "diseases": [
            {
                "disease": "Albuminuria",
                "probability": 0.14112404609491871
            },
            {
                "disease": "Pericarditis - Constrictive",
                "probability": 0.13488754115798504
            }
        ],
        "symptoms": [
            "overweight",
            "obesity"
        ]
    },
    "report": "overweight and obesity"
}
```

###### Code d'erreurs

* 400 Bad Request
```
Invalid input format
```

* 500 Internal Server Error
```
Internal Server Error
```

#### <code>GET</code> /diagnostics

Obtention d'un diagnostic précédement créé.
- Par id: GET /diagnostics/:id
- Par queries: GET /diagnostics?uuid_doctor=34&limit=15...

##### Queries

* uuid_doctor: *sélectionne seulement les diagnostics avec l'uuid du médecin correspondant*
* uuid_patient: *sélectionne seulement les diagnostics avec l'uuid du patient correspondant*
* limit: *nombre maximum de résultat*

##### Entrée

>Token d'authentification nécessaire dans les headers

###### Request params

name         | example value        | rule     |
-------------|----------------------|----------|
token        | 90456543-gdbnj-55    | required |

###### Body

*Aucune donnée dans le body de la requête nécéssaire*

##### Sortie

* 200 Ok

Retourne une liste de diagnostics correspondant à la requête.

```JSON
[
    {
        "id": 4921,
        "uuid_doctor": "123",
        "uuid_patient": "7890",
        "prediction": {
            "diseases": [
                {
                    "disease": "Hepatomegaly",
                    "probability": 913.0
                },
                {
                    "disease": "Lymphedema",
                    "probability": 3427.0
                },
                {
                    "disease": "Paraneoplastic Endocrine Syndromes",
                    "probability": 679.0
                }
            ],
            "symptoms": [
                "Flushing",
                "Fever",
                "Obesity",
                "Overweight"
            ]
        },
        "report": "overweight and obesity, fever with cyanosis and an edema, eye manifestations and fatigue with flushing"
    },
    {
        "id": 4832,
        "uuid_doctor": "123",
        "uuid_patient": "7890",
        "prediction": {
            "diseases": [
                {
                    "disease": "Primary Ovarian Insufficiency",
                    "probability": 4760.0
                },
                {
                    "disease": "Cushing Syndrome",
                    "probability": 5493.0
                },
                {
                    "disease": "Albuminuria",
                    "probability": 3688.0
                }
            ],
            "symptoms": [
                "Flushing",
                "Fatigue",
                "Eye Manifestations",
                "Edema",
                "Cyanosis"
            ]
        },
        "report": "overweight and obesity, eye manifestations and fatigue with flushing"
    }
]
```

###### Code d'erreurs

* 400 Bad Request
```
Missing token
```
```
Invalid ID
```
```
Invalid que
```

* 403 Forbidden
```
Invalid token
```

* 404 Not Found
```
Not Found
```

* 500 Internal Server Error
```
Internal Server Error
```

#### <code>PUT</code> /diagnostics/:id

Mise à jour complète d'un diagnostic par son id.

##### Entrée

>Token d'authentification nécessaire dans les headers

###### Request params

name         | example value        | rule     |
-------------|----------------------|----------|
token        | 90456543-gdbnj-55    | required |

###### Body

Nécessite l'entièreté des champs du diagnostic sous format JSON.
```JSON
{
    "uuid_doctor": "123e4567-e89b-12d3-a456-426655440002",
    "uuid_patient": "786-yz55-792-55segb",
    "prediction": {
        "diseases": [
            {
                "disease": "Albuminuria",
                "probability": 0.14112404609491871
            },
            {
                "disease": "Pericarditis - Constrictive",
                "probability": 0.13488754115798504
            }
        ],
        "symptoms": [
            "overweight",
            "obesity"
        ]
    },
    "report": "overweight and obesity"
}
```

##### Sortie

* 200 Ok

*Aucune donnée dans le body*

###### Code d'erreurs

* 400 Bad Request
```
Missing token
```
```
Invalid input data
```

* 403 Forbidden
```
Invalid token
```

* 500 Internal Server Error
```
Internal Server Error
```

#### <code>PATCH</code> /diagnostics/:id

Mise à jour partielle d'un diagnostic par son id.

##### Entrée

>Token d'authentification nécessaire dans les headers

###### Request params

name         | example value        | rule     |
-------------|----------------------|----------|
token        | 90456543-gdbnj-55    | required |

###### Body

Envoi des champs spécifiques à modifier sous format JSON.
```JSON
{
    "report": "overweight and obesity with fever"
}
```

##### Sortie

* 200 Ok

*Aucune donnée dans le body*

###### Code d'erreurs

* 400 Bad Request
```
Missing token
```
```
Invalid input data
```

* 403 Forbidden
```
Invalid token
```

* 500 Internal Server Error
```
Internal Server Error
```

#### <code>DELETE</code> /diagnostics/:id

Supprime un diagnostic par son id.

##### Entrée

>Token d'authentification nécessaire dans les headers

###### Request params

name         | example value        | rule     |
-------------|----------------------|----------|
token        | 90456543-gdbnj-55    | required |

###### Body

*Aucune donnée dans le body de la requête nécéssaire*

##### Sortie

* 200 Ok

*Aucune donnée dans le body*

###### Code d'erreurs

* 400 Bad Request
```
Missing token
```

* 403 Forbidden
```
Invalid token
```

* 500 Internal Server Error
```
Internal Server Error
```

#### <code>POST</code> /feedbacks

Création d'un feedback.

###### Entrée (body)

Nécessite l'entièreté des champs du feedback sous format JSON.
```JSON
{
    "diagnostic_id": "4792",
    "uuid_doctor": "123",
    "text": "wrong medication",
    "diseases": [
        {
            "name": "Schizophrenia",
            "comment": "Schizophrenia should have an higher probability"
        }
    ],
    "symptoms": [
        {
            "name": "Back Pain",
            "comment": "Back Pain should not be concerned"
        }
    ]
}
```
###### Sortie

Retourne le feedback créé avec notamment son id.
```JSON
{
    "id": 4807,
    "diagnostic_id": "4792",
    "uuid_doctor": "123",
    "text": "wrong medication",
    "diseases": [
        {
            "name": "Schizophrenia",
            "comment": "Schizophrenia should have an higher probability"
        }
    ],
    "symptoms": [
        {
            "name": "Back Pain",
            "comment": "Back Pain should not be concerned"
        }
    ]
}
```

#### <code>GET</code> /feedbacks

Obtention d'un feedback précédement créé.
- Par id: GET /feedbacks/:id
- Par queries: GET /feedbacks?id_report=74201r2&disease=headache&limit=15...

###### Queries

* diagnostic_id: *sélectionne seulement les feedbacks reliés au diagnostic spécifié par l'id*
* disease: *sélectionne seulement les feedbacks reliés à la maladie précisée*
* limit: *nombre maximum de résultat*

###### Entrée

>Token d'authentification nécessaire dans les headers

*Aucune donnée dans le body de la requête nécéssaire*

###### Sortie
Retourne une liste de feedback correspondant à la requête.

```JSON
[
    {
        "id": 4829,
        "diagnostic_id": "4792",
        "uuid_doctor": "123",
        "text": "wrong medication",
        "diseases": [
            {
                "name": "Schizophrenia",
                "comment": "Schizophrenia should have an higher probability"
            }
        ],
        "symptoms": [
            {
                "name": "Back Pain",
                "comment": "Back Pain should not be concerned"
            }
        ]
    }
]
```

#### <code>PUT</code> /feedbacks/:id

Mise à jour complète d'un feedback par son id.

###### Entrée (body)

>Token d'authentification nécessaire dans les headers

Nécessite l'entièreté des champs du feedback sous format JSON.
```JSON
{
    "diagnostic_id": "4792",
    "uuid_doctor": "75t-y84z-hj8",
    "text": "wrong medication",
    "diseases": [
        {
            "name": "Schizophrenia",
            "comment": "Schizophrenia should have an higher probability"
        }
    ],
    "symptoms": [
        {
            "name": "Back Pain",
            "comment": "Back Pain should not be concerned"
        }
    ]
}
```

#### <code>PATCH</code> /feedbacks/:id

Mise à jour partielle d'un feedback par son id.

###### Entrée (body)

>Token d'authentification nécessaire dans les headers

Envoi des champs spécifiques à modifier sous format JSON.
```JSON
{
    "text": "wrong medication and patient"
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
