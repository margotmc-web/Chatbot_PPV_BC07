## Architecture technique du Chatbot PPV

### Panorama : les 3 états de l'architecture

L'architecture du chatbot suit une progression en 3 états qui reflète les phases d'implémentation et les gains de maturité :

- **État 0 (Prototype)** : Faisabilité validée localement
- **État 1 (MVP)** : Solution cible phase 1 — déploiement pilote auprès des N1 SNCF
- **État 2 (Production)** : Solution cible phase 2 — déploiement complet avec intégration Copilot Studio et automatisation

---

## État 0 : Prototype (aujourd'hui)

Le prototype est construit pour valider la faisabilité, intégralement exécuté en local.

```mermaid
flowchart LR
    U([Utilisateur PPV]) --> F["Interface React<br/>(navigateur)"]
    F -->|question| S["Serveur Node.js / Express<br/>server-fixed.js"]
    S -->|1. recherche par le sens| C[("Chroma<br/>base vectorielle<br/>Docker — port 8000")]
    C -->|3 passages pertinents| S
    S -->|2. question + passages| L["OpenAI<br/>gpt-3.5-turbo<br/>(accès direct)"]
    L -->|réponse rédigée| S
    S -->|réponse + sources| F
    D["Documentation PPV<br/>4 documents"] -.->|préparation<br/>vectorize.py| C

    classDef proto fill:#e4f3e8,stroke:#1a6b2f,color:#143d1f;
    classDef ext fill:#fff2e0,stroke:#9a5c00,color:#4a2d00;
    class F,S,C,D proto
    class L ext
```

**Caractéristiques** :
- Entièrement local (poste développeur)
- Indexation manuelle des 4 documents
- Pas d'authentification
- Pas de journalisation
- Réponse directe en temps réel

**Avantages** : Rapidité de mise en œuvre, contrôle total, coût zéro infrastructure

**Limites** : Pas de persistance, pas de multi-utilisateurs, infrastructure manuelle

---

## État 1 : MVP — Phase 1 de la cible

L'État 1 déploie la solution cible phase 1 auprès d'un groupe pilote d'utilisateurs N1.

```mermaid
flowchart LR
    subgraph FRONTEND["FRONTEND"]
        direction LR
        FE1["Interface React<br/>(composant SharePoint)"]
        FE2["Connexion SNCF<br/>(Entra ID)"]
    end
    
    subgraph BACKEND["BACKEND"]
        BE["Node.js / Express<br/>(Azure App Service)"]
    end
    
    subgraph DATABASE["BDD & INDEXATION"]
        direction LR
        DB1["SharePoint de l'offre<br/>(documentation PPV)"]
        DB2[("Azure AI Search<br/>(base vectorielle<br/>service hébergé)")]
        DB3["Historique des<br/>conversations"]
    end
    
    subgraph IA_MODEL["MODÈLE IA"]
        IA["Azure OpenAI<br/>(gpt-4o-mini)"]
    end
    
    U([Utilisateur N1<br/>sur SNCF intranet]) --> FE1
    FE1 --> FE2
    FE2 --> BE
    BE -->|1. requête indexée| DB2
    DB2 -->|passages + contexte| BE
    BE -->|2. question enrichie| IA
    IA -->|réponse + sources| BE
    BE -->|réponse + historique| FE1
    FE1 --> U
    DB1 -.->|synchronisation automatique| DB2
    BE -.->|log conversations| DB3

    classDef micro fill:#e6f3ff,stroke:#0066cc,color:#003366;
    classDef service fill:#e4f3e8,stroke:#1a6b2f,color:#143d1f;
    classDef ia fill:#c4203f,stroke:#8f1026,color:#fff;
    classDef cloud fill:#f0f0f0,stroke:#666,color:#333;
    class FRONTEND,BACKEND,DATABASE,IA_MODEL cloud
    class FE1,FE2,BE micro
    class DB1,DB2,DB3 service
    class IA ia
```

