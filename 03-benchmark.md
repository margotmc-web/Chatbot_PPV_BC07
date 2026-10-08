[← Accueil](README.md)

# 3. Benchmark des solutions

## 3.1 Critères d'évaluation retenus

Six critères, pondérés selon les contraintes du service PPV.

| Critère | Ce qu'il mesure | Poids |
|---|---|---|
| Coût à l'usage | Coût par question, projeté sur 1 500 utilisateurs | Fort |
| Souveraineté des données | La documentation interne sort-elle du périmètre de l'entreprise ? | Fort |
| Qualité en français | Pertinence sur un vocabulaire métier francophone | Fort |
| Facilité d'intégration | Effort pour relier la brique au reste de la chaîne | Moyen |
| Maturité | Stabilité, documentation disponible, communauté | Moyen |
| Réversibilité | Possibilité de changer de brique sans tout reconstruire | Moyen |

## 3.2 Solutions comparées

### Approche générale

| Option | Principe | Retenue |
|---|---|---|
| Chatbot à scénarios | Arbres de décision écrits à la main | Non |
| Réentraînement d'un modèle | Spécialiser un modèle sur la documentation PPV | Non |
| Recherche documentaire préalable (RAG) | Chercher les passages utiles, puis faire rédiger | **Oui** |

### Base de recherche

Notes de 1 à 5, pondérées (poids fort ×3, moyen ×2). Le critère « qualité en français » relève du modèle et ne s'applique pas ici (maximum : 60).

| Critère (poids) | Chroma | FAISS | Azure AI Search | pgvector |
|---|---|---|---|---|
| Coût (×3) | 5 — gratuit, open source | 5 — gratuit | 2 — service payant | 4 — gratuit, mais base PostgreSQL à exploiter |
| Souveraineté (×3) | 5 — exécution locale | 5 — exécution locale | 4 — hébergé dans l'environnement Microsoft de l'entreprise | 5 — exécution locale |
| Intégration (×2) | 4 — serveur prêt à l'emploi | 2 — simple bibliothèque, serveur à développer | 5 — natif dans l'écosystème SNCF | 3 — base PostgreSQL absente du service |
| Maturité (×2) | 3 — outil récent, orienté prototypage | 4 — éprouvé (Meta) | 5 — service d'entreprise | 4 — extension reconnue |
| Réversibilité (×2) | 4 | 3 | 2 — propre à Microsoft | 4 |
| **Score pondéré** | **52 / 60** | 48 / 60 | 42 / 60 | 49 / 60 |

**Décision en deux temps** : Chroma pour le prototype (gratuite, locale, rapide à installer) ; Azure AI Search pour la production, où l'hébergement, la supervision et l'intégration à l'écosystème Microsoft deviennent prioritaires.

### Modèle de rédaction

Maximum : 75.

| Critère (poids) | gpt-3.5-turbo (direct) | gpt-4o-mini (direct) | Azure OpenAI | Modèle en local |
|---|---|---|---|---|
| Coût (×3) | 4 | 5 — plus récent, tarif inférieur | 4 — mêmes modèles + infrastructure | 3 — gratuit à l'usage, serveur puissant nécessaire |
| Souveraineté (×3) | 2 — données hors de l'entreprise | 2 — idem | 4 — environnement de l'entreprise, région UE | 5 — aucune donnée transmise |
| Qualité en français (×3) | 3 | 4 | 4 | 2 — modèles légers moins performants |
| Intégration (×2) | 5 | 5 | 5 — authentification de l'entreprise | 2 — échec constaté (itération 3) |
| Maturité (×2) | 4 — génération ancienne | 4 | 5 | 3 |
| Réversibilité (×2) | 4 | 4 | 3 | 4 |
| **Score pondéré** | 53 / 75 | 59 / 75 | **62 / 75** | 48 / 75 |

**Décision** : gpt-3.5-turbo en accès direct pour le prototype (choix subi, faute d'accès Azure) ; Azure OpenAI en production, avec un modèle de génération plus récente comme gpt-4o-mini.

**Sources mobilisées** : documentation officielle des éditeurs (OpenAI, Microsoft Learn, Chroma, FAISS, pgvector), retours de la communauté technique (dépôts GitHub, signalements de problèmes), tests réalisés au cours du prototypage.

## 3.3 Tests réalisés

| Test | Comparaison | Résultat | Statut |
|---|---|---|---|
| IA interrogée directement vs avec recherche préalable | Itération 1 vs itération 2 | Coût par question 0,05 € → 0,0003 € ; temps ≈ 10 s → < 1 s | 🟡 Estimé |
| Préparation documentaire en JavaScript vs en Python | Itération 2 | Échec répété en JavaScript, succès en Python | 🟢 Vérifié |
| Vectorisation externe vs en local | Itération 3 | Migration non aboutie, vectorisation externe conservée | 🟢 Vérifié |

🔴 Test restant : passer le [jeu d'évaluation](15-jeu-evaluation.md) décrit en [6.2](06-prototypage.md) sur les options retenues en finale, et comparer les résultats à qualité et coût égaux.

## 3.4 Décisions prises et justifications

Les choix suivants ont été arrêtés en cours de prototypage, avant formalisation du benchmark. Leurs justifications sont réelles et vérifiables ; la comparaison chiffrée reste à produire.

| Brique | Choix | Justification |
|---|---|---|
| Approche générale | Recherche documentaire préalable | Seule option qui réduit le coût sans dégrader la couverture, et qui permet de citer les sources |
| Base de recherche | Chroma | Gratuite, s'exécute localement, la documentation ne sort pas de la machine 🟢 |
| Modèle de rédaction | gpt-3.5-turbo | Coût minimal adapté à un prototype ; un modèle plus récent est à retenir en production |
| Modèle de vectorisation | text-embedding-3-small | Le plus économique de sa catégorie, suffisant pour du français courant |
| Hébergement | OpenAI en accès direct | Choix subi : aucun accès Azure dans le cadre de l'alternance. Azure OpenAI reste la cible |

**Une alternative a été testée puis écartée** : l'exécution du modèle de vectorisation en local, pour supprimer toute dépendance externe. La tentative n'a pas abouti — voir [itération 3](06-prototypage.md).
