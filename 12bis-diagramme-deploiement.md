[← Accueil](README.md)

# 12. Diagramme de déploiement

Trois états sont présentés : le prototype tel qu'il s'exécute aujourd'hui, puis les deux phases de l'architecture cible.

Les couches portent **les mêmes noms d'un schéma à l'autre**, et apparaissent dans le même ordre. Celles qui n'existent pas à un stade donné ne sont pas représentées : un diagramme de déploiement ne montre que ce qui s'exécute réellement. Les absences sont signalées dans le tableau comparatif en 12.4.

Chaque stade comporte **une seule base de données**.

## 12.1 Prototype — ce qui s'exécute aujourd'hui

Cinq couches. Aucune escalade : le prototype ne crée pas de ticket.

```mermaid
flowchart TB
    subgraph ACC["Accès utilisateur"]
        A1["Navigateur<br/>poste de travail Windows"]
    end
    subgraph INT["Interface assistant"]
        B1["Interface React<br/>fichiers locaux"]
    end
    subgraph MOT["Moteur de traitement"]
        C1["Node.js / Express — port 3001<br/>server-fixed.js + rag-service.js"]
    end
    subgraph BDD["Base de données"]
        D1[("Chroma — base vectorielle<br/>conteneur Docker, port 8000<br/>128 morceaux indexés")]
    end
    subgraph IA["Modèle d'IA"]
        E1["OpenAI en accès direct<br/>text-embedding-3-small · gpt-3.5-turbo"]
    end

    SRC["Source documentaire<br/>4 documents locaux"]
    IDX["vectorize.py<br/>indexation manuelle"]

    A1 --> B1 --> C1
    C1 -->|1. recherche| D1
    C1 -->|2. rédaction| E1
    SRC --> IDX --> D1

    classDef ok fill:#e4f3e8,stroke:#1a6b2f,color:#143d1f;
    classDef ext fill:#fff2e0,stroke:#9a5c00,color:#4a2d00;
    class A1,B1,C1,D1,SRC,IDX ok
    class E1 ext
```

🟢 Opérationnel. Une seule machine, démarrage manuel, un utilisateur, aucune exposition réseau.

## 12.2 Cible — phase 1 (MVP)

Quatre couches. Le MVP ne génère pas de texte : il déroule des parcours de diagnostic écrits à l'avance, que le navigateur exécute seul. Il n'y a donc ni serveur de traitement, ni modèle d'IA.

```mermaid
flowchart TB
    subgraph ACC["Accès utilisateur"]
        direction LR
        A1["Site SharePoint<br/>de l'offre PPV"]
        A2["Catalogue des<br/>services numériques"]
        A3["Microsoft Entra ID<br/>authentification unique"]
    end
    subgraph INT["Interface assistant"]
        B1["Assistant PPV<br/>composant React intégré aux pages"]
        B2["Parcours de diagnostic prédéfinis<br/>exécutés dans le navigateur"]
    end
    subgraph BDD["Base de données"]
        D1[("Bibliothèque documentaire SharePoint<br/>procédures, FAQ, tarifs")]
    end
    subgraph ESC["Escalade"]
        F1["API ServiceNow<br/>création de ticket"]
    end

    A1 --> B1
    A2 --> B1
    A3 --> B1
    B1 --> B2
    B2 -->|consultation| D1
    B2 --> F1

    classDef todo fill:#fde9ec,stroke:#a1001c,color:#5c0010;
    class A1,A2,A3,B1,B2,D1,F1 todo
```

🔴 À construire. Aucune brique nouvelle hors du périmètre Microsoft 365 déjà en place : pas de service à faire valider par la sécurité, pas de coût à l'usage.

## 12.3 Cible — phase 2 (assistant augmenté)

Six couches. Les points d'entrée et l'interface ne changent pas. Ce qui s'ajoute est derrière : un moteur de traitement hébergé, un index vectoriel à la place de la consultation directe, et un modèle d'IA.

