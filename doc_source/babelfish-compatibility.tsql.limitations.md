# T\-SQL differences in Babelfish<a name="babelfish-compatibility.tsql.limitations"></a>

Following, you can find a table of T\-SQL functionality as supported in the current release of Babelfish with some notes about differences in the behavior from that of SQL Server\.

For more information about support in various versions, see [Supported functionality in Babelfish by version](babelfish-compatibility.supported-functionality-table.md)\. For information about features that currently aren't supported, see [Unsupported functionality in Babelfish](babelfish-compatibility.tsql.limitations-unsupported.md)\. 

Babelfish is available with Aurora PostgreSQL\-Compatible Edition\. For more information about Babelfish releases, see the [https://docs.aws.amazon.com/AmazonRDS/latest/AuroraPostgreSQLReleaseNotes/Welcome.html](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraPostgreSQLReleaseNotes/Welcome.html)\.


| Functionality or syntax | Description of behavior or difference | 
| --- | --- | 
| \\ \(line continuation character\) | The line continuation character \(a backslash prior to a newline\) for character and hexadecimal strings isn't currently supported\. For character strings, the backslash\-newline is interpreted as characters in the string\. For hexadecimal strings, backslash\-newline results in a syntax error\.  | 
| @@version | The format of the value returned by `@@version` is slightly different from the value returned by SQL Server\. Your code might not work correctly if it depends on the formatting of `@@version`\. | 
| Aggregate functions | Aggregate functions are partially supported \(AVG, COUNT, COUNT\_BIG, GROUPING, MAX, MIN, STRING\_AGG, and SUM are supported\)\. For a list of unsupported aggregate functions, see [Functions that aren't supported](babelfish-compatibility.tsql.limitations-unsupported.md#babelfish-compatibility.tsql.limitations-unsupported-list4)\. | 
|  ALTER TABLE  | Supports adding or dropping a single column or constraint only\.  | 
| BACKUP statement | Aurora PostgreSQL snapshots of a database are dissimilar to backup files created in SQL Server\. Also, the granularity of when a backup and restore occurs might be different between SQL Server and Aurora PostgreSQL\. | 
| Blank column names with no column alias | The `sqlcmd` and `psql` utilities handle columns with blank names differently: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/babelfish-compatibility.tsql.limitations.html)  | 
| Column default | When creating a column default, the constraint name is ignored\. To drop a column default, use the following syntax: `ALTER TABLE...ALTER COLUMN..DROP DEFAULT...` | 
| Constraints | PostgreSQL doesn't support turning on and turning off individual constraints\. The statement is ignored and a warning is raised\. | 
| Constraints created with DESC \(descending\) columns | Constraints are created with ASC \(ascending\) columns\. | 
| Constraints with IGNORE\_DUP\_KEY | Constraints are created without this property\. | 
| CREATE, ALTER, DROP SERVER ROLE |  ALTER SERVER ROLE is supported only for `sysadmin`\. All other syntax is unsupported\. The T\-SQL user in Babelfish has an experience that is similar to SQL Server for the concepts of a login \(server principal\), a database, and a database user \(database principal\)\. Only the `dbo` user is available in Babelfish user databases\. To operate as the `dbo` user, a login must be a member of the server\-level `sysadmin` role \(`ALTER SERVER ROLE sysadmin ADD MEMBER login`\)\. Logins without `sysadmin` role can currently access only `master` and `tempdb` as the `guest` user\. Currently, because Babelfish supports only the `dbo` user in user databases, all application users must use a login that is a `sysadmin` member\. You can't create a user with lesser privileges, such as read\-only on certain tables\.  | 
| CREATE, ALTER LOGIN clauses are supported with limited syntax | The CREATE LOGIN\.\.\. PASSWORD clause, \.\.\.DEFAULT\_DATABASE clause, and \.\.\.DEFAULT\_LANGUAGE clause are supported\. The ALTER LOGIN\.\.\. PASSWORD clause is supported, but ALTER LOGIN\.\.\. OLD\_PASSWORD clause isn't supported\. Only a login that is a sysadmin member can modify a password\. | 
| CREATE DATABASE case\-sensitive collation  | Case\-sensitive collations aren't supported with the CREATE DATABASE statement\. | 
| CREATE DATABASE keywords and clauses | Options except COLLATE and CONTAINMENT=NONE aren't supported\. The COLLATE clause is accepted and is always set to the value of `babelfishpg_tsql.server_collation_name`\. | 
| CREATE SCHEMA\.\.\. supporting clauses | You can use the CREATE SCHEMA command to create an empty schema\. Use additional commands to create schema objects\. | 
| CREATE, ALTER LOGIN clauses are supported with limited syntax | The CREATE LOGIN\.\.\. PASSWORD clause, \.\.\.DEFAULT\_DATABASE clause, and \.\.\.DEFAULT\_LANGUAGE clause are supported\. The ALTER LOGIN\.\.\. PASSWORD clause is supported, but ALTER LOGIN\.\.\. OLD\_PASSWORD clause isn't supported\. Only a login that is a sysadmin member can modify a password\. | 
| LOGIN objects | All options for LOGIN objects are supported except for PASSWORD, DEFAULT\_DATABASE, ENABLE, DISABLE\. | 
| Database ID values are different on Babelfish  |  The master and tempdb databases won't be database IDs 1 and 2\.  | 
| Identifiers exceeding 63 characters | PostgreSQL supports a maximum of 63 characters for identifiers\. Babelfish converts identifiers longer than 63 characters to a name that includes a hash of the original name\. For example, a table created as "AB\(ABC1234567890123456789012345678901234567890123456789012345678901234567890" might be converted to "ABC123456789012345678901234567890123456789012345678901234567890"\.  | 
| IDENTITY columns support | IDENTITY columns are supported for data types tinyint, smallint, int, bigint\. numeric, and decimal\. SQL Server supports precision to 38 places for data types `numeric` and `decimal` in IDENTITY columns\.PostgreSQL supports precision to 19 places for data types `numeric` and `decimal` in IDENTITY columns\. | 
| Indexes with IGNORE\_DUP\_KEY | Syntax that creates an index that includes IGNORE\_DUP\_KEY creates an index as if this property is omitted\. | 
| Indexes with more than 32 columns | An index can't include more than 32 columns\. Included index columns count toward the maximum in PostgreSQL but not in SQL Server\. | 
| Indexes \(clustered\) | Clustered indexes are created as if NONCLUSTERED was specified\. | 
| Index clauses | The following clauses are ignored: FILLFACTOR, ALLOW\_PAGE\_LOCKS, ALLOW\_ROW\_LOCKS, PAD\_INDEX, STATISTICS\_NORECOMPUTE, OPTIMIZE\_FOR\_SEQUENTIAL\_KEY, SORT\_IN\_TEMPDB, DROP\_EXISTING, ONLINE, COMPRESSION\_DELAY, MAXDOP, and DATA\_COMPRESSION | 
| NEWSEQUENTIALID function | Implemented as NEWID; sequential behavior isn't guaranteed\. When calling `NEWSEQUENTIALID`, PostgreSQL generates a new GUID value\. | 
| OUTER APPLY | SQL Server lateral joins aren't supported\. PostgreSQL provides SQL syntax that allows a lateral join, but the behavior isn't identical\. For more information about PostgreSQL lateral joins, see [the PostgreSQL documentation](https://www.postgresql.org/docs/14/queries-table-expressions.html)\. | 
| OUTPUT clause is supported with the following limitations | OUTPUT and OUTPUT INTO aren't supported in the same DML query\. References to non\-target table of UPDATE or DELETE operations in an OUTPUT clause aren't supported\. OUTPUT\.\.\. DELETED \*, INSERTED \* aren't supported in the same query\. | 
| Procedure or function parameter limit | Babelfish supports a maximum of 100 parameters for a procedure or function\. | 
| RESTORE statement | Aurora PostgreSQL snapshots of a database are dissimilar to backup files created in SQL Server\. Also, the granularity of when the backup and restore occurs might be different between SQL Server and Aurora PostgreSQL\. | 
| ROLLBACK: table variables don't support transactional rollback | Processing might be interrupted if a rollback occurs in a session with table variables\. | 
| ROWGUIDCOL | This clause is currently ignored\. Queries referencing `$GUIDGOL` cause a syntax error\. | 
| SEQUENCE object support | SEQUENCE objects are supported for the data types tinyint, smallint, int, bigint, numeric, and decimal\. Aurora PostgreSQL supports precision to 19 places for data types numeric and decimal in a SEQUENCE\. | 
| Server\-level roles | The `sysadmin` server\-level role is supported\. Other server\-level roles \(other than `sysadmin`\) aren't supported\. | 
| Database\-level roles other than `db_owner` | The `db_owner` database\-level roles is supported\. Other database\-level roles \(other than db\_owner\) aren't supported\. | 
| SQL keyword SPARSE | The keyword SPARSE is accepted and ignored\. | 
| SQL keyword clause `ON filegroup` | This clause is currently ignored\. | 
| SQL keywords `CLUSTERED` and `NONCLUSTERED` for indexes and constraints | Babelfish accepts and ignores the `CLUSTERED` and `NONCLUSTERED` keywords\. | 
| `sysdatabases.cmptlevel` | `sysdatabases.cmptlevel` are always NULL\. | 
| tempdb isn't reinitialized at restart | Permanent objects \(like tables and procedures\) created in tempdb aren't removed when the database is restarted\. | 
| TEXTIMAGE\_ON filegroup | Babelfish ignores the `TEXTIMAGE_ON` *`filegroup`* clause\. | 
| Time precision | Babelfish supports 6\-digit precision for fractional seconds\. No adverse effects are anticipated with this behavior\. | 
| Transaction isolation levels | READUNCOMMITTED is treated the same as READCOMMITTED\. REPEATABLEREAD, and SERIALIZABLE aren't supported\. | 
| Virtual computed columns \(non\-persistent\) | Virtual computed columns are created as persistent\. | 
| WITHOUT SCHEMABINDING clause | This clause isn't supported in functions, procedures, triggers, or views\. The object is created, but as if WITH SCHEMABINDING was specified\. | 