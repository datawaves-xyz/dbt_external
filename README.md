# dbt_external

This project provides some external tables in the blockchain data warehouse. In blockchain data analysis not only on-chain data is needed, but also off-chain data in many cases, such as NFT metadata and token prices.

The best means of providing this data is the external tables, as there are generally large and creating internal tables via `dbt seed` would cause performance issues. In order to achieve full automation, i.e. the user only needs to manually add new rows to the whitelist and the github action automatically fetches the external data and uploads it to the data warehouse, refreshing the table meta information in the data warehouse. This project uses the tool in [blockchain-dbt](https://github.com/datawaves-xyz/blockchain-dbt) for fetching incremental data.

If you need to use this repository for your own dbt project, you will need to do some initialization work:

- fork this project

- configuring the github action secret

  - SPARK_DATABASE: The database you want to connect to
  - SPARK_STS_HOST: The hostname of the Spark thrift server
  - SPARK_STS_PORT: The port of the Spark thrift server
  - AWS_ACCESS_KEY_ID: Your AWS Access Key
  - AWS_SECRET_ACCESS_KEY: Your AWS Secret Access Key
  - AWS_REGION: The region where you created your bucket
  - AWS_S3_BUCKET: The name of the bucket you're syncing to
  - AWS_S3_PREFIX: The directory inside of the S3 bucket you wish to sync/upload to

- get the full amount of data and initialize the table
  - `$ pip install blockchain-dbt`
  - `$ bdbt export_all_nft_metadata ...`
  - `$ dbt deps`
  - `$ dbt --debug run-operation stage_external_sources`

## Limitations

- The repository is currently serving [Datawaves](https://github.com/datawaves-xyz), so for now it only supports the Spark compute engine and S3 storage. More data warehouses and storage will be supported over time according to Datawaves's roadmap.
- [blockchain-dbt](https://github.com/datawaves-xyz/blockchain-dbt) only provides access to NFT metadata and only supports the use of the [Moralis API](https://moralis.io/), so feel free to submit an [issue](https://github.com/datawaves-xyz/blockchain-dbt/issues) to blockchain-dbt if you need to support more external data.
