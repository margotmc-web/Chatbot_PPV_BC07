[← Accueil](README.md)

# 17. Jeu d'évaluation de la pertinence

Ce jeu de 30 questions sert à mesurer la pertinence de l'assistant de façon reproductible (protocole décrit en [6.2](06-prototypage.md)). Il conditionne le passage en bêta (voir [plan de release](08-plan-de-release.md)).

## 17.1 D'où viennent les questions

Les 30 questions ne sont pas inventées : elles reformulent, en langage courant, **les 30 dernières demandes adressées au support PPV** (16 au 30 juin 2026, annexe 2.5 du dossier principal). Le jeu reflète donc la réalité du support :

| Couverture par la documentation SharePoint | Nombre | Part |
|---|---|---|
| Réponse documentée (Oui) | 21 | 70 % |
| Réponse partielle (Partiel) | 5 | 17 % |
| Aucune réponse documentée (Non) | 4 | 13 % |

Les questions sans réponse documentée sont volontairement conservées : elles testent la capacité de l'assistant à **refuser de répondre** plutôt qu'à inventer une procédure (constat 3 de la [revue de code](07-revue-de-code.md)).

## 17.2 Les 30 questions

| N° | Catégorie | Question posée à l'assistant | Doc. SharePoint | Comportement attendu | Périmètre |
|---|---|---|---|---|---|
| 1 | Connexion VPN | Je n'arrive pas à me connecter à ma VM, que faire ? | Oui | Réponse complète, procédure citée avec sa source | MVP |
| 2 | Authentification SSO | J'ai un message d'erreur SSO quand je me connecte. | Partiel | Réponse partielle avec sa source, puis proposition d'escalade et ticket pré-rédigé | MVP |
| 3 | Accès / droits | Comment vérifier qu'un nouvel utilisateur a bien accès à ses 3 VM ? | Oui | Réponse complète, procédure citée avec sa source | MVP |
| 4 | Lenteurs VM | Ma VM est très lente ce matin. | Oui | Réponse complète, procédure citée avec sa source | MVP |
| 5 | Redémarrage VM | Comment redémarrer ma VM ? | Oui | Réponse complète, procédure citée avec sa source | MVP |
| 6 | Facturation / devis | Combien coûte une VM Clone par mois, et faut-il la budgéter différemment ? | Oui | Réponse complète, procédure citée avec sa source | v2 |
| 7 | Authentification SSO | L'authentification à double facteur ne fonctionne pas. | Partiel | Réponse partielle avec sa source, puis proposition d'escalade et ticket pré-rédigé | MVP |
| 8 | Accès / droits | Je n'ai pas accès à mon lecteur réseau depuis la VM. | Oui | Réponse complète, procédure citée avec sa source | MVP |
| 9 | Sauvegarde VM | Quelle est la procédure pour sauvegarder une VM ? | Oui | Réponse complète, procédure citée avec sa source | MVP |
| 10 | Connexion VPN | Mon VPN se déconnecte toutes les 30 minutes. | Oui | Réponse complète, procédure citée avec sa source | MVP |
| 11 | Utilisation VM Clone | Où trouver la documentation pour cloner une VM ? | Oui | Réponse complète, procédure citée avec sa source | MVP |
| 12 | Authentification SSO | Mon nom d'utilisateur ou mon mot de passe est rejeté. | Partiel | Réponse partielle avec sa source, puis proposition d'escalade et ticket pré-rédigé | MVP |
| 13 | Demande offre | J'ai besoin de 5 VM supplémentaires pour mon équipe, comment faire ? | Oui | Réponse complète, procédure citée avec sa source | v2 |
| 14 | Connexion VPN | Je ne peux plus me connecter depuis la dernière mise à jour. | Oui | Réponse complète, procédure citée avec sa source | MVP |
| 15 | Lenteurs VM | Ma VM devient très lente en fin de journée. | Oui | Réponse complète, procédure citée avec sa source | MVP |
| 16 | Redémarrage VM | Comment réinitialiser une VM pour un nouvel utilisateur ? | Oui | Réponse complète, procédure citée avec sa source | MVP |
| 17 | Demande info offre | Quelle est la différence entre une VM Standard et une VM Clone ? | Oui | Réponse complète, procédure citée avec sa source | v2 |
| 18 | Authentification SSO | Mon certificat utilisateur a expiré. | Partiel | Réponse partielle avec sa source, puis proposition d'escalade et ticket pré-rédigé | MVP |
| 19 | Accès / droits | Un utilisateur n'a pas accès à la VM qui lui a été attribuée. | Oui | Réponse complète, procédure citée avec sa source | MVP |
| 20 | Lenteurs VM | J'ai un problème de latence depuis ce matin. | Oui | Réponse complète, procédure citée avec sa source | MVP |
| 21 | Connexion VPN | Mon VPN affiche « connexion perdue ». | Oui | Réponse complète, procédure citée avec sa source | MVP |
| 22 | Demande offre | Comment demander un quota de stockage supplémentaire ? | Oui | Réponse complète, procédure citée avec sa source | v2 |
| 23 | Sauvegarde VM | Comment sauvegarder mon environnement avant de redémarrer ? | Oui | Réponse complète, procédure citée avec sa source | MVP |
| 24 | Authentification SSO | J'ai une erreur lors de l'authentification par code. | Partiel | Réponse partielle avec sa source, puis proposition d'escalade et ticket pré-rédigé | MVP |
| 25 | Accès / droits | Comment vérifier que nos droits d'accès sont bien configurés ? | Oui | Réponse complète, procédure citée avec sa source | MVP |
| 26 | Redémarrage VM | Ma VM ne répond plus après 24 h sans utilisation. | Oui | Réponse complète, procédure citée avec sa source | MVP |
| 27 | Cas complexe | Certaines applications ne se lancent pas depuis la VM. | Non | « Je ne sais pas », aucune procédure inventée, ticket pré-rédigé | MVP |
| 28 | Cas complexe | J'ai besoin d'un snapshot de ma VM pour un test. | Non | « Je ne sais pas », aucune procédure inventée, ticket pré-rédigé | MVP |
| 29 | Cas complexe | L'infrastructure réseau semble en panne. | Non | « Je ne sais pas », aucune procédure inventée, ticket pré-rédigé | MVP |
| 30 | Cas complexe | Les données ne se synchronisent pas entre deux VM. | Non | « Je ne sais pas », aucune procédure inventée, ticket pré-rédigé | MVP |

*Périmètre : les 26 questions « MVP » portent sur la résolution d'incident ; les 4 questions « v2 » portent sur l'offre et ne seront évaluées qu'à partir de la v2.*

## 17.3 Notation

Chaque réponse de l'assistant est comparée au comportement attendu et classée dans l'une des trois catégories :

| Catégorie | Définition |
|---|---|
| **Exacte** | Le comportement attendu est respecté, et la source citée est la bonne |
| **Incomplète** | La réponse va dans le bon sens mais manque une étape, une source ou la proposition d'escalade |
| **Erronée** | Procédure fausse ou inventée, ou réponse donnée alors qu'aucune documentation n'existe |

## 17.4 Seuils de passage en bêta

| Indicateur | Seuil |
|---|---|
| Réponses exactes sur les 26 questions MVP | ≥ 80 % (au moins 21 sur 26) |
| Réponses erronées | 0 |
| Refus correct sur les 4 questions sans documentation (n° 27 à 30) | 4 sur 4 |

🔴 À exécuter : le jeu sera passé une première fois en début de bêta (S8), puis après chaque évolution de la documentation ou du modèle.
