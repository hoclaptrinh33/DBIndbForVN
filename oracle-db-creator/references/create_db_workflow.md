# Oracle Database Creation Workflow

This document outlines the manual process for creating a new Oracle Container Database (CDB) on a local server using SQL\*Plus.

## Prerequisites

- Oracle Database software must be installed.
- The user must have administrative privileges (e.g., `oracle` user on Linux, or a user in the `ORA_DBA` group on Windows).
- Sufficient disk space for datafiles, control files, and redo logs.

## Step-by-Step Guide

### 1. Set Environment Variables

Set the `ORACLE_SID` and `ORACLE_HOME` environment variables.

**Linux:**

```bash
export ORACLE_SID=orcl
export ORACLE_HOME=/u01/app/oracle/product/19.0.0/dbhome_1
export PATH=$ORACLE_HOME/bin:$PATH
```

**Windows:**

```cmd
set ORACLE_SID=orcl
set ORACLE_HOME=C:\app\oracle\product\19.0.0\dbhome_1
set PATH=%ORACLE_HOME%\bin;%PATH%
```

### 2. Create Required Directories

Create the necessary directories for the database files, trace files, and audit files.

**Linux:**

```bash
mkdir -p /u01/app/oracle/oradata/orcl
mkdir -p /u01/app/oracle/admin/orcl/adump
mkdir -p /u01/app/oracle/admin/orcl/dpdump
mkdir -p /u01/app/oracle/admin/orcl/pfile
mkdir -p /u01/app/oracle/fast_recovery_area/orcl
```

**Windows:**

```cmd
mkdir C:\app\oracle\oradata\orcl
mkdir C:\app\oracle\admin\orcl\adump
mkdir C:\app\oracle\admin\orcl\dpdump
mkdir C:\app\oracle\admin\orcl\pfile
mkdir C:\app\oracle\fast_recovery_area\orcl
```

### 3. Create the Initialization Parameter File (PFILE)

Create a text file named `init<SID>.ora` (e.g., `initorcl.ora`) in the `$ORACLE_HOME/dbs` directory (Linux) or `%ORACLE_HOME%\database` directory (Windows).

Use the template provided in `assets/init.ora.template` and replace the placeholders with the actual values.

### 4. Start the Instance in NOMOUNT Mode

Connect to SQL\*Plus as `SYSDBA` and start the instance using the PFILE created in the previous step.

```bash
sqlplus / as sysdba
```

```sql
STARTUP NOMOUNT PFILE='<path_to_pfile>/initorcl.ora';
```

### 5. Create the Database

Execute the `CREATE DATABASE` statement. Use the template provided in `assets/create_db.sql.template` and replace the placeholders with the actual values.

```sql
@<path_to_create_db_script>/create_db.sql
```

### 6. Build the Data Dictionary Views

After the database is created, run the scripts to build the data dictionary views and PL/SQL packages. This step is crucial for the database to function correctly.

```sql
@?/rdbms/admin/catalog.sql
@?/rdbms/admin/catproc.sql
@?/rdbms/admin/catoctk.sql
@?/rdbms/admin/owminst.plb
```

Connect as `SYSTEM` and run the `pupbld.sql` script.

```sql
CONNECT system/manager
@?/sqlplus/admin/pupbld.sql
```

### 7. Create an SPFILE (Optional but Recommended)

Create a Server Parameter File (SPFILE) from the PFILE for easier parameter management.

```sql
CONNECT / AS SYSDBA
CREATE SPFILE FROM PFILE='<path_to_pfile>/initorcl.ora';
```

### 8. Configure the Listener

Configure the Oracle Net Listener (`listener.ora`) to allow remote connections to the new database.

```bash
lsnrctl start
lsnrctl status
```

## Troubleshooting

- **ORA-01078: failure in processing system parameters**: Check the PFILE for syntax errors or invalid parameter values.
- **ORA-01501: CREATE DATABASE failed**: Check the alert log for detailed error messages. Ensure the directories exist and the Oracle user has write permissions.
- **ORA-01119: error in creating database file**: Ensure the specified path for datafiles is correct and there is sufficient disk space.