**Caractéristiques** :
- Frontend React intégré à SharePoint
- Authentification via Entra ID (compte SNCF)
- Backend hébergé sur Azure App Service
- Documentation depuis SharePoint (synchronisation automatique)
- Base vectorielle managée (Azure AI Search)
- Modèle IA via Azure OpenAI
- Historique des conversations conservé 6 mois
- Déploiement via Git / GitHub → Azure

**Avantages** :
- Environnement Microsoft homogène
- Données traitées en région UE
- Authentification unifiée SNCF
- Scalabilité horizontale possible
- Support de l'infrastructure SNCF
- Multi-utilisateurs natif

**Limites** :
- Copilot Studio pas encore intégré (UI à maintenir en React)
- Indexation manuelle des documents initialement
- Capacité de charge limitée (phase pilote)

**Timeline** : 2-3 mois après approbation prototype

---

## État 2 : Production complète — Phase 2 de la cible

L'État 2 déploie la solution cible phase 2 à tous les N1 PPV avec automatisation et supervision.

```mermaid
flowchart LR
    subgraph FRONTEND["FRONTEND"]
        direction LR
        FE1["Agent Copilot Studio<br/>(dans pages SharePoint)"]
        FE2["Intégration native<br/>SharePoint + Teams"]
        FE3["Authentification<br/>Entra ID"]
    end
    
    subgraph BACKEND["BACKEND & ORCHESTRATION"]
        BE["Node.js / Express<br/>(Azure App Service<br/>haute dispo)"]
        MONITOR["Supervision<br/>(Application Insights)"]
    end
    
    subgraph DATABASE["BDD & INDEXATION AUTOMATIQUE"]
        direction LR
        DB1["SharePoint de l'offre<br/>(sync auto)"]
        DB2[("Azure AI Search<br/>(indexation auto)")]
        DB3["Historique 6 mois<br/>(Azure Storage)"]
        DB4["Power BI Facturation<br/>(suivi revenu)"]
    end
    
    subgraph IA_MODEL["MODÈLE IA"]
        IA["Azure OpenAI<br/>(gpt-4o)"]
    end
    
    U([N1 PPV<br/>100+ utilisateurs]) --> FE1
    FE1 --> FE2
    FE2 --> FE3
    FE3 --> BE
    BE -->|1. requête indexée| DB2
    DB2 -->|passages + contexte| BE
    BE -->|2. question enrichie| IA
    IA -->|réponse + sources| BE
    BE -->|journaux| MONITOR
    BE -->|historique| DB3
    BE -->|métriques usage| DB4
    BE -->|réponse| FE1
    FE1 --> U
    DB1 -.->|sync continue| DB2
    MONITOR -.->|alertes| BE

    classDef prod fill:#fff4e6,stroke:#d97706,color:#78350f;
    classDef service fill:#e4f3e8,stroke:#1a6b2f,color:#143d1f;
    classDef ia fill:#c4203f,stroke:#8f1026,color:#fff;
    classDef cloud fill:#f0f0f0,stroke:#666,color:#333;
    class FRONTEND,BACKEND,DATABASE,IA_MODEL cloud
    class FE1,FE2,FE3,BE,MONITOR prod
    class DB1,DB2,DB3,DB4 service
    class IA ia
```

**Caractéristiques** :
- Frontend Copilot Studio (pas de maintenance React)
- Intégration native SharePoint et Teams
- Backend en haute disponibilité
- Indexation entièrement automatique
- Suivi financier en Power BI (revenu additionnel)
- Supervision applicative (Application Insights)
- Support de 100+ N1 simultanés
- Modèle IA optimisé (gpt-4o)

