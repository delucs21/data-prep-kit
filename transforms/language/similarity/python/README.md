# Similarity Transform 
Please see the set of
[transform project conventions](../../../README.md#transform-project-conventions)
for details on general project conventions, transform configuration,
testing and IDE set up.

## Contributors
- Chad DeLuca (delucac@us.ibm.com)
- Anna Lisa Gentile (annalisa.gentile@ibm.com)

## Summary 
The similarity transforms annotates each input document with potential matches found in a document collection.
The annotation consists of a json object proving the id of the matched document in the collection and 
the specific sentenced deemed as "similar" by the tranform.
The Similarity Transorm relies on a running [ElasticSearch](https://www.elastic.co/elasticsearch) Index.

### Input files

This transform supports the input of [parquet files] (https://parquet.apache.org/) that contain a single column, called "contents",
where each row is a a string that will be searched for in a target document collection.
The document collection is specified with configuration parameters.


### Output format

The output table will contain a single additional column:

| output column name | data type | description |
|-|-|-|
| contents | string | the original input text |
| similarity_score | json | the annotations that describe in which document a potential match was found and which sentence in the document was the closest match  |

Example of single cell contents in the output column:
```
I bet the company staffs want an increase in the wages
```

Example of single cell content in the similarity_score column:

```py
  {
      'contents': array(['I bet the company staffs want to have an increase in the wages.'], dtype=object), 
      'id': '123456789', 
      'index': 'myPrivateDocumentsIndex', 
      'score': 29.345
  }
```

## Configuration

The transform can be initialized with the following parameters.

| Parameter  | Default  | Description  |
|------------|----------|--------------|
| `similarity_es_endpoint` | - | The URL for Elasticsearch |
| `similarity_es_userid` | - | Elasticsearch user ID |
| `similarity_es_pwd` | - | Elasticsearch password |
| `similarity_es_index` | - | The Elasticsearch index to query |
| `similarity_shingle_size` | 8 | Shingle size for query construction (default is 8) |
| `similarity_result_size` | 1 | result size for matched sentences (default is 1) |
| `similarity_annotation_column` | similarity_score | The column name that will contain the similarity annotations, in json format |
| `data_s3_cred` | - | AST string of options for s3 credentials. Only required for S3 data access. <br>access_key: access key help text <br>secret_key: secret key help text <br>url: optional s3 url <br>region: optional s3 region <br>Example: { 'access_key': 'access', 'secret_key': 'secret', <br>'url': 'https://s3.us-east.cloud-object-storage.appdomain.cloud', <br>'region': 'us-east-1' }|
| `data_s3_config` | - | AST string containing input/output paths. <br> input_folder: Path to input folder of files to be processed <br> output_folder: Path to output folder of processed files <br>Example: { 'input_folder': 's3-path/your-input-bucket', 'output_folder': 's3-path/your-output-bucket' } | 
| `data_local_config` | - | AST string containing input/output folders using local fs. <br>input_folder: Path to input folder of files to be processed <br>output_folder: Path to output folder of processed files <br>Example: { 'input_folder': './input', 'output_folder': '/tmp/output' }|
| `data_max_files` | - | Max amount of files to process |
| `data_checkpointing` | - | checkpointing flag |
| `data_files_to_checkpoint` | - | list of file extensions to choose for checkpointing. |
| `data_data_sets` | - | List of sub-directories of input directory to use for input. For example, ['dir1', 'dir2'] |
| `data_files_to_use` | - | list of file extensions to choose for input. |
| `data_num_samples` | - | number of random input files to process |
| `runtime_num_processors` | - | size of multiprocessing pool |
| `runtime_pipeline_id` | - | pipeline id |
| `runtime_job_id` | - | job id |


Example

```py
{
      "similarity_es_pwd" :"my password",
      "similarity_es_userid", "myElasticsearchID",
      "similarity_es_endpoint":"https://thisIsWhere.MyElasticIsRunning.com",
      "similarity_es_index", "myPrivateDocumentsIndex"
}
```

## Running

### Launched Command Line Options 
The following command line arguments are available in addition to 
the options provided by 
the [python launcher](../../../../data-processing-lib/doc/python-launcher-options.md).
```
  --similarity_es_endpoint SIMILARITY_ES_ENDPOINT
                        The URL for Elasticsearch
  --similarity_es_userid SIMILARITY_ES_USERID
                        Elasticsearch user ID
  --similarity_es_pwd SIMILARITY_ES_PWD
                        Elasticsearch password
  --similarity_es_index SIMILARITY_ES_INDEX
                        The Elasticsearch index to query
  --similarity_shingle_size SIMILARITY_SHINGLE_SIZE
                        Shingle size for query construction (default is 8)
  --similarity_result_size SIMILARITY_RESULT_SIZE
                        result size for matched sentences (default is 1)
  --similarity_annotation_column SIMILARITY_ANNOTATION_COLUMN
                        The column name that will contain the similarity score
  --similarity_doc_text_column SIMILARITY_DOC_TEXT_COLUMN
                        The column name that contains the document text
  --data_s3_cred DATA_S3_CRED
                        AST string of options for s3 credentials. Only required for S3 data access.
                        access_key: access key help text
                        secret_key: secret key help text
                        url: optional s3 url
                        region: optional s3 region
                        Example: { 'access_key': 'access', 'secret_key': 'secret',
                        'url': 'https://s3.us-east.cloud-object-storage.appdomain.cloud',
                        'region': 'us-east-1' }
  --data_s3_config DATA_S3_CONFIG
                        AST string containing input/output paths.
                        input_folder: Path to input folder of files to be processed
                        output_folder: Path to output folder of processed files
                        Example: { 'input_folder': 's3-path/your-input-bucket',
                        'output_folder': 's3-path/your-output-bucket' }
  --data_local_config DATA_LOCAL_CONFIG
                        ast string containing input/output folders using local fs.
                        input_folder: Path to input folder of files to be processed
                        output_folder: Path to output folder of processed files
                        Example: { 'input_folder': './input', 'output_folder': '/tmp/output' }
  --data_max_files DATA_MAX_FILES
                        Max amount of files to process
  --data_checkpointing DATA_CHECKPOINTING
                        checkpointing flag
  --data_files_to_checkpoint DATA_FILES_TO_CHECKPOINT
                        list of file extensions to choose for checkpointing.
  --data_data_sets DATA_DATA_SETS
                        List of sub-directories of input directory to use for input. For example, ['dir1', 'dir2']
  --data_files_to_use DATA_FILES_TO_USE
                        list of file extensions to choose for input.
  --data_num_samples DATA_NUM_SAMPLES
                        number of random input files to process
  --runtime_num_processors RUNTIME_NUM_PROCESSORS
                        size of multiprocessing pool
  --runtime_pipeline_id RUNTIME_PIPELINE_ID
                        pipeline id
  --runtime_job_id RUNTIME_JOB_ID
                        job id
  --runtime_code_location RUNTIME_CODE_LOCATION
                        AST string containing code location
                        github: Github repository URL.
                        commit_hash: github commit hash
                        path: Path within the repository
                        Example: { 'github': 'https://github.com/somerepo', 'commit_hash': '1324',
                        'path': 'transforms/universal/code' }
```
These correspond to the configuration keys described above.


### Launched Command Line Options 

When invoking the CLI, the parameters must be set as `--similarity_<name>`, e.g. `--similarity_es_pwd=pass`.

### Running the samples
To run the samples, use the following `make` targets

* `run-cli-sample` - runs src/similarity_transform_python.py using command line args
* `run-local-sample` - runs src/similarity_local.py
* `run-local-python-sample` - runs src/similarity_local_python.py

These targets will activate the virtual environment and set up any configuration needed.
Use the `-n` option of `make` to see the detail of what is done to run the sample.

For example, 
```shell
make run-local-python-sample
...
```
Then 
```shell
ls output
```
To see results of the transform.

### Code example

TBD (link to the notebook will be provided)

