[← Accueil](README.md)

> ⚠️ Version obsolète, conservée pour l'historique : voir la version à jour, [12bis — Diagramme de déploiement](12bis-diagramme-deploiement.md).

# 12. Diagramme de déploiement

## Environnement de prototypage — ce qui tourne aujourd'hui

Tout s'exécute sur un seul poste de travail, à l'exception des appels au fournisseur d'IA.

```mermaid
flowchart TB
    subgraph POSTE["NOEUD — Poste de travail Windows"]
        direction TB
        subgraph NAV["Navigateur"]
            N1["Interface React<br/>fichiers statiques"]
        end
        subgraph NODE["Processus Node.js — port 3001"]
            N2["server-fixed.js<br/>Express"]
            N3["rag-service.js"]
        end
        subgraph DOCK["Conteneur Docker — port 8000"]
            N4[("Chroma<br/>128 morceaux indexes")]
        end
        N5["vectorize.py<br/>execution ponctuelle"]
    end

    subgraph EXT["NOEUD — Internet"]
        E1["API OpenAI<br/>text-embedding-3-small<br/>gpt-3.5-turbo"]
    end

    N1 -->|HTTP localhost| N2
    N2 --> N3
    N3 -->|HTTP localhost:8000| N4
    N3 -->|HTTPS| E1
    N5 -->|indexation manuelle| N4
    N5 -->|HTTPS| E1

    classDef ok fill:#e4f3e8,stroke:#1a6b2f,color:#143d1f;
    classDef ext fill:#fff2e0,stroke:#9a5c00,color:#4a2d00;
    class N1,N2,N3,N4,N5 ok
    class E1 ext
```

| Caractéristique | Valeur |
|---|---|
| Nombre de machines | 1 (poste de travail) |
| Démarrage | Manuel, deux fenêtres de commande |
| Exposition réseau | Aucune — accès local uniquement |
| Secrets | Fichier de configuration local, exclu du dépôt |
| Utilisateurs simultanés | 1 |

## Architecture de déploiement cible

L'environnement cible repose sur l'écosystème Azure de la SNCF. La base vectorielle locale disparaît au profit d'Azure AI Search, et l'interface passe par Copilot Studio plutôt que par une application à maintenir.

```mermaid
flowchart TB
    subgraph POSTE2["NOEUD — Poste utilisateur"]
        U1["Microsoft Teams"]
    end

    subgraph AZURE["NOEUD — Tenant Azure SNCF"]
        direction TB
        subgraph PRES["Couche presentation"]
            A1["Copilot Studio<br/>interface native Teams"]
            A2["Azure AD<br/>authentification SSO"]
        end
        subgraph APP["Couche applicative"]
            A3["Backend Node.js / Express<br/>service heberge"]
        end
        subgraph DATA["Couche donnees"]
            A4[("Azure AI Search<br/>index documentaire")]
            A5["SharePoint<br/>documentation PPV"]
            A6["Journalisation<br/>conservation 6 mois"]
        end
        subgraph IA["Couche IA"]
            A7["Azure OpenAI"]
        end
    end

    subgraph SN["NOEUD — ServiceNow"]
        S1["API de creation de tickets"]
    end

    U1 --> A1
    A1 --> A2
    A1 --> A3
    A3 -->|1. recherche| A4
    A3 -->|2. redaction| A7
    A5 -->|indexation automatique| A4
    A3 --> A6
    A3 -->|escalade| S1

    classDef todo fill:#fde9ec,stroke:#a1001c,color:#5c0010;
    class U1,A1,A2,A3,A4,A5,A6,A7,S1 todo
```

| Caractéristique | Valeur |
|---|---|
| Hébergement | Azure, écosystème SNCF |
| Authentification | Azure AD, authentification unique |
| Point d'entrée | Microsoft Teams via Copilot Studio |
| Données de journalisation | Conservation 6 mois, conformité RGPD |
| Utilisateurs cibles | Environ 1 500 |

## Ce qui change entre les deux

| Élément | Prototype | Cible |
|---|---|---|
| Base de recherche | Chroma, conteneur Docker local | Azure AI Search |
| Modèle d'IA | OpenAI en accès direct | Azure OpenAI |
| Interface | Application React autonome | Copilot Studio dans Teams |
| Indexation | Script Python lancé à la main | Automatique depuis SharePoint |
| Authentification | Aucune | Azure AD, authentification unique |
| Escalade | Aucune | API ServiceNow |

> Le conteneur Docker et la base Chroma sont un choix de prototypage, pas une préconisation. L'option d'une base vectorielle auto-hébergée a été écartée pour la cible : elle imposerait au service PPV d'exploiter une brique supplémentaire, là où Azure AI Search est déjà disponible dans l'environnement SNCF.
