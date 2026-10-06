[← Accueil](README.md)

# 8. Plan de release

## 8.1 Versions et critères de passage

| Version | Périmètre | Condition de passage | Statut |
|---|---|---|---|
| v0.1 — Prototype | 4 documents, un utilisateur, exécution locale | Faisabilité démontrée | 🟢 Vérifié |
| Bêta | 10 à 15 utilisateurs pilotes, hébergement entreprise | Pertinence mesurée | 🔴 À construire |
| v1 — MVP | Résolution d'incident, accès Sharepoint de l'offre/Catalogue des services numériques, ticket automatique | Baisse des sollicitations N1 | 🔴 À construire |
| v2 | Recherche d'information sur l'offre | — | 🔴 À construire |

## 8.2 Calendrier

```mermaid
gantt
    title Versions et jalons
    dateFormat YYYY-MM-DD
    axisFormat %b
    section Prototype
    v0.1 faisabilite demontree        :done, p1, 2026-08-01, 2026-10-01
    section Beta
    Hebergement entreprise            :active, b1, 2026-10-01, 45d
    Utilisateurs pilotes              :b2, after b1, 30d
    Mesure de la pertinence           :b3, after b1, 30d
    section v1 MVP
    Resolution d'incident             :v1, after b2, 60d
    Integration Sharepoint de l'offre/Catalogue des services numériques                 :v1b, after b2, 45d
    section v2
    Recherche d'information           :v2, after v1, 60d
```

Dates à ajuster.
