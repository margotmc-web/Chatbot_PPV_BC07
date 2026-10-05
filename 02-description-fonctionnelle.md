[← Accueil](README.md)

# 2. Description fonctionnelle

## 2.1 Expérience utilisateur visée

*À compléter.*

## 2.2 Parcours 1 — Résolution d'un incident

```mermaid
flowchart TD
    A([Incident constaté]) --> B["L'utilisateur décrit<br/>le problème"]
    B --> C["Recherche dans<br/>la documentation"]
    C --> D{"Passage<br/>trouvé ?"}
    D -->|Oui| E["Réponse rédigée<br/>+ sources citées"]
    D -->|Non| G["Pas de procédure<br/>disponible"]
    E --> H{"Incident<br/>résolu ?"}
    H -->|Oui| I([Fin — sans ticket])
    H -->|Non| K["Transmission<br/>au niveau 2"]
    G --> K
    K --> L([Ticket créé avec<br/>l'historique de l'échange])

    classDef ok fill:#e4f3e8,stroke:#1a6b2f,color:#143d1f;
    classDef todo fill:#fde9ec,stroke:#a1001c,color:#5c0010;
    class B,C,E,G ok
    class K,L todo
```

En vert, ce qui fonctionne dans le prototype 🟢. En rouge, l'escalade vers le niveau 2 🔴.

## 2.3 Parcours 2 — Recherche d'information sur l'offre

```mermaid
flowchart TD
    A([Question sur l'offre<br/>coût, options, éligibilité]) --> B["Recherche dans la documentation<br/>tarifaire et commerciale"]
    B --> C{"Information<br/>trouvée ?"}
    C -->|Oui| D["Réponse + source<br/>+ date du document"]
    C -->|Non| E["Orientation vers<br/>le gestionnaire de parc"]
    D --> F{"Usage<br/>inadapté ?"}
    F -->|Non| G([Fin — information obtenue])
    F -->|Oui| H["Suggestion de réorientation<br/>vers une VM standard"]
    H --> I([Mise en relation])
    E --> I

    classDef ok fill:#e4f3e8,stroke:#1a6b2f,color:#143d1f;
    classDef todo fill:#fde9ec,stroke:#a1001c,color:#5c0010;
    class B,D ok
    class E,H,I todo
```

Ce parcours n'entre pas dans le MVP : il est ajouté en v2.
