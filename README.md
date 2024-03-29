# Azure_DataFactory_pipeline_Orchestration

## Project architecture :

![Screenshot (486)](https://github.com/shekharj21/shekharj21/assets/54074505/a00853b0-135f-4cac-96d5-9f1dc02b64e1)
1. ADLS : We will be using ADLS as an input the Data into the Azure Data Factory.
2. Azure Data Factory : We will be using azure datafactory to transform the data and for orchestration purpose.
3. AZURE SQL : As a Destination to dump the data.

## Create a Resource group :
1. Create a resource group in azure portal.
2. Its all the simple information.

## Deploy Storage Account :


![Screenshot (487)](https://github.com/shekharj21/shekharj21/assets/54074505/3749c371-8bde-44d4-b34f-99273b7c7ab8)


1. go to resource group.
2.  click on create and search for "Storage Account".
3.  create storage account and give all the details like global name etc.
4.  in the advanaced section there is a option to choose named as Data Lake storage Gen2 or hierarchical namespace. we are going to use data lake as input so check the box named as "Enable hierarchical namespace" in Data lake Gen2.This will create the data lake. if we don't check it then azure will create normal storage.
5.  Just keep all the settings as default in other sections.
6.  Create Storage account.
   
## Deploy Azure Data Factory :
![Screenshot (488)](https://github.com/shekharj21/shekharj21/assets/54074505/9f7ae069-9dff-4633-8633-38bddeca7274)

1. Search data Factories in add resource in home tab.
2. select reosurce group, naame and region.
3. keep all other section settings as default and select create.

## Deploy Azure SQL DB :

![Screenshot (489)](https://github.com/shekharj21/shekharj21/assets/54074505/1335e164-d4f5-4b6a-b663-d2d7587c94b2)

1. Find Azure SQL DB.
2. add resouce group, DB name. we don't have server so there is a option "create new" under Server. Click it.
3. Give all the detailsa and choose SQL authentication and create server.
4. After that Go to "compute + storage" and choose the bare minimum plan FOR COST SAVING.
5. Keep all other section setting as default.
6. Create a SQL DB.
7. after creaing Db, go to the Db and choose set firewall access and add your own IPv4 Address.
8. go and then check qurey editior (access on the left section of DB).

## Create Storeage Container and upload input File :
![Screenshot (490)](https://github.com/shekharj21/shekharj21/assets/54074505/32abe05c-1b4e-48eb-b9c6-06cdda32157f)

1. Click on resource group and then click on the storage account.
2. click on Data lake Storage and click on "+ Container".
3. give a name to it and create it.
4. click on the created container and add input file by clicking on upload.

## Create Linked Service in Azure data Factory :
![Screenshot (491)](https://github.com/shekharj21/shekharj21/assets/54074505/c0eef0b5-336b-4db3-a4b1-23811b63e978)

1. azure data factory needs to know the connectivity between Azure data lake and Azure SQL Database. We will use the liked service (connectivity information) to help Azure data factory connect with with input (ADLS) and output (Azure SQL).
2. Go to Resource group. (we need to create 2 linked service i.e. for storage and datalake)
3. open data factory studio -----> monitor ------> Linked Services.
4. choose name and details. test the connection and create it.
5. craete a linked service for Azure SQL Database too.
6. choose all the information and create it.


## Create Dataset for Azure Data Factory :
![Screenshot (492)](https://github.com/shekharj21/shekharj21/assets/54074505/732a01f5-9fa9-4a9b-8f56-19e023e50a9e)

(we need to create two datasets i.e. for ADLS and for SQL)
1. Go to Data Factory.
2. Click Author -----> Dataset ----> New Dataset.
3. add a first dataset for input (ADLS) by typing DATA LAKE and choose file format.
4. Fill all the info and create a dataset for ADLS.
5. Create same for the SQL Database.
   
## Create Data Flow Job in Azure data Factory :

![Screenshot (495)](https://github.com/shekharj21/shekharj21/assets/54074505/50842e03-6da4-4e1c-b396-af3102a43c07)


1. Open Azure data Factory and choose Data Flows.
2. Give the name to DataFlow.
3. click on add source and give name to source and choose dataset which we have created for input.
4. Make Sure to Check validate Data Flowing Debug and test connection. now we can check data Preview.
5. Click + on Source and we can filter the data from a large dataset.
6. add an expression to filterout the data.
7. we want to find avg rating by year then we will perform operations on that data by clicking + on the filtered data tag.
8. we will choose group by "Year" and at aggreagate option we will write Query in the expression builder.
9. we will use sink option by clicking + on the average rating tag to dump the data in SQL Database.
10. Make Sure you Publish all in order to save it.


## Create and Execute the pipeline in the azure data Factory :
![Screenshot (496)](https://github.com/shekharj21/shekharj21/assets/54074505/d97c60fe-f669-4e51-8aea-df221a95fe36)

1. click on pipeline option in Data Factory Author.
2. Choose a name and minimise.
3. We are using dataflows as a activity for that search Data Flow and drag and drop.
4. choose a dataflow name.
5. publish it.
6. Click on add trigger and select ok for pipeline to run.
7. after the successful creation go to the Azure SQL DB and check for a new table.
8. If you see the table and data that means we created a pipeline succesfully.

## Create a Trigger and run the data Pipeline Successfully :

![Screenshot (497)](https://github.com/shekharj21/shekharj21/assets/54074505/cd4e95ca-be3a-4b42-b71f-c5b7ab4c3292)

1. After Successfully transimmision of data from input (Data lake) to output (SQL DB), now we need to remove the data from Data Lake and delete the table in SQL DB too because we are creating a trigger that whenever the data comes into the Data Lake, it will be moved to SQL DB.
2. go to the pipeline and choose create trigger and choose the following setting (minimal).
3. run the trigger so now the data from any .csv file uploaded in DATA LAKE will be exported to AZURE SQL DB.
   



