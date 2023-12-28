<div align="center">

# Project
With this project, the goal is to create and secure an Azure SQL Database. Which we will accomplish by completing the following tasks:
<li>Create a basic application environment consisting of a virtual network, Linux VM, and SQL Database</li>
<li>Limit network access to your Azure SQL Database by utilizing server-level firewall IP rules and virtual network access</li>
<li>Create database users and roles to easily manage permissions in your database</li>
<li>Enable Auditing and Temporal Tables to monitor executed queries and changes to your data</li>
<li>Configure a backup policy for your Azure SQL Database</li>

## Task 1: Setting up your environment
On the Home page of the Microsoft Azure portal, select Create a resource. Search for and Create a Virtual Network. Under Project details, verify that the right Subscription and Resource group are selected. Under Instance details, fill in the Name of the Virtual Network (Coursera) and select the Region. Click Next: IP Addresses.

![1](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/4e18a95b-6d6f-48d3-aa20-565ccb4d80e5)

Since we don’t need a large address space, fill in the IPv4 address space (10.2.0.0/24). Click on Add subnet, fill in the Subnet name (default) and Subnet address range (automatically filled in), click Add, then Next: Security. For the scope of this project, we leave all options in the Security tabs disabled.

![2](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/28ff6402-6109-4c7a-80de-5be6d5532e68)

Click on Review + create, verify your configuration and click Create.
Search for and create a Virtual Machine. Under Project details, verify that the right Subscription and Resource group are selected. Under Instance details, fill in the Virtual machine name (CourseraApp). Select the Region, which has to be the same as the Region of the Virtual Network, the image (Ubuntu Server) and Size.

![3](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/4d33d6ad-5b54-450e-90a7-69b47447ea1c)

Under the Administrator account section, set the Authentication type to SSH public key. Fill in the Username (cmm) and Key pair name (cmm_key).

![4](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/2c9750a3-fd6e-416a-b2fa-dbb72bd29238)

Under Inbound port rules, set the Public inbound ports to Allow selected ports and specify the port under Select inbound ports to SSH (22).

![5](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/ecd54b47-0444-4f84-9b7e-4729cd3e1550)

Click Next: Disks. Under Disk options, set the OS disk type to Standard HDD. Click Next: Networking. Under Network interface, verify that the correct Virtual network and Subnet are selected. For the Public IP, leave this on (new). For the NIC network security group, select Basic.

![6](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/522dfc7d-1c43-4699-96d1-a62ffd72ff4a)

Click Next: Management. Under Auto-shutdown, make sure this is Disabled. Then click Review + create, verify your selections, followed by Create. When prompted, click Download private key and create resource. Verify the key is downloaded and save this to a location for later.

![7](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/4d6b8e1d-2acd-41aa-931d-ef457b7e99d0)

Search for and create a SQL database. Under Project details, verify that the right Subscription and Resource group are selected. Under Database details, fill in the Database name (Coursera) and under Server, click the Create new link. Fill in the Server name (crs-sql-srv) and Server admin login (cmm).

![8](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/b362cb08-11e8-4cc0-ad21-1411b025b85f)

Next to Compute + storage, click on the Configure database link and select the basic option.

![9](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/7bf92bb2-6bac-41c8-bc72-3c752194be58)

Click Next: Networking. Under Network connectivity, set the Connectivity method to Public endpoint. (Note: this is not a secure setup, but will be fixed later)

![10](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/619eca1a-54c8-42d7-9c62-55ca63a92700)

Click Next: Additional settings. Under Data source, set Use existing data to Sample. Click Review + create, verify your selections, followed by Create.
  
## Task 2: Limiting network access to your database
Start in the SQL database resource and copy the server name.

![11](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/36cb5d1d-cd90-471f-9a9f-c31ddf9610b7)

Open SQL Server Management Studio and try to log in with your server firewall. You will not be able to connect to the database since your client is not connected to the client IP address. A client IP rule can be either added via the Management Studio or by using the Azure portal.

![12](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/82493732-91a8-457d-b26e-87dee3a977ab)

Go back to the Azure portal and click on Set server firewall, then click on Add client IP followed by Save.

