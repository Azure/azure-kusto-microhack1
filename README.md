## Microhack 1: Cluster Creation and Data Ingestion

This Microhack is organized into the following 4 challenges:
- Challenge 1: Create an ADX cluster
- Challenge 2: Create integration with Azure services (Event Hub and Storage Account)
- Challenge 3: Starting with the basics of KQL
<!---- Challenge 4: Check stats and key metrics of the cluster -->

Each challenge has a set of tasks that need to be completed in order to move on to the next challenge. It is advisable to complete the challenges and tasks in the prescribed order.

#### Challenge 1: Create an ADX cluster
To use Azure Data Explorer (ADX), you first have to create an ADX cluster, and create one or more databases in that cluster. Each database has tables. Then you can ingest data into a database so that you can run queries against it.

In this challenge, you will design an ADX based architecture, create an ADX cluster and database.
In addition, you will get familiarized with two tools that enable you to connect to your Azure Data Explorer and run queries.

**Expected Learning Outcomes:**
- Deploy ADX cluster from Azure Portal
- Use ADX clients such as Kusto Web Explorer and Kusto Explorer
- The initial configuration of the cluster at creation time

##### Task 1: Create an ADX cluster resource
Sign in to the Azure portal, select the + Create a resource button in the upper-left corner of the portal’s main page.

<img src="/assets/images/Challenge1-Task1-Pic1.png" width="400">
  
- Search for Azure Data Explorer. Under Azure Data Explorer, select Create.

<img src="/assets/images/Challenge1-Task1-Pic2.png" width="450">
<img src="/assets/images/Challenge1-Task1-Pic3.png" width="450">

- Fill out the basic cluster details with the following information.

![Screen capture 1](/assets/images/Challenge1-Task1-Pic4.png)