**Avantages** :
- Absence de maintenance UI (Copilot Studio natif)
- Automatisation complète (pas d'intervention manuelle)
- Supervision proactive des défaillances
- ROI finalisé et mesuré (Power BI)
- Scalabilité sans limite pratique
- Support 24/7 SNCF standardisé

**Limites** :
- Coût infrastructure élevé
- Dépendance Microsoft complète
- Changement de paradigme pour les N1 (Copilot vs React)

**Timeline** : 6-9 mois après État 1 stabilisé

---

## Comparaison synthétique : Prototype → État 1 → État 2

| Dimension | Prototype | État 1 | État 2 |
|---|---|---|---|
| **Frontend** | React local | React + SharePoint | Copilot Studio natif |
| **Backend** | Node.js local | Azure App Service | App Service HA |
| **Stockage** | Chroma Docker local | Azure AI Search | Azure AI Search auto-indexé |
| **Documentation** | 4 fichiers manuels | SharePoint + sync auto | SharePoint + indexation temps réel |
| **Authentification** | Aucune | Entra ID SNCF | Entra ID SNCF |
| **Modèle IA** | OpenAI direct | Azure OpenAI (gpt-4o-mini) | Azure OpenAI (gpt-4o) |
| **Historique** | Aucun | 6 mois conservé | 6 mois conservé + PBI |
| **Supervision** | Aucune | Logs applicatifs | Application Insights complet |
| **Utilisateurs** | 1 (dev) | ~20 pilotes | 100+ en production |
| **Déploiement** | Docker local | Git → Azure | GitOps complet |
| **Coût infrastructure** | 0 € | ~2k€/mois | ~4k€/mois |
| **Coût IA** | Partagé OpenAI | Azure OpenAI ~500€/mois | Azure OpenAI ~1000€/mois |
| **Durée déploiement** | 2 semaines | 2-3 mois | 6-9 mois |

---

## Écarts entre Prototype et Architecture cible (État 1 & 2)

### Raisons des écarts

| Écart | Prototype | Cible (État 1+) | Justification |
|---|---|---|---|
| **Lieu d'exécution** | Local (poste dev) | Cloud (Azure) | Infrastructure d'entreprise, pas de machine locale requise pour les N1 |
| **Authentification** | Aucune | Entra ID SNCF | Traçabilité légale et conformité RGPD (qui a posé quelle question ?) |
| **Documentation source** | 4 fichiers manuels | SharePoint officiel de l'offre | Source unique de vérité, mise à jour centralisée |
| **Indexation** | Script Python manuel | Automatique (Azure) | Scalabilité : indexer automatiquement 2500 VM chaque jour |
| **Base vectorielle** | Chroma (locale) | Azure AI Search | Service managé, pas de maintenance, backup automatique |
| **Historique** | Aucun | 6 mois conservé | RGPD + amélioration du modèle IA par feedback |
| **Modèle IA** | OpenAI direct | Azure OpenAI | Données UE, région compliant SNCF, facturation centralisée |
| **Supervison** | Aucune | Application Insights (État 1) / Complète (État 2) | Détection proactive des défaillances, alertes |
| **UI maintenance** | React (alternant) | Copilot Studio (aucune) en État 2 | Copilot Studio = outil Microsoft prêt à l'emploi, moins de code à maintenir |

### Risques d'écart et mitigations

- **Risque** : Données OpenAI vs Azure OpenAI diffèrent légèrement → Réponses IA divergentes
  - **Mitigation** : Benchmark des deux services (cf. [03-benchmark.md](03-benchmark.md)) ; Azure OpenAI + modèle gpt-4o-mini est validé en français
  
- **Risque** : SharePoint moins rapide que 4 fichiers indexés
  - **Mitigation** : Azure AI Search inclut caching et optimisation requête; tests de charge en État 1
  
- **Risque** : Copilot Studio impose UX différente (État 2)
  - **Mitigation** : Formation N1 et démonstration prototype d'ici État 1 ; feedback avant État 2

---

## Points de validation critiques

### Pour État 1 (MVP) :
1. ✓ Azure OpenAI + gpt-4o-mini en français = qualité ≥ prototype
2. ✓ Azure AI Search troure les 3 passages utiles en < 500ms
3. ✓ Entra ID et SharePoint synchronisés
4. ✓ Support < 2s pour N1 pilotes

### Pour État 2 (Production) :
1. ✓ Copilot Studio stable à 100+ N1 simultanés
2. ✓ Indexation automatique ne casse rien
3. ✓ Application Insights détecte défaillances
4. ✓ Power BI suit revenu additionnel (15k€/an réalisés)
