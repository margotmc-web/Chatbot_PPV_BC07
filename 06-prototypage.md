[← Accueil](README.md)

# 6. Prototypage et itérations

## 6.1 Découpage des itérations

```mermaid
flowchart TD
    I0["Itération 0<br/>Maquette d'écrans"] --> R0{Parcours validé}
    R0 --> I1["Itération 1<br/>IA connectée, sans recherche"]
    I1 --> R1{"Coût trop élevé<br/>à l'échelle"}
    R1 --> I2["Itération 2<br/>Recherche documentaire"]
    I2 --> R2{"3 blocages :<br/>dépendances, indexation,<br/>communication"}
    R2 --> F2["Correctif : indexation<br/>basculée en Python"]
    F2 --> I3["Itération 3<br/>Modèle en local"]
    I3 --> R3{"Migration<br/>non aboutie"}
    R3 --> OUT["Faisabilité non démontrée<br/>piste reportée en production"]

    classDef ok fill:#e4f3e8,stroke:#1a6b2f,color:#143d1f;
    classDef ko fill:#fde9ec,stroke:#a1001c,color:#5c0010;
    classDef neutre fill:#eef2f7,stroke:#003366,color:#002244;
    class I0,I1,I2,F2 ok
    class I3,R3,OUT ko
    class R0,R1,R2 neutre
```

## 6.2 Résultats et statut des indicateurs

| Indicateur | Valeur | Statut |
|---|---|---|
| Coût par question | 0,05 € sans recherche, 0,0003 € avec | 🟡 Estimé |
| Temps de réponse | ≈ 10 s sans recherche, < 1 s avec | 🟡 Estimé |
| Pertinence des réponses | Objectif visé : 95 % | 🔴 À mesurer |
| Indexation documentaire | 4 documents, 128 morceaux | 🟢 Vérifié |

## 6.3 Faisabilité non démontrée

L'itération 3 constitue une non-faisabilité constatée dans les conditions du prototype. Les attendus du bloc demandent de démontrer la faisabilité **ou non** des briques les plus complexes : une piste écartée pour des raisons argumentées est un résultat de prototypage, pas un échec de projet.
