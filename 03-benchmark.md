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

| Option | Type | Évaluation |
|---|---|---|
| Chroma | Base vectorielle, exécutable en local | |
| FAISS | Bibliothèque de recherche vectorielle | |
| Azure AI Search | Service hébergé Microsoft | |
| pgvector | Extension d'une base relationnelle | |

### Modèle de rédaction

| Option | Type | Évaluation |
|---|---|---|
| gpt-3.5-turbo | Grand modèle de langage, accès direct | |
| gpt-4o-mini | Grand modèle de langage, génération plus récente | |
| Azure OpenAI | Mêmes modèles, hébergés dans l'environnement de l'entreprise | |
| Modèle en local | Exécution sur l'infrastructure du service | |

*Grilles à renseigner : une note par critère du 3.1.*

## 3.3 Tests réalisés

🔴 À construire. Le protocole prévu : le jeu d'évaluation décrit en [6.2](06-prototypage.md) est passé sur chaque option retenue en finale, et les résultats sont comparés à qualité et coût égaux.

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
