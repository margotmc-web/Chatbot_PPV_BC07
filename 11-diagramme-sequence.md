[← Accueil](README.md)

# 11. Diagramme de séquence

## Scénario principal — Résolution d'un incident avec cas d'erreur

Le scénario nominal est celui qui fonctionne aujourd'hui. Deux cas d'erreur y sont intégrés : l'absence de passage pertinent dans la documentation, et l'indisponibilité du service d'IA — panne réellement rencontrée lors du prototypage, à l'épuisement du crédit du fournisseur.

```mermaid
sequenceDiagram
    autonumber
    actor J as Jeanne
    participant F as Interface React
    participant S as Serveur Express
    participant R as Service RAG
    participant C as Base vectorielle
    participant IA as Modele OpenAI

    J->>F: Saisit « ma VM ne demarre pas »
    F->>S: POST /api/chat
    S->>R: traiterQuestion(texte)

    R->>IA: vectoriser(question)
    IA-->>R: empreinte de la question
    R->>C: rechercher(empreinte, 3)
    C-->>R: 3 morceaux + taux de correspondance

    alt Aucun passage au-dessus du seuil
        R-->>S: Score insuffisant
        S-->>F: « Aucune procedure documentee »
        F-->>J: Propose l'escalade
        Note over J,F: Suite : diagramme « Escalade »
    else Passages pertinents trouves
        R->>R: construireContexte(morceaux)
        R->>IA: redigerReponse(question, contexte)

        alt Service IA indisponible
            IA-->>R: Erreur 429 ou 503
            loop Nouvelle tentative apres 2 s
                R->>IA: redigerReponse(question, contexte)
                IA-->>R: Nouvelle erreur
            end
            R-->>S: Echec de redaction
            S-->>F: Message d'erreur + passages bruts
            F-->>J: Affiche les extraits de documentation
            Note over R,IA: Degradation controlee :<br/>la recherche reste exploitable<br/>meme si la redaction echoue
        else Reponse obtenue
            IA-->>R: Texte redige
            R-->>S: Echange complet
            S-->>F: Reponse + sources + taux
            F-->>J: Affiche la procedure et ses sources
        end
    end
```

## Escalade

```mermaid
sequenceDiagram
    autonumber
    actor J as Jeanne
    participant F as Interface React
    participant S as Serveur Express

    Note over J,F: Suite du scenario principal :<br/>aucun passage pertinent trouve
    J->>F: Accepte
    F->>S: POST /api/ticket
    S-->>F: Texte du ticket pre-redige
    F-->>J: Affiche le texte a copier
```

## Les trois chemins

| Chemin | Déclencheur | Comportement attendu | Statut |
|---|---|---|---|
| Nominal | Passages trouvés, service disponible | Réponse rédigée avec ses sources citées | 🟢 Vérifié |
| Erreur documentaire | Aucun morceau au-dessus du seuil de correspondance | Refus de répondre, puis pré-rédaction du ticket, que l'utilisateur colle dans ServiceNow | 🔴 À construire |
| Erreur technique | Le service d'IA renvoie une erreur de quota ou d'indisponibilité | Une nouvelle tentative, puis affichage des extraits bruts | 🔴 À construire |

## Pourquoi ces deux cas d'erreur

**L'erreur documentaire** est le point dur du projet. Un modèle de langage répond avec assurance même lorsque aucun document ne traite la question : sans seuil de correspondance, l'assistant invente une procédure plausible. Dans un contexte de support informatique, c'est le risque le plus grave.

**L'erreur technique** a été rencontrée pendant le prototypage, à l'épuisement du crédit d'essai du fournisseur. Elle a montré que la dépendance à un service externe est un point de rupture, et c'est ce constat qui a motivé l'itération 3 — la tentative d'exécution locale du modèle de vectorisation, restée inaboutie.

La dégradation contrôlée prévue ici est importante : même si la rédaction échoue, la recherche documentaire a déjà produit un résultat exploitable. L'utilisateur reçoit les extraits bruts plutôt qu'un écran vide.
