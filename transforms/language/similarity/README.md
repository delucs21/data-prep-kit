# Similarity Transform 
The similarity transforms annotates each input document with potential matches found in a document collection.
The annotation consists of a json object proving the id of the matched document in the collection and 
the specific sentenced deemed as "similar" by the tranform.
These can be later used in subsequent operations, per the set of 
[transform project conventions](../../README.md#transform-project-conventions)
the following runtimes are available:

* [pythom](python/README.md) - enables the running of the base python transformation
  in a Python runtime
* [ray](ray/README.md) - enables the running of the base python transformation
  in a Ray runtime
* [spark](spark/README.md) - enables the running of a spark-based transformation
in a Spark runtime. 
* [kfp](kfp_ray/README.md) - enables running the ray docker image 
in a kubernetes cluster using a generated `yaml` file.


## Summary
This transform annotates documents with similar sentences found in a target document collection.
