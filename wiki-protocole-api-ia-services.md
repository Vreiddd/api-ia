### Documentation - Protocol API-IA Services (NLP & IA)

#### Introduction

Le protocol décrit ce-dessous vise à définir la communication entre l'API et les services NLP et IA lors de la pédiction de maladies.

#### Protocol

##### Message transfert encoding

Chaque donnée envoyée doit être encaspulé par un header de 4 octets comportant la taille en octets des données qui sont transmises.
Un message est constitué du header et des données.

Si l'on souhaite transmettre l'ensemble de bytes suivant:
```
0x33 0x57 0x45 0x89
```
Les données seront encapsulé de la manière suivante pour former le message:
```
0x00 0x00 0x00 0x04 0x33 0x57 0x45 0x89
```

##### NLP Communication

###### Entrée

Envoie en raw text (bytes) du diagnostic

```
Overweight and obesity, fever with cyanosis and an edema, eye manifestations and fatigue with flushing.
```

###### Sortie

Renvoie une liste de symtômes en JSON

```JSON
["overweight", "obesity", "fever", "cyanosis", "edema", "eye manifestations", "fatigue", "flushing"]
```

##### IA Communication

###### Entrée

Envoie d'une liste de symptôme en JSON

```JSON
["overweight", "obesity", "fever", "fatigue"]
```

###### Sortie

Renvoie une liste de prédictions de maladies en JSON

```JSON
[
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
    }
]
```
