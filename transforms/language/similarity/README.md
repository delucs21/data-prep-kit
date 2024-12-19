# Similarity Transform 
The similarity transforms annotates each input document with potential matches found in a document collection.
The annotation consists of a json object proving the id of the matched document in the collection and 
the specific sentenced deemed as "similar" by the tranform.
These can be later used in subsequent operations, per the set of 
[transform project conventions](../../README.md#transform-project-conventions)
the following runtimes are available:

* [python](python/README.md) - enables the running of the base python transformation
  in a Python runtime


## Summary
This transform annotates documents with similar sentences found in a target document collection.
