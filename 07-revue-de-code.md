[← Accueil](README.md)

# 7. Revue de code

## 7.1 Processus retenu

```mermaid
flowchart LR
    A["Branche dédiée<br/>par évolution"] --> B["Demande de fusion<br/>(pull request)"]
    B --> C1["Outils automatiques<br/>style · failles · tests"]
    B --> C2["Relecture assistée par IA<br/>consigne versionnée"]
    B --> C3["Relecture humaine<br/>développeur identifié"]
    C1 & C2 & C3 --> D[["Registre des constats<br/>criticité · décision · suite"]]
    D --> E{Décision}
    E -->|Corrigé| F["Commit correctif<br/>puis fusion"]
    E -->|Écarté| G["Justification<br/>dette technique"]
    F --> H(["Code intégré"])
    G --> H

    classDef src fill:#fff2e0,stroke:#9a5c00,color:#4a2d00;
    class C1,C2,C3 src
```

## 7.2 Grille de relecture

Sept critères arrêtés avant la première revue, notés de 0 à 2 : lisibilité et nommage, gestion des erreurs, protection des clés d'accès, couverture de tests, temps de réponse, sécurité, conformité RGPD.

## 7.3 Registre des constats

| N° | Élément | Criticité | Constat | Source | Décision | Corrigé |
|---|---|---|---|---|---|---|
| | | | | | | |

Les constats écartés sont consignés au même titre que les constats corrigés, avec leur justification.

🔴 Dispositif à mettre en place sur les évolutions restantes.