![13](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/a3ea03f5-33da-420a-9af4-c0abb8316729)

Now go back to SQL Server Management Studio and try to connect again. The database should now be accessible. 

If you remove the ClientIPAddress rule, you will still be able to see the database in the Management Studio. However, you will receive an error message when trying to access the database.

Go back to the Azure portal and Allow Azure services and resources to access this server. Click Save.

![14](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/db5dac66-6dea-46ac-b1d0-d71136b2b5c1)

Navigate back to the Resource group, under Overview, select the Virtual Machine. Click Connect via SSH. 

![15](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/112f9b0d-8186-4ac9-a549-06810fbf3814)

Under the Private key path, fill in the name of the private key you created earlier.

![16](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/2a3c24c2-f34c-4483-9b6e-576bad563c49)

Open a new tab in your browser and navigate to shell.azure.com
If you receive a message that no storage is mounted, click on the Show advanced settings link. Verify the selection in the form and click Create storage.

![17](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/159c81d3-3499-44ea-b918-d5fa7f5bfb36)

After the storage has been created, the cloud session will boot up. When ready, click Upload files. Then upload your private key.

![18](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/8eb64854-c72e-4863-a3b7-55949347745d)

Navigate back to the Azure portal Virtual Machine tab and copy the command under step 2.

![19](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/d384c837-847c-4530-ad4f-ea3c9dd531e6)

Navigate back to the cloud session, paste the command in PowerShell but edit the key name. Do the same for the command under step 4.

![20](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/97a28e25-44f9-42c8-bca2-bba261bdeff1)

You are now connected to the Virtual Machine. Before you can connect to the SQL database however, you need to install the required tools. 

Type the following lines into PowerShell:

![99](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/79fb7126-0f41-45ab-8864-f39db2dec042)

This will take several minutes to complete, but will allow you to use SQL command to connect to your database. To do this, enter the following command into PowerShell:
  
  sqlcmd -S tcp:<YOUR_SERVER_NAME>.database.windows.net,1433 -d Coursera -U '<YOUR_USERNAME>' -P '<YOUR_PASSWORD>' -N -l 30

If this has succeeded, you will see a 1> input line waiting for the input, meaning you have successfully connected to the SQL Server.

Next, enter the following command into PowerShell, followed by GO to execute the batch:

![21](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/c6edf0db-77a6-46d9-acb5-7c6e8d3de552)

Type “exit” to close SQL cmd, followed by typing “exit” again to close the SSH session.
Navigate back to the Azure portal and open the SQL database resource. Disable the Allow Azure services and resources to access this server setting in the Firewall settings of your database.

![22](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/568392fd-0be1-47ff-b36b-8c888dd878ed)

Navigate back to the cloudshell tab in your browser. Press the up-arrow to the command to reconnect to your Virtual Machine. Then up-arrow again to repeat the sqlcmd.
Since Azure services are not allowed to access your SQL Server anymore, you will get an error message.
Navigate back to the Azure portal tab in your browser. On the Firewall settings page, Click Add existing virtual network. Fill in the name of the Firewall rule (Coursera) and verify the correct Virtual network and default Subnet is selected. Then click Enable, followed by OK.

![23](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/9f126e65-254c-4eb8-8292-0f7a1b10b6fc)

Navigate back to the cloudshell session and repeat the same steps as before:

![24](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/ffef2c5c-ecf1-4b51-b018-eeb772229c60)

Instead of a public address, there should now be a private IP address:

![25](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/f7c19373-29d2-4d6c-8fa0-49639b582558)

Type “exit” to close sqlcmd.
You will now be able to access the SQL database via your own virtual network. Even though we’ve blocked public access, if we type nslookup <server name>, we’ll see a public IP address.

![26](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/ea3027db-2ce6-4226-8005-1018ddff7b21)

We’ll set up a private endpoint so that our DNS request doesn’t need to leave the virtual network. 
Navigate back to the Azure portal and search for Private Link Center. Under the Overview section, click to Create private endpoint.

![27](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/a13f06ab-e59d-4ed9-9e55-8289d2cc1fdc)

