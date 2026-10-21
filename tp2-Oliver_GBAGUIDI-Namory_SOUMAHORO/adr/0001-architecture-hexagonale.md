# ADR-0001 : Isoler le stockage derrière le port OrderRepository
- Date : 2026-10-21
- Statut : Proposé
- Décideurs : GBAGUIDI Micke-Phil, SOUMAHORO Namory

## Contexte
Les commandes sont aujourd'hui dans une Map en mémoire : elles sont perdues à chaque redémarrage et les identifiants repartent à CMD-0001. Il faudra passer à PostgreSQL, sans réécrire les règles métier ni les tests du cas d'utilisation.

## Décision
Le cas d'utilisation ne connaît que le port OrderRepository { save, findById }. Chaque technologie de stockage est un adaptateur secondaire, choisi dans container.js.

## Conséquences
- (+) Passer à PostgreSQL = écrire PostgresOrderRepository et modifier container.js uniquement.
- (+) Les tests tournent sans base de données (adaptateur en mémoire), donc rapides et fiables.
- (+) La règle est vérifiée automatiquement par `npm run lint:archi`.
- (-) Il faut écrire et maintenir la traduction Order <-> tables orders / order_lines.
- (-) Le contrat du port n'est pas formalisé (findById renvoie undefined, pas null) :
      il faut le documenter et le tester pour chaque adaptateur.
- (-) Plus de fichiers et d'indirections ; on ne peut pas utiliser directement
      les fonctionnalités SQL avancées (jointures, agrégats) depuis le cas d'utilisation.

## Alternatives considérées
- Architecture en couches (le service appelle directement le DAO SQL) : rejetée parce que le métier dépendrait de la base, et les tests auraient besoin d'une base lancée.
- Un ORM utilisé directement dans le domaine : rejetée, car le domaine importerait une bibliothèque externe, ce qui violerait lint:archi.