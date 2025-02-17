# Text Encoder Transform 

Please see the set of
[transform project conventions](../../README.md#transform-project-conventions)
for details on general project conventions, transform configuration,
testing and IDE set up.

## Contributors

- Michele Dolfi (dol@zurich.ibm.com)

## Description 

This transform is using [sentence encoder models](https://en.wikipedia.org/wiki/Sentence_embedding) to create embedding vectors of the text in each row of the input .parquet table.

The embeddings vectors generated by the transform are useful for tasks like sentence similarity, features extraction, etc which are also at the core of retrieval-augmented generation (RAG) applications.

### Input 

| input column name | data type | description |
|-|-|-|
| the one specified in _content_column_name_ configuration | string | the content used in this transform |


### Output columns


| output column name | data type | description |
|-|-|-|
| the one specified in _output_embeddings_column_name_ configuration | `array[float]` | the embeddings vectors of the content |


## Configuration

The transform can be tuned with the following parameters.


| Parameter  | Default  | Description  |
|------------|----------|--------------|
| `model_name`                    | `BAAI/bge-small-en-v1.5` | The HF model to use for encoding the text. |
| `content_column_name`           | `contents` | Name of the column containing the text to be encoded. |
| `output_embeddings_column_name` | `embeddings` | Column name to store the embeddings in the output table. |


## Usage

### Launched Command Line Options 

When invoking the CLI, the parameters must be set as `--text_encoder_<name>`, e.g. `--text_encoder_column_name_key=myoutput`.

### Code example

Here is a sample [notebook](text_encoder-python.ipynb)

### Transforming data using the transform image

To use the transform image to transform your data, please refer to the 
[running images quickstart](../../../doc/quick-start/run-transform-image.md),
substituting the name of this transform image and runtime as appropriate.

## Testing

Following [the testing strategy of data-processing-lib](../../../data-processing-lib/doc/transform-testing.md)

Currently we have:
- [Unit test](test/test_text_encoder_python.py)



# TextEncoder Ray Transform 
Please see the set of
[transform project conventions](../../README.md#transform-project-conventions)
for details on general project conventions, transform configuration,
testing and IDE set up.

## Summary 
This project wraps the text_encoder transform with a Ray runtime.

## Configuration and command line Options

Text Encoder configuration and command line options are the same as for the base python transform. 

### Code example

Here is a sample [notebook](text_encoder-ray.ipynb)

### Launched Command Line Options 

In addition to those available to the transform as defined here,
[ray launcher options](../../../data-processing-lib/doc/ray-launcher-options.md) are available.

### Transforming data using the transform image

To use the transform image to transform your data, please refer to the 
[running images quickstart](../../../doc/quick-start/run-transform-image.md),
substituting the name of this transform image and runtime as appropriate.
