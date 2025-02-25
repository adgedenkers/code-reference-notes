- Foreign Keys On Always
- All appropriate table names are plural
- names of all fields, tables, views, etc. will be in "snake case"
- lookup tables 
- Other SQLite3 Databases will be attached and will join with data from that users table.
- All Tables, in each database, must have the following fields.
```sql
    active INTEGER DEFAULT 1, -- Status field
    created_by_user_id INTEGER, -- User who created the record
    created DATETIME DEFAULT CURRENT_TIMESTAMP, -- Record creation time
    updated DATETIME DEFAULT CURRENT_TIMESTAMP, -- Record update time
```
- Each table, in each database will have a trigger, so that when the data in even an individual field in a record is updated, that triggers the function that updates the `updated` column of the record that changed, to show the date and time the update was made.
```sql
CREATE TRIGGER update_{table_name}_updated 
AFTER UPDATE ON {table_name}
FOR EACH ROW
BEGIN
    UPDATE {table_name} 
    SET updated = CURRENT_TIMESTAMP 
    WHERE id = NEW.id;
END;
```
