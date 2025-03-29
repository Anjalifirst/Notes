# Add HDB Resale Data into Supabase

## Account Creation
- Create a free account if you have not already done so.
- First, enter a project name
- Next, enter a database password

![alt text](assets/create_db.png)


## Preparing and Download CSV
- Download the csv file on HDB resale price from the website https://data.gov.sg
- Please note that as of March 2025, there should be around 201K rows.

## Importing CSV to Supabase
- Create a table under table editor but do not enter anything yet.
![alt text](assets/Supabase_csv.png)
- Click on load CSV and follow the wizard to complete the import. (Please note that it take a while to upload all the data)
- Next you need to create an id column as there is no primary key during the importation.
- All the columns are in text, which we need to use dbt for transformation.
### Create primary key
- For the imported table you may encounter a warnings saying that there is no primary key.
- Click add column at the table editor
- Select `int8` as datatype
- Unchecked `Allow Nullable`
- Checked `Is Primary Key` and `Is Unique`
- The system will generate a sequence id as primary key.


## Connection with Supabase
To get the connection setting from Supabase, please follow the steps below:

![alt text](assets/connect.png)
- Click connect

![assets/select_type.png](assets/select_type.png)
- You can select any type, the setting should be similar. If you want to test your connection in Python, you can select Python or SQLAlchemy
- If you select Python or SQL Alchemy, they also provided code in Python for us to test the connection. This is useful.
- For demonstration purpose, we stick to URI
- Next we will be given 3 connection methods
- We will focus on 2 connection methods, `direct connection` and `Session pooler (Supavisor)`
- You could try direction connection first, if there is connection issue try session pooler.

### Get Connection Parameters - Direct Connection
![alt text](assets/direct_connection.png)
- Click `View parameters`
- The settings that is related to your database will be presented to you.

### Get Connection Parameters - Session pooler
![alt text](assets/pooler.png)
- Click `View parameters`
- The settings that is related to your database will be presented to you.

### Testing Connection
If you already encounter connection issue with Meltano, you can choose Python or SQLAlchemy
![assets/select_type.png](assets/select_type.png)
Next to the connection string, the code to test the connection is provided.

### Possible Connection Error
In Supabase, it uses IPV6 for direction connection. I being to suspect the problem may depends on our ISP. Anyway, there is a warning as shown:
![alt text](assets/connect_warning.png)

The solution is to try direct connection first and if not working use session pooler.

Reference link: https://supabase.com/docs/guides/database/connecting-to-postgres
