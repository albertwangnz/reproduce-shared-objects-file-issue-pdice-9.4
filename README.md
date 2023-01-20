# Project description
The kjb, ktr and xml files that can be used to reproduce the Shared objects file issue in Pentaho Data Integration Community Edition 9.4.

## The issue
When we used PDI-CE 8.0, we used the feature `Shared objects file` in the transformation Miscellaneous tab. We defined database connections in different files to support different database engines. We set up a variable `DB_CONN_SHARED_FILE` in a previous transformation step. The value of the variable is the location of a shared object file (XML), like `sharefiles/database-connections-mysql.xml`. Then we used the variable as the value of the Shared objects file field as shown below. So, we could dynamically support various database engines, like MySQL, SQL Server, with the same `*.ktr` files.

It used to work in PDI-CE 8.0. But after we upgraded to PDI-CE 9.4, it does not work anymore. If we hard-code the value in the Shared objects file field, as shown below, like `sharefiles/database-connections-mysql.xml`, it works. But the variable does not work anymore.

## Needed environment to reproduce
1. Linux
2. MySQL database engine

## How to reproduce
### Prepare
1. Download the file `pdi-ce-9.4.0.0-343.zip` - https://www.hitachivantara.com/en-us/products/dataops-software/data-integration-analytics/pentaho-community-edition.html
2. Decompress the zip file to your local
3. Go to the folder `data-integration`
4. Run the command: `git clone git@github.com:albertwangnz/reproduce-shared-objects-file-issue-pdice-9.4.git`
5. Go to the folder reproduce-shared-objects-file-issue-pdice-9.4
6. Create the `testdatabase` on the MySQL database engine by using the file `test.sql`
7. Edit the file `sharefiles/database-connections-mysql.xml` and modify the following values based on your situation
  - server
  - port
  - username
  - password
  - PORT_NUMBER
8. Go back to the folder `data-integration`
9. Copy the file `mysql-connector-java-5.1.48.jar` to the folder `lib`

### Reproduce the scenario of using hard-code value in Shared objects file
1. Edit the file `reproduce-shared-objects-file-issue-pdice-9.4/use_shared_object_file_variable.ktr`
  - Line #419 shows we currenty use a hard-code value of the `Shared objects file`
2. Run the command: `sh kitchen.sh -file="reproduce-shared-objects-file-issue-pdice-9.4/main.kjb"`
3. You will see the ETL runs without error
4. In the log, you will see the variable `DB_CONN_SHARED_FILE` is set value to `${Internal.Entry.Current.Directory}/sharefiles/database-connections-mysql.xml`, even though we don't use the variable in this transformation.

### Reproduce the scenario of using variable in Shared objects file
1. Edit the file `reproduce-shared-objects-file-issue-pdice-9.4/use_shared_object_file_variable.ktr`
  - Comment line #419
  - Uncomment line #418, now we use variable
2. Run the command: `sh kitchen.sh -file="reproduce-shared-objects-file-issue-pdice-9.4/main.kjb"`
3. You will see the ETL is failed