- Subscription: Use your own subscription
- Resource Group: It's recommended to create a new resource group for the microhack's resources. Call it: <youralias>-microhack-RG
- Cluster name: Must be unique for each participant. Call it: <youralias>microhackadx (cluster name must begin with a letter and contain lowercase alphanumeric characters.)
- Region: France Central
-	Enable performance update (EngineV3): keep the default (enabled)
-	Compute specification: For a production system, select the specification that best meets your needs (storage optimized or compute optimized). For this Microhack we can use the Dev (No SLA) SKU. </br>
With various compute SKU options to choose from, you can optimize costs for the performance and hot-cache requirements for your scenario. If you need the most optimal performance for a high query volume, the ideal SKU should be compute-optimized. If you need to query large volumes of data with relatively lower query load, the storage-optimized SKU can help reduce costs and still provide excellent performance. You can read more about ADX’s SKU types [here](https://docs.microsoft.com/en-us/azure/data-explorer/manage-cluster-choose-sku).
- Availability zones: keep the default.
Move to the next tab (“Scale”). Choose how to scale your resource. Select the “Optimized autoscale” option. It’s always recommended to use this option. Optimized Autoscale is a built-in feature that helps clusters perform their best when demand changes. Optimized Autoscale enables your cluster to be performant and cost effective by adding and removing instances based on demand. For this microhack keep the default values (Minimum instance count == 2, Maximum instance count == 3)

You can keep all the other configurations with the default values. 
Select Review + create to review your cluster details. Then, select Create to provision the cluster. Provisioning typically takes about 10 minutes.
Creating an ADX cluster takes in average 10-15 minutes.

When the deployment is complete, select Go to resource. You will be redirected to the ADX cluster resource page. On the top of the Overview page, you can see the basic details of the cluster, like: the Subscription, the state (running) and the URI.

##### Task 2: Create a Database
- You're now ready for the second step in the process: database creation.
- On the Overview tab, select Create database. Alternatively, you can go to the “Databases” blade.

  ![Screen capture 1](/assets/images/Challenge1-Task2-Pic1.png)

- Fill out the form with the following information.
  
<img src="/assets/images/Challenge1-Task2-Pic2.png" width="450">
  
  | Setting       | Suggested Value   | Field Description                                                             |
  | ------------- | ----------------- | ----------------------------------------------------------------------------- |
  | Admin         | Default selected  | The admin field is disabled. New admins can be added after database creation. |
  | Database Name | TelemetryDatabase | The database name must be unique within the cluster.                          |
  | Retention period | 365	| The time span (in days) for which it's guaranteed that the data is kept available to query. The time span is measured from the time that data is ingested. This is the longer-term storage (in reliable storage) retention. |
  | Cache period	| 31	| The time span (in days) for which to keep frequently queried data available in SSD storage or RAM of the cluster’s VM, rather than in longer-term storage. Azure Data Explorer stores all its ingested data in reliable storage (most commonly Azure Blob Storage), away from its actual processing (such as Azure Compute) nodes. To speed up queries on that data, Azure Data Explorer caches it, or parts of it, on its processing nodes, SSD, or even in RAM. The best query performance is achieved when all ingested data is cached. Sometimes, certain data doesn't justify the cost of keeping it "warm" in local SSD storage. For example, many teams consider that rarely accessed older log records are of lesser importance. They prefer to have reduced performance when querying this data, rather than pay to keep it warm all the time. By increasing the cache policy, more VMs will be required to store data on their SSD/RAM. For Azure Data Explorer cluster, compute cost (VMs) is the most significant part of cluster cost as compared to storage and networking. |

- Select Create to create the database. Creation typically takes less than a minute. When the process is complete, you're back on the cluster Overview blade. You can see the database that you have created from on the Databases blade.

  ![Screen capture 1](/assets/images/Challenge1-Task2-Pic3.png)
  
##### Task 3: Write your first Kusto Query Language (KQL) query
  What is a Kusto query?
  Azure Data Explorer provides a web experience that enables you to connect to your Azure Data Explorer clusters and write and run Kusto Query Language queries. The web experience is available in the Azure portal and as a stand-alone web application, the Azure Data Explorer Web UI, that we will use later.
  A Kusto query is a read-only request to process data and return results. The request is stated in plain text that's easy to read. A Kusto query has one or more query statements and returns data in a tabular or graph format.
  In the next challenges, we'll ingest data to the cluster, and then learn the most important concepts in KQL and write interesting queries. In this task, you will write a few basic queries to get an understanding of the environment.
  To start, go to the “Query” blade. In this example, you'll use the Azure Data Explorer web interface as a query editor (Kusto Query Language can also be used in Azure Monitor Logs, Azure Sentinel, and other services that are built on-top of Azure Data Explorer.)
  
  ![Screen capture 1](/assets/images/Challenge1-Task3-Pic1.png)
  
  We can see our cluster and the database that we created.
To tun KQL queries, we must select the database that the query will run on (the scope). 
To select the data base, just click on the database name.
Now – you can write a simple KQL query: print ("hello world"),
and hit the “Run” button. The query will be executed and its result can be seen in the result grid on the bottom of the page. 
  
  ![Screen capture 1](/assets/images/Challenge1-Task3-Pic2.png)
  
  You can also download [Kusto Explorer](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/tools/kusto-explorer), the desktop client to run queries and benefit from some advanced features available in the client.

##### Task 4: Enable Diagnostic logs
  Azure Monitor diagnostic logs provide monitoring data about the operation of Azure resources. ADX uses diagnostic logs for insights on ingestion, commands, queries, and tables usage. You can export operation logs to Azure Storage, Event Hub, or Log Analytics.
  Diagnostic logs are disabled by default. To enable diagnostic logs, go to your cluster page in the portal. Under Monitoring, select Diagnostic settings. 

  ![Screen capture 1](/assets/images/Challenge1-Task4-Pic1.png)
  
  Select Add diagnostic setting. In the Diagnostic settings window. Enter a Diagnostic setting name as per your preference. 
Select all the log categories and metrics (SucceededIngestion, FailedIngestion, IngestionBatching, Command, or Query, TableUsageStatistics, TableDetails and Journal).
For this microhack, select the Destination details to be a Log Analytics workspace and select your own workspace or create a new one.
Save the new diagnostic logs settings and metrics.

  
#### Challenge 2: Create integration with Azure services (Event Hub and Storage Account)
  
  Data ingestion to ADX is the process used to load data records from one or more sources into a table in your ADX cluster. Once ingested, the data becomes available for query.

  ADX supports several ingestion methods. [These methods include ingestion tools, connectors and plugins, managed pipelines, programmatic ingestion using SDKs, and direct access to ingestion.]

  **Expected Learning Outcomes:**
  - Create continuous ingestion from Azure Event Hub (a managed pipeline) 
  - Create one-time ingestion from Azure Blob Storage to your ADX cluster.

  ##### Task 1: Use the “One-click” UI (User Interfaces) to create a data connection to Event Hub
For the best user experience, we will use the Azure Data Explorer Web UI (aka: Kusto web Explorer/KWE). To open it, click on the “Open in Web UI” or just go to [Kusto Web Explorer](https://dataexplorer.azure.com)
  
  ![Screen capture 1](/assets/images/Challenge2-Task1-Pic1.png)
  
  The web UI opens. The messages are in a JSON format and they are being sent to your event hub. This is how a sample message looks like:
  ```
  {
  "messageProperties": {
    "iothub-creation-time-utc": "2021-12-22T11:16:58.668Z"
  },
  "enrichments": {},
  "applicationId": "1b2f5f29-a78b-4012-bf31-2016473cadf6",
  "deviceId": "13n5b9yiael",
  "messageSource": "telemetry",
  "telemetry": {
    "Status": "Online",
    "BatteryLife": 52,
    "Light": 62602.66864777621,
    "Tilt": 44.70275959833819,
    "Shock": -6.381560743718394,
    "ActiveTags": 164,
    "Location": {
      "lon": -68.4585,
      "lat": 40.9633,
      "alt": 1069.4617
    },
    "TransportationMode": "Train",
    "LostTags": 5,
    "Temp": 13.178830710467722,
    "Humidity": 91.17280445807984,
    "Pressure": 1033.3527307505506,
    "TotalTags": 187
  },
  "schema": "default@v1",
  "enqueuedTime": "2021-12-22T11:16:58.753Z",
  "templateId": "dtmi:ltifbs50b:mecybcwqm"
}
```

KWE lets us easily connect to Azure Event Hub and build a table which is schema based on an event sample data. Go to the “Data” blade. The name of this capability is “One-click ingestion” and it allows you to quickly ingest data.
  
<img src="/assets/images/Challenge2-Task1-Pic2.png" width="500">

  ##### Task 2: Configure the Event hub data connection
  
  The ’Ingest new data’ wizard opens. 
  
  Destination tab:
The Cluster and Database fields are auto-populated. Select the ADX cluster and the Database that you created in challenge 1. We haven’t created a table, so use the “Create new table” option. In order to make it easier to query, we recommend using a table name without hyphens, '-'. </br>
The table will be named LogisticsTelemetry.
  
  Source tab:
Set the Source type to “Event Hub”, and specify the event hub details:
- Subscription: Your event hub's subscription
- Event hub namespace: Your event hub's namespace
- Event hub: Your event hub name
- Data connection name: Set a name for your data connection. We used ‘Database1-adx-microhack-eh’. Data connection connects ADX database to Event hub (or to storage account through Event Grid notifications)
- Consumer group: you can use the default one
- Compression: None
  Event system properties: leave empty. For this Microhack, we are not going to use them. (System properties store properties (meta data) that are set by the Event Hubs service, at the time the event is enqueued. ADX can embed the selected properties into a new column in your destination table.)
  
  <img src="/assets/images/Challenge2-Task2-Pic1.png" width="500">

  Click on “Next: Schema” 
  
  Schema tab:
  
  Sample data will be read from the Event Hub and you will see data preview. The default format is TXT, but our Event hub sends JSON data.
Change the Data format field to JSON. Keep the nested levels as 1. As you can see, ADX inferred the column names and the data type according to the JSON’s data.
Among the types you can find GUID, string, datetime, and dynamic.</br>
You can think of dynamic column as a JSON-like type. The dynamic data type can take on any value of other scalar data types like: bool, datetime, guid, int, long, real, string, timespan, an array of dynamic values, and a property bag (dictionary) that maps unique string values to dynamic values.</br>
Although the dynamic type appears JSON-like, it can hold values that the JSON model does not represent because they don't exist in JSON (e.g., long, real, datetime, timespan, and guid).

The 'Nested levels' field expands levels of nested data in dynamic type columns into separate columns. Although the raw event’s JSON format has nestedness of 2 levels, for this microhack we will use 1 level and see later how to leverage the powerful update policy capability of ADX to break these dynamic columns.

This is an example of the telemetry JSON (that is part of a bigger JSON that is being sent from the evet hub):
```
{"Location":{"alt":"252.71910000000003","lon":-93.2176,"lat":41.7911},"LostTags":4,"Light":"49115.368835522917","Temp":"32.93780098864795","TotalTags":186,"Status":"Offline","TransportationMode":"Land","BatteryLife":-5,"Tilt":"-52.64112596209344","Humidity":"74.018336518734131","Shock":"6.696957328744805","Pressure":"603.69265616418761","ActiveTags":170}
```
  ![Screen capture 1](/assets/images/Challenge2-Task2-Pic2.png)
  
  If you set the Nested levels to 2, ADX will “break” the JSON to independent fields.
  
  ![Screen capture 1](/assets/images/Challenge2-Task2-Pic2-2.png)
  
  For this microhack, we want to learn how ADX deals with dynamic fields, so we will keep the Nested levels as 1.
  
  <img src="/assets/images/Challenge2-Task2-Pic3.png" width="450">
  
  ![Screen capture 1](/assets/images/Challenge2-Task2-Pic4.png)
  
  Open the command viewer. You can see the control commands that were automatically generated.
  In contrast to Kusto queries, control commands are requests to Kusto to process or modify data or metadata. Control commands are distinguished from queries by having the first character in the text of the command be the dot (.) character (which can't start a query). Not all control commands modify data or metadata. The large class of commands that start with .show, are used to display metadata or data. For example, the .show tables command returns a list of all tables in the current database.
  
  Review the control commands that were generated by One-Click. 
  
  ![Screen capture 1](/assets/images/Challenge2-Task2-Pic5.png)
  
  You can see the:
  '.create table table_name command', which creates a new empty table.
 '.alter table table_name policy ingestionbatching', which alters the batching policy of the specified table. During the ingestion process, ADX optimizes for throughput by batching small ingress data chunks together before ingestion. Batching reduces the resources consumed by the ingestion process. The batching policy defines when to seal a batch and send it for the next stage of the ingestion (once the first condition is met):
  
  | Parameter | Value | Description |
  | --------- | ----- | ----------- |
  | Size | 1G | Batch size limit reached or exceeded the configured size |
  | Count | 1000 files | Batch file (blob) number limit reached the configured count |
  | Time | 5 minutes | The configured batching time has expired (maximum delay time per batch) (One click sets the batching time to 30 seconds) |

The table mapping (data mapping) command: Data mappings are used during ingestion to map incoming data to columns inside tables. In our case, the incoming data arrives from the event hub in JSON format. The table mapping maps the JSON fields (by describing the path to elements in a JSON document) to our table’s columns.
The desired result:

![Screen capture 1](/assets/images/Challenge2-Task2-Pic6.png)
  
  Click on ”Next: Summary” to create this data connection.

  The Azure portal opens on the Event Hubs Instance page, so you can monitor the Event hub’s outgoing messages. In addition, in the One click’s **Continuous ingestion from Event Hub established** window, all steps should be marked with green check marks when establishment finishes successfully. The cards below these steps give you options to explore your data with Quick queries or Monitor the Event Hub connections and data. 
  
  Use the “Take 10” link (under “Quick queries”). Review the query and the data.
  
  Notice that the query begins with a reference to the data table. This data is piped into the first and only operator in our query (take) and returns a specific number of arbitrary rows.
  Run the query by either selecting the Run button above the query window or selecting Shift+Enter on the keyboard.
  Use the “Manage Data Connection” link (under the “Monitor” section) to go to the portal and review your data connection. The data connection is saved under the Database.
  
<img src="/assets/images/Challenge2-Task2-Pic7.png" width="600">

  ##### Task 3: Use the “One-click” UI (User Interfaces) to create a data connection to Azure blob storage
  
  This time, we will ingest data from an Azure Storage account. We will ingest two datasets: </br>
    1. Logistics telemetry data. This time, the table will be named LogisticsTelemetryHistorical.  </br> 
    2. Data on New York City taxi rides, which will be used for Microhack 2
  
  Go again to the “Data management” tab, and select the **Ingest from blob container** option under **Continuous ingestion**
  
  <img src="/assets/images/Challenge2-Task3-Pic1.png" width="450">
  
  Make sure the cluster and the Database fields are correct. Select **Create new table**
  
  <img src="/assets/images/Challenge2-Task3-Pic2.png" width="450">
  
  In the **Link to source**, paste the SAS URL of the blob storage (the proctors will provide this information). Then select one of the **Schema defining file** (all the files in that blob storage have the same schema) and click **Next**
  
  ![Screen capture 1](/assets/images/Challenge2-Task3-Pic3.png)
  
  Make sure you use the **JSON Data format**
  
  ![Screen capture 1](/assets/images/Challenge2-Task3-Pic4.png)
  
  Wait for the ingestion to be completed. For production modes, you could use Azure Event Grid for continuous Blob ingestion. The **Event Grid** link under **Continuous Ingestion** will create the Event Grid resource for that. We won't use this option in this Microhack.

   <img src="/assets/images/ingestion-completed.png" width="520">
  
  <!--- Event Grid Continuous ingestion images 
![Screen capture 1](/assets/images/Challenge2-Task3-Pic6.png)
 ![Screen capture 1](/assets/images/Challenge2-Task3-Pic7.png)  --->
  
  Verify that data was ingested to the table
  ```
  LogisticsTelemetryHistorical
  | count 
  ```

Repeat the above steps for ingesting data from the New York City container.

  **Relevant docs for this challenge:**
  - [Azure Data Explorer data ingestion overview | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/ingest-data-overview)
  - [Use one-click ingestion to ingest data into Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/ingest-data-one-click)
   

### Challenge 3: Starting with the basics of KQL

In this challenge you’ll write queries in Kusto Query Language (KQL) to explore and gain insights from your data. 

Expected Learning Outcomes:
- Know how to write queries with KQL.
- Use KQL to explore data by using the most common operators.

**What is a Kusto query?**
A Kusto query is a read-only request to process data and return results. The request is stated in plain text that's easy to read, author, and automate. A Kusto query has one or more query statements and returns data in a tabular or graph format.

**What is a tabular statement?**
The most common kind of query statement is a tabular expression statement. Both its input and its output consist of tables or tabular datasets.

Tabular statements contain zero or more operators. Each operator starts with a tabular input and returns a tabular output. Operators are sequenced by a pipe (|). Data flows, or is piped, from one operator to the next. The data is filtered or manipulated at each step and then fed into the following step.

It's like a funnel, where you start out with an entire data table. Each time the data passes through another operator, it's filtered, rearranged, or summarized. Because the piping of information from one operator to another is sequential, the query's operator order is important. At the end of the funnel, you're left with a refined output.
Let's look at an example query:

```
LogisticsTelemetryHistorical
| where enqueuedTime > ago(7d) 
| where messageSource == "telemetry"
| count 
```

This query has a single tabular expression statement. The statement begins with a reference to the table LogisticsTelemetry and contains the operators where and count. Each operator is separated by a pipe. The data rows for the source table are filtered by the value of the enqueuedTime column and then filtered by the value of the messageSource column. In the last line, the query returns a table with a single column and a single row that contains the count of the remaining rows.

References:
- [SQL to Kusto cheat sheet](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/sqlcheatsheet)
- [KQL cheat sheets](https://github.com/marcusbakker/KQL/blob/master/kql_cheat_sheet.pdf)

<!--
#### Task 0: Connect to the cluster

For the following tasks, connect to the cluster [ADX Microhack Cluster](https://adxmicrohackcluster.eastus.kusto.windows.net/)

![Screen capture 1](/assets/images/Challenge5-Task0-Pic1.png)

![Screen capture 1](/assets/images/Challenge5-Task0-Pic2.png)

-->

##### Task 1: Basic KQL queries - explore the data
  
In this task, you will see some KQL examples. For this task, we will use the table LogisticsTelemetry, which obtains data from the event hub. </br> 
Execute the queries and view the results. KQL queries can be used to filter data and return specific information. Now, you'll learn how to choose specific rows of the data. The where operator filters results that satisfy a certain condition. 

```
LogisticsTelemetry
| where deviceId startswith "x"
| take 10
```

Similarly, you can filter where the time of an event occurred more than a certain number of years/days/minutes ago. For example, run the following query, where 2m means 2 minutes:

  ```
LogisticsTelemetry
| where enqueuedTime > ago(2m)
| take 10 
  ```

Find out how many records are in the table

  ```
LogisticsTelemetry
| summarize count() // or: count
  ```

Find out how many records have enqueuedTime bigger than the last 10 minutes.

  ```
LogisticsTelemetry
| where enqueuedTime > ago(10m)
| summarize count()
 ``` 
 
Find out how many records have deviceId that startswith "x" 

```
LogisticsTelemetry
| where deviceId startswith "x"
| summarize count()
```
  
Find out how many records have deviceId that startswith "x", per device ID (aggregate by device ID)

```
LogisticsTelemetry
| where deviceId startswith "x"
| summarize count() by deviceId
```
  
Find out how many records startswith "x", per device ID (aggregate by device ID). Render a timechart

```
LogisticsTelemetry
| where deviceId startswith "x"
| summarize count() by deviceId
| render piechart 
```

KQL makes it simple to access fields in JSON and treat them like an independent column:

```
LogisticsTelemetry
// | where enqueuedTime > ago(10d) 
| extend h = telemetry.Humidity
| summarize avg(toint(h)) by bin(enqueuedTime, 1h)
| render timechart 
```

For the following tasks, we will use the table LogisticsTelemetryHistorical.

#### Task 2: Explore the table and columns
Write a query to get the schema of the table. 

Expected result:  
<img src="/assets/images/Schema.png" width="400">

[getschema operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/getschemaoperator)

#### Task 3: Keep the columns of your interest
Write a query to get only specific desired columns: deviceId, enqueuedTime, Temp. Take arbitrary 10 records.

Expected result:</br>
<img src="/assets/images/project.png" width="400">

[project-away operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/projectawayoperator)

[Project operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/projectoperator)

#### Task 4: Filter the output
Write a query to get only specific desired columns: deviceId, enqueuedTime, Temp. Take arbitrary 10 records from the past 90 days.

Hint 1: “ago” </br>
Hint 2: In case you see 0 records, remember that operators are sequenced by a pipe (|). Data is piped, from one operator to the next. The data is filtered or manipulated at each step and then fed into the following step. By using the ‘Take’ operator, there is no guarantee which records are returned

[where operator in Kusto query language - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/whereoperator)

#### Task 5: Sorting the results
Write a query to get the 5 records which have the highest temperature. Write another query get the 5 records which have the lowest temperature.

[sort operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/sortoperator)

#### Task 6: Reorder, rename, add columns
Write a query to convert Fahrenheit temperatures to Celsius temperatures. For readability, show the Fahrenheit temperature and the Celsius temperaturesa as the 2 left-most columns. You can use the following formula: 
C = (F – 32) * (5.0/9.0) <br>
Take 5 random records from the past week.
Hint 1: 'project' operator provides lot more features
Hint 2: We used 5.0 and 9.0, rather than 5 and 9 to ensure these numbers were to the 'real' data type (double-precision floating-point format), rather than 'long' (a signed integer, Int64)

Expected result:</br>
<img src="/assets/images/temp.png" width="600">

[extend operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/extendoperator)
[project-rename operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/projectrenameoperator)
[project-reorder operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/projectreorderoperator)


#### Task 7: Total number of records
Write a query to find out how many records are in the table. 

[count operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/countoperator)

#### Task 8: Aggregations and string operations
Write a query to find out how many records have deviceId starting with 'x'. <br>
Write another query to find out how many records have deviceId starting with 'x', per device ID (aggregated by deviceId).</br>
Expected result for the second query:</br>
<img src="/assets/images/count_by.png" width="250">

[String operators - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/datatypes-string-operators)

[summarize operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/summarizeoperator)

#### Task 9: Render a chart
Write a query to find out how many records startswith "x" , per device ID (aggregated by device ID) and render a piechart.

Expected result:</br>
<img src="/assets/images/pie.png" width="500">

[render operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/renderoperator?pivots=azuredataexplorer)

#### Task 10: Create bins and visualize time series 
Write a query to show a timechart of the number of records over time. Use 10 minute bins (buckets). Each point on the timechart represent the number of devices on that bucket.

Expected result:</br>
<img src="/assets/images/chart.png" width="650">

[bin() - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/binfunction)

#### Task 11: Aggregations with time series visualizations
Write a query to show a timechart of the **average temperature** over time. Use 30 minute bins (buckets) Each point on the timechart represent the average temperature in that 30 min period.
Hint: summarize avg(Temp) by bin(enqueuedTime, 30m) 

Expected result:</br>
<img src="/assets/images/timeseries.png" width="650">

[summarize operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/summarizeoperator)

<!---

#### Challenge 4: Check stats and key metrics of the cluster
    
  ##### Task 1:  #####
  Go to the Insights blade in the portal (in the ADX cluster page, under monitoring). This blade provides comprehensive monitoring of your clusters by delivering a unified view of your cluster performance, operations, usage, and ingestion operations.
  
  ##### Task 2: #####
  Go to the Overview tab: It provides metrics tiles that highlight the availability and overall status of the cluster for quick health assessment. A summary of active Azure Advisor recommendations and resource health status. Charts that show the top CPU and memory consumers and the number of unique users over time.
  
  ##### Task 3: #####
  Go to the Key Metrics tab: It shows a unified view of some of the cluster's metrics. They're grouped into general metrics, query-related metrics, ingestion-related metrics, and streaming ingestion-related metrics.
  
  ##### Task 4: #####
  Go to The Ingestion tab: It provides details about the ingestion operations, including the result of your ingestion attempts (per DB of per table), the latency of the ingestion process, and more.
  
  ##### Task 5: #####
  Go to the Overview tab: You can stop the cluster to save compute costs. You will not lose any data. ADX persists data on blob storage. When you restart your cluster, it will take few minutes to startup and warm up the cache before you can start writing the queries. When the cluster has been stopped, no continuous ingestion will be performed.
  
**Relevant docs for this challenge:**
  - [Monitor Azure Data Explorer performance, health & usage with metrics | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/using-metrics)

-->
