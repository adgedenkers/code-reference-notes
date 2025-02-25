# DB Rules for AI Prep

Always use the following rules when creating SQL tables:

1. Use the correct SQL dialect for the platform (e.g., SQLite, MS SQL, PostgreSQL).
2. Enable and enforce foreign keys for all relationships.
3. Use plural table names.
4. Use "snake_case" for all names (fields, tables, views, etc.).
5. Make use of lookup tables wherever logical.
6. Include the following fields in all applicable tables:
    - `id` as the primary key.
    - `created` and `updated` timestamp fields, with `updated` set to update automatically on any record change.
    - `active` (integer) to indicate whether a record is valid, with a default value of `1`.
    - Foreign key relationships where applicable (e.g., `user_id`, `created_by_id`).
7. Create indexes on key fields to optimize queries.
8. Define triggers for timestamp updates where applicable.

**Example Implementation (SQLite):**
```sql
CREATE TABLE items (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    item TEXT,
    user_id INTEGER NOT NULL,
    created_by_id INTEGER NOT NULL DEFAULT 0;
    active INTEGER NOT NULL DEFAULT 1,
    created TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (created_by_id) REFERENCES users(id)
);

CREATE INDEX idx_items_id ON items(id);
CREATE INDEX idx_items_item ON items(item);
CREATE INDEX idx_items_user_id ON items(user_id);

CREATE TRIGGER update_items_updated
AFTER UPDATE ON items
FOR EACH ROW
BEGIN
    UPDATE items
    SET updated = CURRENT_TIMESTAMP
    WHERE id = OLD.id;
END;
```
