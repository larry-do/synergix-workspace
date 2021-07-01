1. Checkout svn path: https://svn.synergixtech.com/svn/SuperModel
2. Go to {your-checkout-path}\SourceCode\Releases\ExportSchemaApp\stable
3. Open settings.ini and change these values

isTesting=Y
iamahacker=Y
svn.user={svn-username}
svn.pass={svn-password}
postExportCommand=explorer.exe "{your-checkout-path}"
outputPath={your-checkout-path}\\dist
kdiff3Path={your-checkout-path}\\SourceCode\\Releases\\ExportSchemaApp\\stable\\Kdiff\\kdiff3.exe
schemaPath={your-th6-project-path}\\TH6\\src\\main\\resources\\synergix\\th6\\data\\meta

Example

svn.user=larry
svn.pass=passwordoflarry
postExportCommand=explorer.exe "C\:\\LarryWorkspace\\Repo\\SuperModel"
outputPath=C\:\\LarryWorkspace\\Repo\\SuperModel\\dist
kdiff3Path=C\:\\LarryWorkspace\\Repo\\SuperModel\\SourceCode\\Releases\\ExportSchemaApp\\stable\\Kdiff\\kdiff3.exe
schemaPath=C\:\\LarryWorkspace\\Repo\\TH6Larry\\TH6\\src\\main\\resources\\synergix\\th6\\data\\meta

4. Go to {your-checkout-path}\dist
5. Create a file dbconfig.xml follow as file dbconfig - Sample.xml to define your databases.
Example as Project Team DBs

<databases>
	<!-- Project Team DB -->
	<database db="ctrl" alias="ctrl" type="postgresql" url="jdbc:postgresql://172.18.0.22:5432/pjpgctrl" user="postgres" password="Taskhub1" schema="public" category="CTRL"/>
	<database db="main" alias="main1" type="postgresql" url="jdbc:postgresql://172.18.0.22:5432/pjpgmain1" user="postgres" password="Taskhub1" schema="public" category="MAIN"/>
	<database db="main" alias="main2" type="postgresql" url="jdbc:postgresql://172.18.0.22:5432/pjpgmain2" user="postgres" password="Taskhub1" schema="public" category="MAIN"/>
	<database db="main" alias="main3" type="postgresql" url="jdbc:postgresql://172.18.0.22:5432/pjpgmain3" user="postgres" password="Taskhub1" schema="public" category="MAIN"/>
</databases>

6. You set config. Now when you want to sync DB. Repeat from step 7
7. MUST UPDATE SuperModel BEFORE SYNC DB
7. Go to {your-checkout-path}\SourceCode\Releases\ExportSchemaApp\stable
8. Double click to run runGUI.bat (or can run cmd "java -jar ExportSchemaApp.jar")
9. Select 	Schema Path: {your-th6-project-path}\\TH6\\src\\main\\resources\\synergix\\th6\\data\\meta
			Output: {your-SuperModel-checkout-path}\\dist
10. Click Export and wait until complete
11. Go to {your-SuperModel-checkout-path}\dist
12. Run batchrun.bat
13. Enter
	COMMAND: sync
	SCHEMA: modmain
	DATABASE: {db_alias}
14. Wait to complete.
15. Common issues:
	- Synchronize at first time , tool may show error message about "can not find rev.txt, update first"
		But please create a new empty rev.txt before start.
	- Stuck when processing, run this query at ctrl db (PostgreSQL only):
	select pg_terminate_backend(pid) from pg_stat_activity where datname = '{main_db_name}' and client_addr != '{your-ip-address}';
	