```mermaid
flowchart TB
    subgraph ACC["Accès utilisateur"]
        direction LR
        A1["Site SharePoint<br/>de l'offre PPV"]
        A2["Catalogue des<br/>services numériques"]
        A3["Microsoft Entra ID<br/>authentification unique"]
    end
    subgraph INT["Interface assistant"]
        B1["Assistant PPV<br/>agent Copilot Studio intégré aux pages"]
    end
    subgraph MOT["Moteur de traitement"]
        C1["Azure App Service<br/>backend Node.js / Express"]
    end
    subgraph BDD["Base de données"]
        D1[("Azure AI Search<br/>index vectoriel de la documentation")]
    end
    subgraph IA["Modèle d'IA"]
        E1["Azure OpenAI<br/>vectorisation et rédaction"]
    end
    subgraph ESC["Escalade"]
        F1["API ServiceNow<br/>création de ticket avec historique"]
    end

    SRC["Source documentaire<br/>bibliothèque SharePoint"]
    LOG["Journalisation<br/>conservation 6 mois"]

    A1 --> B1
    A2 --> B1
    A3 --> B1
    B1 --> C1
    C1 -->|1. recherche| D1
    C1 -->|2. rédaction| E1
    C1 --> F1
    C1 --> LOG
    SRC -->|indexation automatique| D1

    classDef todo fill:#fde9ec,stroke:#a1001c,color:#5c0010;
    class A1,A2,A3,B1,C1,D1,E1,F1,SRC,LOG todo
```

🔴 À construire.

## 12.4 Comparaison des trois états

| Couche | Prototype 🟢 | Phase 1 — MVP 🔴 | Phase 2 🔴 |
|---|---|---|---|
| **Accès utilisateur** | Navigateur, poste local | SharePoint de l'offre PPV et catalogue des services numériques | Identique à la phase 1 |
| **Interface assistant** | Interface React autonome | Composant React intégré aux pages, parcours exécutés dans le navigateur | Agent Copilot Studio intégré aux pages |
| **Moteur de traitement** | Node.js / Express sur le poste | *Absent* | Node.js / Express hébergé sur Azure App Service |
| **Base de données** | Chroma, index vectoriel en conteneur | Bibliothèque documentaire SharePoint | Azure AI Search, index vectoriel |
| **Modèle d'IA** | OpenAI en accès direct | *Absent* | Azure OpenAI |
| **Escalade** | *Absente* | API ServiceNow | API ServiceNow, avec transmission de l'historique |
| Source documentaire | 4 documents locaux | Bibliothèque SharePoint, consultée directement | Bibliothèque SharePoint, indexée automatiquement |
| Indexation | Script Python lancé à la main | Sans objet | Automatique à chaque mise à jour |
| Authentification | Aucune | Microsoft Entra ID | Microsoft Entra ID |
| Journalisation | Aucune | Héritée de SharePoint | Interne SNCF, 6 mois, conformité RGPD |
| Utilisateurs | 1 | Environ 1 500 | Environ 1 500 |

## 12.5 Trois lectures du tableau

**Le MVP n'a pas de moteur de traitement, et c'est voulu.** Dérouler des parcours écrits à l'avance est quelque chose qu'un navigateur sait faire seul. Un serveur ne redevient nécessaire qu'à partir du moment où il faut chercher dans la documentation et rédiger une réponse — donc en phase 2.

**La base de données change de nature à chaque étape, mais il n'y en a jamais qu'une.** Le prototype interroge un index vectoriel local, le MVP consulte directement la documentation SharePoint, la phase 2 interroge un index vectoriel hébergé. La bibliothèque SharePoint n'est une base interrogée qu'en phase 1 ; en phase 2, elle devient une source qui alimente l'index.

**Le prototype n'est pas la cible en réduction.** Chroma et Docker ont été retenus pour prototyper vite sur un poste de travail. Les conserver imposerait au service PPV d'exploiter une brique supplémentaire, à faire valider par la sécurité, là où Azure AI Search rend le même service et figure déjà au catalogue SNCF. Ce que le prototype démontre, c'est le principe — la recherche documentaire précède la génération — et non le choix des outils.
