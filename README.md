# Architecture Logicielle – Séance 2 · Architecture hexagonale

M1 Data Engineer · INGETIS

TP 2 réalisé par **Oliver GBAGUIDI** et **Namory SOUMAHORO** (parcours Data).

## Contenu

| Dossier | Contenu |
|---|---|
| `hexagonal/` | Exemple du cours : module Commande de ShopLite en architecture hexagonale |
| `clean/` | Exemple du cours : virement bancaire en Clean Architecture |
| `docs/adr/` | Modèle d'ADR et ADR d'exemple |
| `tp2-Oliver_GBAGUIDI-Namory_SOUMAHORO/` | Notre TP 2 |

Dans le dossier du TP :

- `hexagone.md` : étapes 1 et 2 (le cas d'utilisation piloté par trois portes : ligne de commande, API HTTP, tests ; schéma Mermaid de l'hexagone avec les ports et la composition root) ;
- `note-specialite.md` : étape 3, lecture Data (contrat des données de la commande, persistance et identifiants, schéma PostgreSQL d'un futur `PostgresOrderRepository`) et étape 4 (règle d'architecture cassée puis réparée) ;
- `adr/0001-architecture-hexagonale.md` : ADR « Isoler le stockage derrière le port OrderRepository ».

## Lancer l'exemple

Prérequis : Node.js 20 ou plus récent, aucune dépendance à installer. Commandes à lancer depuis ce dossier.

Ligne de commande :

```bash
node hexagonal/cli.js C-42 CLAVIER:2 SOURIS:1
```

API HTTP (port 3000), puis envoyer les requêtes de `hexagonal/requests.http` avec l'extension REST Client :

```bash
npm run hexa:server
```

Tests, sans serveur ni base :

```bash
node --test hexagonal/PlaceOrder.test.js
```

## Vérifier la règle d'architecture

```bash
npm run lint:archi
```

L'outil vérifie qu'aucun fichier d'un dossier `domain/` n'importe l'extérieur.

## Voir le schéma de l'hexagone

Ouvrir `tp2-Oliver_GBAGUIDI-Namory_SOUMAHORO/hexagone.md` dans VS Code avec l'extension Markdown Preview Mermaid Support, puis Ctrl+Shift+V.