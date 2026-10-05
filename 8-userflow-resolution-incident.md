## Parcours 1 — Résolution d'un incident (périmètre du MVP)

Utilisateur type : Jeanne, usage quotidien d'une VM standard.

```mermaid
flowchart TD
    A([Jeanne constate un incident<br/>sa VM ne démarre pas]) --> B["Elle ouvre l'assistant"]
    B --> C["Elle décrit le problème<br/>en langage courant"]
    C --> D["L'assistant recherche<br/>dans la documentation"]
    D --> E{"Un passage pertinent<br/>est-il trouvé ?"}

    E -->|Oui| F["Réponse rédigée<br/>+ sources citées"]
    E -->|Non| G["« Je ne trouve pas<br/>de procédure »"]

    F --> H["Jeanne applique<br/>la procédure"]
    H --> I{"Incident résolu ?"}
    I -->|Oui| J([Fin — incident résolu<br/>sans ticket])
    I -->|Non| K

    G --> K["Proposition de transmettre<br/>au support de niveau 2"]
    K --> L{"Jeanne accepte ?"}
    L -->|Non| M([Fin — abandon])
    L -->|Oui| N["Ticket créé<br/>avec l'historique de l'échange"]
    N --> O([Prise en charge par<br/>un technicien N2])

    classDef ok fill:#e4f3e8,stroke:#1a6b2f,color:#143d1f;
    classDef todo fill:#fde9ec,stroke:#a1001c,color:#5c0010;
    class B,C,D,F,G,H ok
    class K,N,O todo
```

*En vert : les étapes opérationnelles dans le prototype. En rouge : l'escalade vers le niveau 2, à construire.*
