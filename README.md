# reproduce-shared-objects-file-issue-pdice-9.4
The kjb, ktr and xml files that can be used to reproduce the Shared objects file issue in Pentaho Data Integration Community Edition 9.4

# Needed environment
(1) Linux
(2) MySQL database engine

# How to reproduce #
(1) Download the file pdi-ce-9.4.0.0-343.zip - https://www.hitachivantara.com/en-us/products/dataops-software/data-integration-analytics/pentaho-community-edition.html
(2) Decompress the zip file to your local
(3) Go to the folder data-integration
(4) Run the command: git clone git@github.com:albertwangnz/reproduce-shared-objects-file-issue-pdice-9.4.git
(5) Go to the folder reproduce-shared-objects-file-issue-pdice-9.4
(6) Create the testdatabase on the MySQL database engine by using the file test.sql
(7) Edit the file sharefiles/database-connections-mysql.xml and modify the following values based on your situation
  (7.1) server
  (7.2) port
  (7.3) username
  (7.4) password
  (7.5) PORT_NUMBER
(8) Go back to the folder data-integration
(9) Copy the file mysql-connector-java-5.1.48.jar to the folder lib
(10) Edit the file reproduce-shared-objects-file-issue-pdice-9.4/use_shared_object_file_variable.ktr
  (10.1) Line 
(10) Hard-code the value of the Run the command: sh kitchen.sh -file="reproduce-shared-objects-file-issue-pdice-9.4/main.kjb"
  (10.1) The log shows the variable DB_CONN_SHARED_FILE is set to: ${Internal.Entry.Current.Directory}/sharefiles/database-connections-mysql.xml
  (10.2) 