## Parcours 4 — Gestionnaire du parc

Utilisateur type : Léa, gestionnaire du parc de machines virtuelles.

```mermaid
flowchart TD
    A([Léa veut suivre<br/>l'usage de l'assistant]) --> B["Tableau de bord"]
    B --> C["Volume de questions<br/>par thème"]
    B --> D["Taux de résolution<br/>sans ticket"]
    B --> E["Questions sans réponse<br/>trouvée"]

    C --> F{"Un thème<br/>surreprésenté ?"}
    F -->|Oui| G["Documentation à clarifier<br/>ou offre à expliquer"]
    E --> H["Lacune documentaire<br/>identifiée"]
    H --> I["Rédaction ou mise à jour<br/>d'une procédure"]
    G --> I
    I --> J["Réindexation<br/>de la base documentaire"]
    J --> K([L'assistant répond<br/>à ces questions])

    D --> L{"Taux en baisse ?"}
    L -->|Oui| M["Alerte : qualité<br/>des réponses à vérifier"]

    classDef todo fill:#fde9ec,stroke:#a1001c,color:#5c0010;
    class B,C,D,E,G,H,I,J,K,M todo
```

*Ce parcours est entièrement à construire. Il conditionne pourtant la mesure des résultats : sans lui, aucun des critères de passage de version du plan de release n'est observable.*