Under Project details, verify the Subscription and Resource group. Under Instance details, fill in the Name (Coursera) and select the Region. Click Next: Resource. For the Resource type, select Microsoft.Sql/servers and verify that the correct Resource is selected. Click Next: Configuration.

![28](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/88885739-dbd2-4438-8dd8-bb0fb4bc6194)

In the Configuration tab, verify that the correct Virtual network and Subnet are selected. Under Private DNS integration, make sure to select Yes to Integrate with private DNS zone and that you’re creating a new privatelink. Click Review + create, verify your selection, followed by Create

![29](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/1e854aaa-f34d-421e-be82-e459694168fd)

Navigate back to the cloudshell tab in your browser and repeat the nslookup command. Instead of a public address, there should now be a private address.

![30](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/f33120ea-4946-45c6-81bc-07298fc3ed3d)

Navigate back to the Azure portal SQL database resource. Click Set server firewall, where you’ll notice that the Deny public access toggle is now enabled.
You can only use this option after you’ve created a private endpoint. This can be used to block external IP access to your database without needing to delete any of the rules from your IP table.

## Task 3: Creating a dedicated database user for your application
In the Azure portal, navigate to the SQL server Overview page. Under Settings, select Active Directory admin and click on Set admin. Search for your username, click Select and Save. 

Open SQL Server Management Studio and connect to your server using the SQL Server admin credentials from before.

Navigate to the folder Databases/System Databases/master and right click master, select New Query.

![31](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/01cf46f7-1d55-4086-8e77-1b71a4f9235e)

Fill in the following statement and execute to create a server login.

![32](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/7a7463c1-23fc-4cea-9f72-7dd79c209490)

Now we need to switch the connection to our Coursera database. Since Azure SQL Database does not support the USE Database command, we can either use the databases dropdown to change the connector of our query windows, or create a new connection and change the default database.

We continue by executing the following statements to create a database user using SQL login, which we previously created, and a contained database user, which will use its own password to log in to the database directly.

![33](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/01d75454-0927-419c-a2b4-20df8795ac23)

We create one more contained user with AD login credentials. 
To do this, open another connection to the database, but now instead of SQL Server Authentication, select Azure Active Directory - Password and log in with your Azure credentials.
Navigate to the folder Databases/System Databases/Coursera, right click Coursera and select New Query. Enter and execute the following statement:
  
  CREATE USER [email_from_your_domain] FROM EXTERNAL PROVIDER WITH DEFAULT_SCHEMA=[dbo]

![34](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/e7e13ebd-5c66-4536-ac14-9cf935ce82c1)

Let's suppose that all three users are used by applications to connect to your database and that they require the same permissions. The easiest way to manage this would be to create a database role for these users, and then assign or deny different permissions to that role.
To create a database role, type and execute the following statement in the query window:

![35](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/1d9bab13-eeb3-466b-88ba-4040044fe978)

Since we do not plan to do advanced permission management, we use dbo as the default schema for the users and as the owner of the created role. 

To add users to the Applications role, type and execute the following statements in the query window, for each user you previously created:
	
 ALTER ROLE Applications ADD MEMBER [user OR email_from_your_domain]

Before the users in the role can access anything, you need to give the role some permissions. Permissions can be given directly to the role or by adding the role to another role.

Suppose your applications are using a framework that generates ad-hoc queries, applications would need permissions to read and write data to the database. To do this, type and execute the following statements:

![36](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/601b4fc9-22fd-4736-bc60-195d0ed98f53)

## Task 4: Azure SQL Database Auditing and Temporal Tables
We set up Database Auditing and Temporal Tables to monitor executed queries and changes to our data.
To do this, we go to the Azure portal and go to the SQL Server resource. Navigate to the side menu, under Security, select Auditing. Select On to Enable Azure SQL Auditing and the checkbox for Storage.

![37](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/22624d42-1633-424a-8c0d-7f06913faafc)

Click on the Storage details - Configure option. (Note, it is recommended to only enable server-level auditing, which by default audits all databases on the server to the same storage. Only enable database-level auditing if you need to use a different storage account, retention period or log specific event types for a certain database.) 

