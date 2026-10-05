[← Accueil](README.md)

# 4. Architecture technique

## 4.1 Le principe retenu

```mermaid
flowchart LR
    subgraph AV["Sans recherche préalable"]
        direction TB
        A1["Question"] --> A2["Toute la documentation<br/>envoyée à l'IA"] --> A3["Réponse<br/>5 000 mots transmis"]
    end
    subgraph AP["Avec recherche préalable"]
        direction TB
        B1["Question"] --> B2["Recherche :<br/>3 passages utiles"] --> B3["Ces 3 passages<br/>envoyés à l'IA"] --> B4["Réponse + sources<br/>300 mots transmis"]
    end
    AV ~~~ AP

    classDef ko fill:#fde9ec,stroke:#a1001c,color:#5c0010;
    classDef ok fill:#e4f3e8,stroke:#1a6b2f,color:#143d1f;
    class A2,A3 ko
    class B2,B3,B4 ok
```

Le coût par question passe de 0,05 € à 0,0003 € 🟡 — calcul à partir du volume transmis.

## 4.2 Prototype et architecture cible

```mermaid
flowchart LR
    subgraph P["PROTOTYPE — aujourd'hui"]
        direction TB
        subgraph PF["FRONTEND"]
            P1["Chatbot VM<br/>React, navigateur"]
        end
        subgraph PB["BACKEND"]
            P2["Appli backend<br/>Node.js / Express"]
        end
        subgraph PD["BDD"]
            direction LR
            P3["4 documents<br/>de référence"]
            P4[("Base vectorielle<br/>Docker, poste local")]
        end
        subgraph PI["MODELE IA"]
            P5["OpenAI<br/>accès direct"]
        end
        P1 <--> P2
        P2 <-->|1. recherche| P4
        P2 <-->|2. rédaction| P5
        P3 -.->|indexation manuelle| P4
    end

    subgraph C["CIBLE — architecture préconisée"]
        direction TB
        subgraph CF["FRONTEND"]
            direction LR
            C1["Chatbot VM"]
            C2["Connexion utilisateur<br/>via SharePoint"]
        end
        subgraph CB["BACKEND"]
            C3["Appli backend<br/>hébergée et supervisée"]
        end
        subgraph CD["BDD"]
            direction LR
            C4["SharePoint<br/>de l'offre"]
            C5["PBI de<br/>facturation"]
            C6["Historique<br/>des échanges"]
            C7[("Base vectorielle<br/>service hébergé")]
        end
        subgraph CI["MODELE IA"]
            C8["Azure OpenAI"]
        end
        C1 <-->|API| C2
        C1 <--> C3
        C3 <--> C4
        C3 <--> C5
        C3 <--> C6
        C3 <-->|1. recherche| C7
        C3 <-->|2. rédaction| C8
        C4 -.->|indexation automatique| C7
    end

    P ~~~ C

    classDef bloc fill:#fff,stroke:#9aa0b5,color:#1a1a2e;
    classDef vect fill:#e4f3e8,stroke:#1a6b2f,color:#143d1f;
    classDef ia fill:#c4203f,stroke:#8f1026,color:#fff;
    class P1,P2,P3,C1,C2,C3,C4,C5,C6 bloc
    class P4,C7 vect
    class P5,C8 ia
```

M�me structure des deux côtés : seule change la nature de chaque brique. La création de ticket n'existe que dans la cible.

## 4.3 Les technologies, par couche

| Couche | Technologie | Type | Rôle |
|---|---|---|---|
| Interface | React | Bibliothèque d'interface | Les écrans affichés dans le navigateur |
| Serveur | Node.js / Express | Environnement d'exécution et cadre serveur | Reçoit la question, orchestre, renvoie la réponse |
| Recherche | Chroma | Base de données vectorielle | Recherche par le sens et non par mots-clés |
| Recherche | Python | Langage de programmation | Préparation et indexation des documents |
| IA | text-embedding-3-small | Modèle de vectorisation | Traduit un texte en nombres représentant son sens |
| IA | gpt-3.5-turbo | Grand modèle de langage | Rédige la réponse à partir des passages reçus |
| Outillage | Docker | Conteneurisation | Environnement reproductible |
| Outillage | Git / GitHub | Gestion de versions | Historique du code et support des relectures |

> Le prototype fonctionne avec OpenAI en accès direct, alors que l'architecture préconisée retient Azure OpenAI. Cet écart s'explique par l'absence d'accès Azure dans le cadre de l'alternance ; la brique testée est identique dans les deux cas.
