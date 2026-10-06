[← Accueil](README.md)

# 5. Planification du développement

## 5.1 Backlog produit priorisé

Priorisation MoSCoW appliquée au MVP, limité à la résolution d'incident.

| Priorité | Fonctionnalité | Statut |
|---|---|---|
| Doit avoir | Saisie en langage naturel et réponse rédigée | 🟢 Vérifié |
| Doit avoir | Recherche préalable dans la documentation | 🟢 Vérifié |
| Doit avoir | Affichage des sources et du taux de correspondance | 🟢 Vérifié |
| Doit avoir | Réponse « je ne sais pas » quand aucun passage ne correspond | 🔴 À construire |
| Doit avoir | Création automatique d'un ticket vers le niveau 2 | 🔴 À construire |
| Doit avoir | Indexation automatique de la documentation SharePoint | 🔴 À construire |
| Devrait avoir | Accès depuis le catalogue des services numériques/Sharepoint de l'offre | 🔴 À construire |
| Devrait avoir | Historique des échanges par utilisateur | 🔴 À construire |
| Pourrait avoir | Tableau de bord d'usage pour le gestionnaire de parc | 🔴 À construire |
| N'aura pas (MVP) | Recherche d'information sur l'offre | Reporté en v2 |
| N'aura pas (MVP) | Connexion à la facturation | Reporté en v2 |

**Règle de priorisation retenue** : le MVP est limité à la résolution d'incident, qui concentre l'essentiel des pertes identifiées. Traiter les deux usages de front rendrait impossible l'imputation des résultats obtenus.

## 5.2 Calendrier de développement

Voir le diagramme de jalons en [8.2](08-plan-de-release.md).

## 5.3 Jalons et dépendances

| Jalon | Condition préalable |
|---|---|
| Mesure de la pertinence | Jeu d'évaluation constitué |
| Passage en bêta | Accès Azure obtenu, base vectorielle hébergée |
| Ouverture aux pilotes | Documentation réelle indexée, réponse « je ne sais pas » en place |
| Passage en v1 | Résultats de la bêta mesurés, création de ticket opérationnelle |

**Dépendance critique** : l'obtention d'un accès Azure conditionne tout le reste. Tant qu'elle n'est pas levée, le prototype ne peut pas quitter le poste de travail.

🔴 Ce backlog et ces jalons constituent une proposition, à valider avec le service PPV.
