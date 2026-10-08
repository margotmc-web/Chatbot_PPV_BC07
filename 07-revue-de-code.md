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

## 7.2 Outils et périodicité

| Élément | Choix |
|---|---|
| Outils automatiques | ESLint (JavaScript), Ruff (Python), npm audit (vulnérabilités des dépendances), analyse des secrets GitHub, tests |
| Relecture assistée par IA | Consigne versionnée dans le dépôt |
| Relecture humaine | Développeur identifié |
| Périodicité | À chaque demande de fusion, plus une revue collective de 30 min en fin de sprint |

## 7.3 Grille de relecture

Sept critères arrêtés avant la première revue, notés de 0 (non conforme) à 2 (conforme).

## 7.4 Revue n° 1 — Traitement d'une question

Périmètre : `server-fixed.js` (serveur en service), `vectorize.py` (indexation), `rag-service.js` (module de recherche), `package.json` (dépendances).

| Critère | Note | Justification |
|---|---|---|
| Lisibilité et nommage | 1 | Code commenté et structuré ; mais quatre versions du serveur et cinq scripts d'indexation coexistent |
| Gestion des erreurs | 1 | Erreurs interceptées, mais ni délai maximal ni nouvelle tentative sur les appels à l'IA |
| Protection des clés d'accès | 2 | Variables d'environnement, `.env` exclu du dépôt, `.env.example` sans clé réelle |
| Couverture de tests | 1 | Les routes de l'API sont testées, ni la recherche ni les cas limites |
| Temps de réponse | 1 | Contexte limité à 3 passages, mais aucun délai maximal sur les appels externes |
| Sécurité | 0 | Requêtes acceptées depuis n'importe quel site ; messages du navigateur transmis sans contrôle |
| Conformité RGPD | 1 | Aucune conversation stockée ; questions envoyées à OpenAI en accès direct (choix assumé du prototype) |
| **Total** | **7 / 14** | Faisabilité démontrée, corrections nécessaires avant la bêta |

## 7.5 Registre des constats

| N° | Élément | Criticité | Constat | Action recommandée | Décision |
|---|---|---|---|---|---|
| 1 | `vectorize.py` | Haute | Découpage à chaque point : les prix décimaux sont coupés (« 0.50€ » → « coûtent 0 » / « 50€/heure ») | Découpage par taille (1 000 caractères, chevauchement 200) | À corriger avant la bêta |
| 2 | `vectorize.py` | Haute | Seules les 3 premières phrases de chaque document sont indexées | Supprimer la limite | À corriger avant la bêta |
| 3 | `server-fixed.js` | Haute | Aucun seuil de pertinence ; la consigne ne demande pas de refuser sans passage pertinent | Seuil + réponse « je ne sais pas » + consigne « uniquement à partir des documents » | À corriger avant la bêta |
| 4 | `server-fixed.js` | Haute | Historique du navigateur transmis tel quel à l'IA (risque d'injection de consignes) | Ne garder que les rôles utilisateur et assistant, limiter la longueur | À corriger avant la bêta |
| 5 | `server-fixed.js` | Moyenne | Requêtes acceptées depuis n'importe quel site (`origin: '*'`) | Restreindre via `CORS_ORIGIN` | Accepté en local, à corriger avant la bêta |
| 6 | `server-fixed.js` | Moyenne | Pas de délai maximal ni de nouvelle tentative, contrairement au diagramme de séquence | Délai 10 s, une nouvelle tentative après 2 s, puis passages bruts | À corriger |
| 7 | `server-fixed.js` | Moyenne | Température 0,7, trop créative pour du support | Abaisser entre 0 et 0,2 | À corriger |
| 8 | `package.json` | Moyenne | `npm start` lance `server-rag.js` (syntaxe `import` non reconnue) ; deux chaînes avec des collections différentes (`ppv` / `ppv_documents`) | Une seule chaîne, fichiers obsolètes supprimés | À corriger |
| 9 | `rag-service.js` | Moyenne | Distance Chroma affichée comme pourcentage de correspondance | Convertir en similarité (1 − distance) | À vérifier puis corriger |
| 10 | `package.json` | Faible | `chroma-js` est une bibliothèque de couleurs ; `@xenova/transformers` est un vestige de l'itération 3 | Retirer ces dépendances | À corriger |
| 11 | `server-fixed.js` | Faible | Consigne « [Ref 1] » vs documents étiquetés « [Document 1] » | Harmoniser | À corriger |
| 12 | `server-fixed.js` | Faible | Erreurs internes renvoyées au navigateur ; clé non vérifiée au démarrage | Message générique, contrôle au démarrage | Dette technique (v1) |

## 7.6 Retours au développeur

- **Points forts** : séparation claire recherche / contexte / rédaction ; clés protégées ; code commenté ; supervision (`/api/health`) et script de test en place.
- **Points d'amélioration** : fiabiliser l'indexation (constats 1-2) ; empêcher toute réponse sans source pertinente (3) ; sécuriser les entrées (4-5).
- **Priorité du premier sprint de bêta** : constats 1 à 5.
- **Bonne pratique** : un test automatique par constat corrigé (ex. : un prix décimal reste intact après indexation).
