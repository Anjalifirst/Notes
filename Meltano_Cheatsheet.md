# meltano Cheatsheet

## Setting up meltano Projects
Use the following command:
- `meltano init <project-name>`
- `cd <project-name>`

Example:

- `meltano init meltano-ingestion`
- `cd meltano-ingestion`


## meltano Connection
- meltano is a pipleline that connect between different database resource.
- It can be different database, or between a file and a database. Irregardless, they are called data resource.
- A data resource can be a source or destination.
- In meltano, we named source as **extractor** and we use `tap` as the prefix.
- For destination, we named it as **loader** and we use `target` as the prefix.
- The rest is just setting up the configuration.
- meltano can be very difficult or very easy. It depends on your data source and its connectors.
- Some data source requires very complex settings, some are very easy such as duckDB.

## Creating Extractor (Data Source)
- We use the command `meltano add extractor tap-<SOURCE_NAME>` where the source name can be any data source. Please search the meltano website for all the available connectors.
- All data source are prefix `tap`. A datasource such as postgres can be source and destination.
- Listed are some possible data source: [`tap-github`, `tap-csv`, `tap-duckdb`, `tap-postgres`]
- Next we need to configure the data source using the command shown below:
```zsh
meltano config tap-<SOURCE_NAME> set --interactive
```
- An interactive menu will be shown and you can choose which option to set.
- Alternatively, we can also use command line as shown below
```zsh
meltano config tap-<SOURCE_NAME> set <SETTINGS> [value]
```
- In addition, meltano also allows data selection using the command `select`.
```zsh
meltano select <plugin> <entity> <attribute>
```
- The following example allows the extractor to select only some resource.
```zsh
meltano select tap-github releases tag_name
meltano select tap-github releases body
meltano select tap-github releases published_at
```
- Once the setup has been done, for some connectors it provide a testing tool. Most extractor got the testing tool. You need to search the web for the respective connector to confirm. The command of the test is usually in the format below:
```zsh
meltano config tap-<SOURCE_NAME> test
```

## Creating Loader (Data Destination)
- We use the command `meltano add loader target-<TARGET_NAME>` where the target name can be any data source. Please search the meltano website for all the available connectors.
- All data destination are prefix `target`. A datasource such as postgres can be source and destination.
- Listed are some possible data source: [`target-bigquery`, `target-postgres`, `target-jsonl`]
- Next we need to configure the data destination using the command shown below:
```zsh
meltano config target-<TARGET_NAME> set --interactive
```
- An interactive menu will be shown and you can choose which option to set.
- Alternatively, we can also use command line as shown below
```zsh
meltano config target-<TARGET_NAME> set <SETTINGS> [value]
```

## Running Meltano Pipeline
Once the data source and destination are set. We can run the pipeline as:
```zsh
meltano run tap-<SOURCE_NAME> target-<TARGET_NAME>
```
- For any errors please refer to the log file under the folder .meltano
- As mention earlier, most errors are configuration for each data connectors.

Some examples:
- `meltano run tap-github target-jsonl`
- `meltano run tap-github target-bigquery`
- `meltano run tap-postgres target-bigquery`
- `meltano run tap-duckdb target-postgres`
- `meltano run tap-duckdb target-bigquery`

## Common Extractor and Configuration 

### tap-github

Add extractor:
```zsh
meltano add extractor tap-github
```

Set configuration:
```zsh
meltano config tap-github set --interactive
```

Essential settings:
- auth_token=token provided by Github
- repositories=repository

Data Selection:
We can also use 
```zsh
meltano tap-github set --interactive
```

Show all configurations:
```zsh
meltano select tap-github <entity> <attribute>
#####
meltano select tap-github releases tag_name
meltano select tap-github releases body
meltano select tap-github releases published_at
```


Test configuration
```zsh
meltano config tap-github test
```

Reference:
- https://hub.meltano.com/extractors/tap-github/

### tap-duckdb
Add extractor:
```zsh
meltano add extractor tap-duckdb
```

Set configuration:
```zsh
meltano config tap-duckdb set --interactive
```

Essential settings:
- Option 3: path=/Users/USER/location_of_your_duckdb.mydb.db


Show all configurations:
```zsh
meltano config tap-duckdb list
```

Test configuration
```zsh
meltano config tap-duckdb test
```

Reference:
- https://hub.meltano.com/extractors/tap-duckdb

### tap-postgres
Add extractor:
```zsh
meltano add extractor tap-postgres
```

Set configuration:
```zsh
meltano config tap-postgres set --interactive
```

Essential settings:
- Option 5: database=postgres
- Option 10: filter_scheme=['public']
- Option 13: host=aws-0-us-west-1.pooler.supabase.com
- Option 16: password=reducted
- Option 17: port=5432
- Option 35: user=postgres.random_nodes



Show all configurations:
```zsh
meltano config tap-postgres list
```

Test configuration
```zsh
meltano config tap-postgres test
```

Reference:
- https://hub.meltano.com/extractors/tap-postgres



## Common Loader and Configuration

### target-jsonl
Add loader:
```zsh
meltano add loader target-jsonl
```
Note: There are many variants for jsonl

Set configuration:
```zsh
meltano config target-jsonl set --interactive
```

Essential settings:
- destination_path=/location_of_destination/  (default is /output)

Show all configurations:
```zsh
meltano config target-jsonl list
```

Reference:
- https://hub.meltano.com/loaders/target-jsonl


### target-bigquery
Add loader:
```zsh
meltano add loader target-bigquery
```
Note: There are many variants for jsonl

Set configuration:
```zsh
meltano config target-bigquery set --interactive
```

Essential settings:
- Option 10: credentials_path=/Users/USER/.dbt/my_service_key.json
- Option 11: dataset=my_dataset_name
- Option 13: denormalized=True
- Option 15: flattening_enabled=True
- Option 16: flattening_max_depth=1
- Option 19: method=batch_job
- Option 26: project=my_bigquery_project

Show all configurations:
```zsh
meltano config target-bigquery list
```

Reference:
- https://hub.meltano.com/loaders/target-bigquery



### Additional Resources:
- https://docs.meltano.com/getting-started/part1
- https://docs.meltano.com/guide/integration/#selecting-entities-and-attributes-for-extraction