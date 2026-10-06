[← Accueil](README.md)

# 10. Diagramme de classes

## Modèle objet de l'assistant PPV

Dix classes, correspondant au périmètre du prototype et à l'escalade prévue vers ServiceNow. Les classes en vert existent dans le code 🟢, celles en rouge restent à construire 🔴.

```mermaid
classDiagram
    direction LR

    class Utilisateur {
        +String identifiant
        +String profil
        +String typeVM
        +poserQuestion(texte) Echange
    }

    class Conversation {
        +String identifiant
        +Date ouverteLe
        +ajouterEchange(echange)
        +exporterHistorique() String
    }

    class Echange {
        +String question
        +String reponse
        +Date horodatage
        +Float scoreConfiance
        +Float coutEnEuros
        +estFiable() Boolean
    }

    class SourceCitee {
        +String titreDocument
        +Float tauxCorrespondance
        +String extrait
    }

    class Document {
        +String identifiant
        +String titre
        +String emplacement
        +Date dateMiseAJour
        +decouper(taille, chevauchement) List~MorceauDocument~
    }

    class MorceauDocument {
        +String identifiant
        +String texte
        +Integer position
        +Vecteur empreinte
    }

    class BaseVectorielle {
        +String emplacement
        +Integer nombreMorceaux
        +indexer(morceaux)
        +rechercher(empreinte, nombre) List~MorceauDocument~
    }

    class ServiceRAG {
        +Integer nombrePassages
        +Float seuilConfiance
        +traiterQuestion(texte) Echange
        +construireContexte(morceaux) String
    }

    class ModeleIA {
        +String nom
        +String fournisseur
        +Float coutParMillionMots
        +vectoriser(texte) Vecteur
        +redigerReponse(question, contexte) String
    }

    class Ticket {
        +String numero
        +String statut
        +String contexteTransmis
        +creerDepuis(conversation) Ticket
    }

    Utilisateur "1" --> "*" Conversation : ouvre
    Conversation "1" *-- "*" Echange : contient
    Echange "1" o-- "*" SourceCitee : cite
    SourceCitee "*" --> "1" Document : provient de
    Document "1" *-- "*" MorceauDocument : est decoupe en
    BaseVectorielle "1" o-- "*" MorceauDocument : indexe
    ServiceRAG "1" --> "1" BaseVectorielle : interroge
    ServiceRAG "1" --> "1" ModeleIA : sollicite
    ServiceRAG "1" ..> "*" Echange : produit
    Conversation "1" ..> "0..1" Ticket : escalade vers

    note for ServiceRAG "Classe centrale du prototype :\nenchaine recherche puis redaction"
    note for Ticket "Escalade via l'API ServiceNow\nnon implementee dans le prototype"
```

## Ce que montre le modèle

| Relation | Lecture |
|---|---|
| `Conversation` ◆— `Echange` | Composition : un échange n'existe pas hors de sa conversation. Supprimer la conversation supprime les échanges |
| `Echange` ◇— `SourceCitee` | Agrégation : les sources existent indépendamment, elles sont seulement rattachées à la réponse |
| `Document` ◆— `MorceauDocument` | Composition : les morceaux sont produits par le découpage du document (1 000 caractères, 200 de chevauchement) |
| `BaseVectorielle` ◇— `MorceauDocument` | Agrégation : la base indexe les morceaux sans les posséder — c'est ce qui permet de réindexer sans tout reconstruire |
| `ServiceRAG` → `BaseVectorielle` et `ModeleIA` | Les deux dépendances qui portent la démonstration de faisabilité |

## Correspondance avec le code

| Classe | Fichier du dépôt | Statut |
|---|---|---|
| `ServiceRAG` | `rag-service.js` | 🟢 Vérifié |
| `BaseVectorielle` | Chroma, conteneur Docker | 🟢 Vérifié |
| `Document`, `MorceauDocument` | `vectorize.py` | 🟢 Vérifié |
| `Echange`, `SourceCitee` | `server-fixed.js` | 🟢 Vérifié |
| `ModeleIA` | API OpenAI | 🟢 Vérifié |
| `Utilisateur`, `Conversation` | Pas de persistance dans le prototype | 🔴 À construire |
| `Ticket` | API ServiceNow | 🔴 À construire |

> `scoreConfiance` et `estFiable()` matérialisent le seuil en dessous duquel l'assistant doit refuser de répondre et proposer l'escalade. C'est le point dur identifié en itération 2 : un modèle de langage répond avec assurance même sans information pertinente.