![38](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/925ef3db-c711-4614-a4e4-f0ce45cd3787)

We use the storage account created in the previous task for Azure Shell. A retention policy can be configured, but can be left to 0 for unlimited time. Proceed by clicking OK and saving the configuration. Auditing will now be enabled.

Connect to the database via Microsoft SQL Server Management Studio with the admin account.

Before creating a history table, we create a separate schema to contain the tables. This allows for cleaner code and easier permissions management. type and execute the following statement:
	
![39](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/02e9cb41-a692-44de-b988-859e82457179)

To enable table versioning and create a history table, we add two additional datetime columns that will add a time dimension to the entities to track changes over time. In addition, we add columns that will help identify who changes or inserted the data. (in the script, these are mInsertBy and mModifiedBy. The lowercase m is used as an identifier for meta columns, to easily separate table attributes from metadata columns.)

![40](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/b5534d6a-aa96-4d6d-9053-7f9da9da8608)

By executing a couple of simple update statements, you can change the Middle Name attribute and then use the FOR SYSTEM_TIME ALL option, while querying by primary key and ordering by SysStartTime, to see how the identity changed over time. 

![41](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/d8a5cec5-cea0-4291-863f-2514965bb399)

You can see the Middle Name change, and at the end of the table, there is a start time and end time column, which tell us at which point in time the certain version was active. The modified column hasn’t changed since it wasn’t explicitly updated. 

![42](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/dd254925-b66b-4ee8-9d94-95142d6f6d93)

Lastly, the following statement allows you to turn system versioning off, clean the history and re-enable it. This is useful when testing, so you can work with clean data. (system versioning is only as trustworthy as the database principles that have the permissions to manipulate the database schema and data.)

![43](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/839a0e7b-7f71-42e4-8e05-f2c9cd450fef)

Go back to the Azure portal and the SQL database. Navigate to the side menu, under Security, select Auditing. Click View audit logs.

![44](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/ca9ec85d-9cca-4c2e-98a3-be76e0bdff75)

Click on the Run in Query Editor link and log in with your admin credentials. There’ll be an auto-generated statement that helps query the audit file. In case of large files, edit the WHERE clause to focus the query only on certain databases, statements etc. Press Run to see the results. You’ll find a statement used to clean table history. This would indicate that the table was tampered with and that you need to restore the database from a point before the table was edited.

![45](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/b7c2b02c-f767-4ce1-b6f8-5340b999666e)

## Task 5: Defining a backup policy for you Azure SQL Database
Azure makes it easy to manage backups by offering point-in-time (PITR) and long-term retention (LTR) backups. If you set up a LTR policy, PITR backups are copied to a different blob storage based on the defined schedule, without performance impact on the database workload.

From the Azure portal, open the SQL Server, navigate to the side menu. Under Settings, select Backups. Select the Retention policies tab, click on the Database checkbox, click on Configure policies. 

![46](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/2a046c15-6b13-45a7-8221-d417556cd887)

Usually, you can set up the PITR up to 35 days, but for the Basic tier, you can only set it up to 7 days. For LTR backups, you can define weekly, monthly and yearly backups. All three options are based on weekly increments and work together. For example, if your weekly retention policy is 4 weeks and monthly policy is 12 months, every weekly backup will be kept for 4 weeks except the first backup each month, which will be kept for a year.

Define the backup policy and click Apply.

![47](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/ed0e92de-32a1-4d57-9b60-0b4afce7b867)

Backups won’t be copied immediately after setting up an LTR policy, but as soon as they are copied, you can find them under Available backups. You can restore backups by clicking on the Restore button.

![48](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/a90e0cd5-a2d4-49d4-8420-5d4927786073)

You can also go back to the home page, navigate to your SQL database resource, and click on Restore.

![49](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/92760d6a-9785-4ac2-aaca-84acd04167ed)

Under Source Details, you can Select source and define either PIT or LTR backups.

![50](https://github.com/GeoffreyMorren/Azure-SQL-Database/assets/152500568/bfe204e2-8205-4a66-9d22-a85f4b5bc245)

</div>
