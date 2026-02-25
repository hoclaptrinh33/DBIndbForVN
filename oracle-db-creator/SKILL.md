---
name: oracle-db-creator
description: Comprehensive guide and templates for creating a new Oracle Container Database (CDB) from scratch on a local server using SQL*Plus. Use this skill when the user asks to create a new Oracle database instance, set up a new CDB, or initialize an Oracle database environment manually without DBCA.
---

# Oracle Database Creator

This skill provides the workflow and templates required to manually create a new Oracle Container Database (CDB) on a local server using SQL\*Plus.

## Quick Start

When a user requests to create a new Oracle database:

1. **Gather Requirements**: Ask the user for the desired `DB_NAME`, `ORACLE_SID`, and the base directory for datafiles (e.g., `/u01/app/oracle/oradata`).
2. **Review Workflow**: Read the detailed step-by-step guide in [references/create_db_workflow.md](references/create_db_workflow.md).
3. **Prepare Environment**: Guide the user to set up the necessary directories and environment variables.
4. **Generate Files**: Use the templates in the `assets/` directory to generate the initialization parameter file (`init.ora`) and the database creation script (`create_db.sql`).
5. **Execute**: Provide the user with the exact SQL\*Plus commands to execute the scripts and build the data dictionary.

## Bundled Resources

### References

- **[references/create_db_workflow.md](references/create_db_workflow.md)**: The complete, step-by-step manual process for creating an Oracle database from scratch. Read this file to understand the exact sequence of commands and scripts required.

### Assets

- **[assets/init.ora.template](assets/init.ora.template)**: A template for the initialization parameter file (PFILE). Use this to generate the `init<SID>.ora` file.
- **[assets/create_db.sql.template](assets/create_db.sql.template)**: A template for the `CREATE DATABASE` SQL statement. Use this to generate the script that actually creates the database files and structure.

## Important Considerations

- **Manual Creation vs DBCA**: This skill focuses on the manual creation process using SQL\*Plus. This is useful for learning, scripting, or when a GUI (Database Configuration Assistant - DBCA) is not available.
- **Data Dictionary Scripts**: After the `CREATE DATABASE` statement succeeds, it is critical to run the catalog and catproc scripts to build the data dictionary views and PL/SQL packages.
- **Paths**: Ensure all paths in the templates are updated to match the user's actual server environment (Windows or Linux).
