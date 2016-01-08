# mmd-caffe

This is the implementation of PAMI paper "Learning Transferable Visual Features with Very Deep Adaptation Networks". We fork the repository with version ID `c6414ea` from [Caffe](https://github.com/BVLC/caffe) and make our modifications. The main modifications are listed as follow:

- Change the label from a single integer to a tuple, which contains the origin label and an indicator to distinguish source and target
- Add mmd layer described in the paper to neuron layers
- Add entropy loss layer described in the paper to loss layers

Training Model
---------------

In `models/google_net/amazon_to_webcam`, we give an example model based on GoogLeNet. In this model, we insert mmd layers  after inception 4d, 4e, 5a and the last fully connect layer individually. In addition, an entropy loss layer is linked after the last fully connect layer.

Data Preparation
---------------
In `data/office/amazon_to_webcam/*.txt`, we give an example to show how to prepare the train and test data file. In this task, `amazon` dataset is the source domain and `webcam` dataset is the target domain. For training data in source domain, it's label looks like `(-1, origin_label)`. For training data in target domain, it's label looks like `(origin_label, -1)`. For testing data, it's label looks like `(origin_label, -1)`. Integer `-1` is used to distinguish source and target during training, and `origin_label` is the acutal label.

Parameter Tuning
---------------
In `models/google_net/amazon_to_webcam/proto_parse.py`, we write a python script to help tune the network parameters. An inception is ragarded as a unit and it's parameters are tune together.

Dependency
---------------
We use [CGAL](http://www.cgal.org) to solve Quadratic Programming problem when updating `beta`. Please install it before compiling this code. We add CGAL as LIBRARIES in `Makefile`. However, if you prefer to compile caffe with `cmake`, maybe you should config CGAL yourself.
