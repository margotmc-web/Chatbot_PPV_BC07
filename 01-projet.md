[← Accueil](README.md)

# 1. Le projet

## 1.1 Présentation

Le service PPV de la SNCF met à disposition de ses clients internes des machines virtuelles, facturées à la consommation. Son support de niveau 1 traite des demandes très répétitives : une machine qui ne démarre pas, une session qui se ferme, une question sur les conditions tarifaires de l'offre.

Le projet consiste à concevoir un assistant conversationnel capable de traiter ces demandes de premier niveau à partir de la documentation interne du service. L'environnement cible est entièrement cloud Microsoft : Sharepoint de l'offre/Catalogue des services numériques comme portes d'entrée, SharePoint comme base documentaire, pour une population d'environ 1 500 utilisateurs.

**Périmètre du prototype.** Ce dossier ne porte pas sur l'assistant dans son ensemble, mais sur la brique qui concentre la difficulté technique : faire répondre une intelligence artificielle à partir d'une documentation d'entreprise, de façon fiable et à un coût soutenable pour 1 500 utilisateurs.

Les briques d'ingénierie classiques — intégration aux pages SharePoint de l'offre, gestion des comptes, montée en charge — sont volontairement exclues : elles ne présentent pas de difficulté de conception et ne relèvent pas d'une démonstration de faisabilité.

## 1.2 Objectifs

> **Problématique.** Dans quelle mesure l'automatisation par IA du support de niveau 1 permet-elle d'améliorer à la fois la disponibilité des VM, levier direct de revenu pour le service PPV, et l'accès rapide à l'information de l'offre, gage de satisfaction client ?

Deux objectifs en découlent, qui correspondent à deux usages distincts :

1. **Réduire le temps d'indisponibilité des machines virtuelles**, en permettant à l'utilisateur de résoudre seul les incidents les plus courants, sans attendre l'ouverture d'un ticket.
2. **Rendre l'information sur l'offre immédiatement accessible**, sans mobiliser un interlocuteur humain pour des questions déjà documentées.

## 1.3 Enjeux

| Nature | Enjeu | Statut |
|---|---|---|
| Économique | Les incidents affectant la disponibilité des VM représentent une perte estimée à 1,87 M€ par an. Mode de calcul et hypothèses en annexe du mémoire principal (renvois H1 à H12). | 🟡 Estimé |
| Opérationnel | Le support de niveau 1 absorbe un volume important de demandes répétitives, au détriment des sujets à plus forte valeur ajoutée. | 🟡 Estimé |
| Technique | Une IA interrogée sans préparation coûte trop cher à l'échelle de 1 500 utilisateurs. Cette contrainte structure l'ensemble du prototype. | 🟢 Vérifié |
| Conformité | La documentation interrogée est interne : l'architecture doit permettre de garder la maîtrise des données transmises à un service tiers. | 🔴 À construire |

## 1.4 Utilisateurs cibles

| Persona | Profil | Attentes vis-à-vis de l'assistant |
|---|---|---|
| Jeanne, 50 ans | Usage quotidien d'une VM standard | Gagner du temps à l'utilisation et dans le traitement des incidents |
| Louis, 50 ans | Usage occasionnel d'une VM clone | Mêmes gains, avec une réorientation possible vers une VM standard |
| Léa, 45 ans | Gestionnaire du parc | Fidéliser les clients, augmenter le parc, faire connaître l'offre |
