## Parcours 2 — Recherche d'information sur l'offre (v2)

Utilisateur type : Louis, usage occasionnel d'une VM clone.

```mermaid
flowchart TD
    A([Louis s'interroge sur la pertinence de son parc de VM Clone, vaut-il mieux des VM Standard ?]) --> B["Il pose sa question<br/>à l'assistant"]
    B --> C["L'assistant recherche dans la documentation<br/>tarifaire et commerciale"]
    C --> D{"Information<br/>trouvée ?"}

    D -->|Oui| E["Réponse + source<br/>+ date de mise à jour du document"]
    D -->|Non| F["Orientation vers<br/>le gestionnaire de parc"]

    E --> G{"Besoin complémentaire<br/>détecté ?"}
    G -->|Non| H([Fin — information obtenue])
    G -->|"Oui, usage inadapté"| I["Suggestion de réorientation<br/>vers une VM standard"]
    I --> J([Mise en relation<br/>avec le gestionnaire])
    F --> J

    classDef ok fill:#e4f3e8,stroke:#1a6b2f,color:#143d1f;
    classDef todo fill:#fde9ec,stroke:#a1001c,color:#5c0010;
    class B,C,E ok
    class F,I,J todo
```

*Ce parcours n'entre pas dans le MVP : il est ajouté en v2, une fois la résolution d'incident mesurée.*
