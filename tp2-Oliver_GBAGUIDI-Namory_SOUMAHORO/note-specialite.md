# ÉTAPE 3

1- Une commande contient les champs: id, customerId, lines, total et createdAt. Les champs stockés sont id, customerId, createdAt et lines (sku, qty, unitPrice). Le champ calculé est total.

2- C'est le prix au moment de l'achat. Si le catalogue change plus tard, les anciennes commandes, le chiffre d'affaires et les statistiques restent justes. Sans cette copie, il faudrait historiser le catalogue.

3- findBy renvoie undefined quand la commande n'existe pas. Ce comportement n'est écrit nulle part.

4- Lorsque le serveur redémarre toutes les commandes sont perdues. Les identifiants viennent d'un compteur n dans container.js, qui repart à zéro à chaque démarrage.

5- Schéma PostgreSQL :
CREATE TABLE orders (
  id          VARCHAR(20) PRIMARY KEY,
  customer_id VARCHAR(50) NOT NULL,
  created_at  TIMESTAMPTZ NOT NULL
);
CREATE TABLE order_lines (
  order_id    VARCHAR(20) NOT NULL REFERENCES orders(id) ON DELETE CASCADE,
  line_no     INTEGER     NOT NULL,
  sku         VARCHAR(50) NOT NULL,
  qty         INTEGER     NOT NULL CHECK (qty > 0),
  unit_price  NUMERIC(10,2) NOT NULL,
  PRIMARY KEY (order_id, line_no)
);

    - La traduction entre l'objet Order et les lignes des tables se ferait uniquement dans adapters/secondary/PostgresOrderRepository.js


# ÉTAPE 4

    - Message d'erreur: Règle violée : le domaine dépend de l'extérieur :
                        seance2-architecture/hexagonal/domain/Order.js  ->  ../container.js