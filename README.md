# NNEF-Tools

This repository contains tools to generate and consume NNEF documents, such as a parser that can be included in consumer applications and converters for deep learning frameworks.

Currently, there are two sets of converters; one provided by Khronos Group (in the `converter` folder) and one provided by the company Au-Zone (in the `au-zone` folder).

The ones provided by the Khronos Group convert between Caffe and TensorFlow. The Caffe converter reads/writes models in protobuf format, while the TensorFlow converter reads/writes the Python script that builds the TensorFlow graph

The ones provided by Au-Zone convert between Caffe2 and TensorFlow, both reading/writing models in protobuf format.

**We are working on merging the above converters into a unified tool that can handle all supported formats under the same interface, and publishing it as a Python package.**


# IMPORTANT BUG FIX

We have recently found a bug in the Python code that reads/writes the tensor binary files (the header contained 4 extra padding bytes therefore not conforming to the spec). We have updated the code to read/write and _check_ the proper header size. As a consequence, any files written out with the code that contained the bug cannot be read back with the updated code. To aid the usage of such existing files, we have created a script called `fix_nnef_binary_size.py` that can be used to remove the excess 4 bytes from existing NNEF files. The script is located in the root folder of this repo, it has no dependencies (not even the NNEF parser). It can be run on the main folder of an NNEF model, and it fixes all binary files in the folder. In case one runs it on an NNEF model that does not contain the bug, it does nothing. It can be used as follows:

```
python fix_nnef_binary_size.py my_nnef_model_folder
```

Such an invocation fixes the files in place. Optionally, a second argument can be supplied to the script to write the fixed files to a different output path. In this case, the script copies all non-binary files (such as graph.nnef) to the target folder, so the resulting folder contains the whole valid model.
