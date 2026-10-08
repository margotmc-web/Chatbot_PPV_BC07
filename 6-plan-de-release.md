## Plan de release

```mermaid
gantt
    title Versions et jalons (S1 = semaine du 12 octobre 2026)
    dateFormat YYYY-MM-DD
    axisFormat %d/%m
    section Prototype
    v0.1 faisabilité démontrée               :done, p1, 2026-08-01, 2026-10-10
    section Beta (S1-S8)
    Cadrage, achats, environnements (S1-S4)  :b1, 2026-10-12, 2026-11-07
    3 sprints de développement (S5-S7)       :b2, 2026-11-09, 2026-11-28
    Tests avec les utilisateurs pilotes (S8) :b3, 2026-11-30, 2026-12-05
    section v1 MVP (S9-S10)
    Conformité RGPD et sécurité (S9)         :v1, 2026-12-07, 2026-12-12
    Ouverture aux utilisateurs (S10)         :v1b, 2026-12-14, 2026-12-19
    Jalon MVP - GO/NOGO                      :milestone, m1, 2026-12-18, 0d
    section v2 (S11-S20)
    Spécification et environnements Azure (S11-S12) :w1, 2026-12-21, 2027-01-02
    3 sprints de développement (S13-S15)     :w2, 2027-01-04, 2027-01-23
    Tests de charge et recette (S16)         :w3, 2027-01-25, 2027-01-30
    Sécurité, formation, activation (S17-S19):w4, 2027-02-01, 2027-02-20
    Clôture (S20)                            :w5, 2027-02-22, 2027-02-27
    Mise en production                       :milestone, m2, 2027-02-26, 0d
```

*Le calendrier suit le planning du projet en 20 semaines (dossier principal, Bloc 3) : S1 commence le 12 octobre 2026, à l'issue du prototype ; la mise en production intervient fin février 2027, dans l'échéance du premier trimestre fixée par le commanditaire.*
