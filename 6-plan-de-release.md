## Plan de release

```mermaid
gantt
    title Versions et jalons
    dateFormat YYYY-MM-DD
    axisFormat %b
    section Prototype
    v0.1 — faisabilité démontrée      :done, p1, 2026-08-01, 2026-10-01
    section Bêta
    Hébergement entreprise            :active, b1, 2026-10-01, 45d
    10 à 15 utilisateurs pilotes      :b2, after b1, 30d
    Mesure de la pertinence           :b3, after b1, 30d
    section v1 — MVP
    Résolution d'incident             :v1, after b2, 60d
    Intégration SharePoint et catalogue :v1b, after b2, 45d
    section v2
    Recherche d'information sur l'offre :v2, after v1, 60d
```

*Dates à ajuster.*
