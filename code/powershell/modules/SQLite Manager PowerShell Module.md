# SQLiteManager PowerShell Module Cheat Sheet

## 📌 Module Overview

The **SQLiteManager** module provides PowerShell functions for creating, managing, and versioning SQLite databases. It ensures schema changes are logged and allows for both automatic and manual versioning.

---

## 🔄 **Importing the Module**

To load the module into your PowerShell session:

```powershell
Import-Module SQLiteManager
```

To force reload after changes:

```powershell
Remove-Module SQLiteManager -Force
Import-Module SQLiteManager -Force
```

To check where PowerShell is loading the module from:

```powershell
Get-Module SQLiteManager | Select-Object Name, Path
```

---

## 📌 **Available Commands**

### 1️⃣ **Initialize a New Database**

Creates a new SQLite database and sets up required tables.

```powershell
Initialize-SQLiteDatabase
```

**Prompts for:**

- Database name
- Save location
- Application name
- Author

🔹 **Creates Tables:**

- `database`: Tracks metadata
- `changes`: Logs schema modifications

---

### 2️⃣ **Set (Update) the Database Schema**

Logs schema changes and automatically bumps the minor version.

```powershell
Set-DatabaseSchema
```

**Prompts for:**

- Description of schema change

🔹 **Example Output:**

```
Schema updated! New version: 1.1
Change recorded: Added 'users' table
```

---

### 3️⃣ **Set (Manually Update) the Major Version**

Manually increments the major version (e.g., `1.x → 2.0`).

```powershell
Set-DatabaseVersion
```

**Prompts for:**

- Description of major update

🔹 **Example Output:**

```
Major version updated! New version: 2.0
Change recorded: Major redesign of database structure
```

---

### 4️⃣ **View Change History**

Displays logged schema changes.

```powershell
Get-ChangeHistory
```

🔹 **Example Output:**

```
change_id | change_description       | version_affected | timestamp
--------- | ------------------------ | ---------------- | -----------------
1         | Added users table        | 1.1              | 2025-02-20 14:30
2         | Major redesign           | 2.0              | 2025-02-20 14:35
```

---

### 5️⃣ **Invoke a SQL File Against a SQLite Database**

Executes a SQL script file on the specified SQLite database.

```powershell
Invoke-SQLiteScript -DatabasePath "C:\path\to\database.db" -ScriptPath "C:\path\to\script.sql"
```

**Parameters:**

- `-DatabasePath`: Path to the SQLite database.
- `-ScriptPath`: Path to the SQL file to execute.

🔹 **Example Output:**

```
Executing script: C:\path\to\script.sql on database: C:\path\to\database.db
Execution completed successfully.
```

---

## 📌 **Debugging & Verification**

### 🔎 **Find Available Modules**

```powershell
Get-Module -ListAvailable | Where-Object { $_.Name -eq "SQLiteManager" }
```

### 🔎 **Find All Installed Module Locations**

```powershell
$env:PSModulePath -split ';'
```

### 🚀 **Manually Import a Specific Version**

```powershell
Import-Module "C:\Users\adged\Documents\PowerShell\Modules\SQLiteManager\SQLiteManager.psm1" -Force
```

---

## 🛠 **Customizing & Expanding**

To permanently add the module path to your PowerShell environment:

```powershell
notepad $PROFILE
```

Add this line at the bottom and save:

```powershell
$env:PSModulePath += ";C:\Users\adged\Documents\PowerShell\Modules"
```

Then reload your profile:

```powershell
. $PROFILE
```

---

## 🚀 **Final Notes**

- **Minor version updates** happen **automatically** with `Set-DatabaseSchema`.
- **Major version updates** must be triggered **manually** with `Set-DatabaseVersion`.
- **All schema changes** are **logged** in the `changes` table for tracking.
- **Check the database location** if you’re unsure where PowerShell is loading the module from!

📢 Let me know if you'd like more features added to this module! 🚀