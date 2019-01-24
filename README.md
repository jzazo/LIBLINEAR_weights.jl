# LIBLINEAR

[![Build Status](https://travis-ci.org/innerlee/LIBLINEAR.jl.svg?branch=master)](https://travis-ci.org/innerlee/LIBLINEAR.jl)
[![Build status](https://ci.appveyor.com/api/projects/status/x9jq6w5mji1u6eff?svg=true)](https://ci.appveyor.com/project/innerlee/liblinear-jl)

Julia bindings for [LIBLINEAR](https://www.csie.ntu.edu.tw/~cjlin/liblinear/).

```julia
using RDatasets, LIBLINEAR
using Printf, Statistics

# Load Fisher's classic iris data
iris = dataset("datasets", "iris")

# LIBLINEAR handles multi-class data automatically using a one-against-the rest strategy
labels = iris[:Species]

# First dimension of input data is features; second is instances
instances = convert(Matrix, iris[:, 1:4])'

# Train SVM on half of the data using default parameters. See the linear_train
# function in LIBLINEAR.jl for optional parameter settings.
model = linear_train(labels[1:2:end], instances[:, 1:2:end], verbose=true);

# Test model on the other half of the data.
(predicted_labels, decision_values) = linear_predict(model, instances[:, 2:2:end]);

# Compute accuracy
@printf "Accuracy: %.2f%%\n" mean((predicted_labels .== labels[2:2:end]))*100
```

---

## LIBLINEAR-weights

This library also implements an instance that admits weights in the loss function per data sample.
It calls upon modified liblinear [sources](https://www.csie.ntu.edu.tw/~cjlin/libsvmtools/#weights_for_data_instances).

To use this branch, change to the #weights branch:
```
(v1.0)> add LIBLINEAR#weights
```

Example of usage from before:
```julia
W = ones(length(labels))
model = linear_train(labels[1:2:end], instances[:, 1:2:end], W=W, verbose=true);
```
Note that `linear_predict` with weights is not supported by LIBLINEAR sources.



## Credits

Created by Zhizhong Li; #weight branch created by Javier Zazo.

This package is adapted from the [LIBSVM](https://github.com/simonster/LIBSVM.jl) Julia package by Simon Kornblith.
