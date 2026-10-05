## Parcours 3 — Escalade vers le support de niveau 2

```mermaid
flowchart LR
    subgraph U["Utilisateur"]
        U1["Décrit son problème"]
        U2["Reçoit le numéro<br/>de ticket"]
    end
    subgraph A["Assistant"]
        A1["Détecte l'absence<br/>de réponse fiable"]
        A2["Récupère le contexte :<br/>question, passages consultés,<br/>procédure déjà tentée"]
        A3["Crée le ticket"]
    end
    subgraph N2["Support niveau 2"]
        N1["Reçoit un ticket<br/>pré-documenté"]
        N3["Traite l'incident"]
        N4["Complète la documentation<br/>si la lacune est confirmée"]
    end

    U1 --> A1 --> A2 --> A3 --> N1 --> N3
    A3 --> U2
    N3 --> N4
    N4 -.->|réindexation| A1

    classDef todo fill:#fde9ec,stroke:#a1001c,color:#5c0010;
    class A1,A2,A3,N1,N3,N4,U2 todo
```

*La boucle de retour — le technicien complète la documentation, qui est réindexée — est ce qui rend l'assistant plus pertinent au fil du temps. Elle est à construire.*
