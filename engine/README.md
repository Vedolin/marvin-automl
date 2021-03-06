[![Build Status](https://travis-ci.org/marvin-ai/marvin-automl-engine.svg)](https://travis-ci.org/marvin-ai/marvin-automl-engine) [![codecov](https://codecov.io/gh/marvin-ai/marvin-automl-engine/branch/master/graph/badge.svg)](https://codecov.io/gh/marvin-ai/marvin-automl-engine)

[![Join the chat at https://gitter.im/gitterHQ/gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/marvin-ai)

# Marvin AutoML Engine v0.0.1

## Overview

AutoML Engine


## Requirements

 - Python 2.7
 - scikit-learn 0.18.2
 - scipy 0.19.1
 - numpy 1.13.1
 - pandas 0.20.3
 - matplotlib 2.0.2
 - marvin-python-toolbox 0
 - Fabric 1.14.0
 - tpot 0.9.3
- datacleaner 0.1.5

## Run or Install

There are a Docker Image prepared with this engine. To run the docker container:

```
sudo docker pull marvinaiplatform/marvin-automl:0.0.1
```
```
sudo docker run --name=marvin-automl-0.0.1 --mount type=bind,source=$HOME/marvin/data,destination=/marvin-data -p 8000:8000 marvinaiplatform/marvin-automl:0.0.1
```
Access `http://localhost:8000/docs/` to use the API HTTP interface.

To install the engine locally (without docker) follow these steps:

1. Install Python Toolbox, [described here](https://www.marvin-ai.org/book/installing-marvin/ubuntu-user-installation)
2. Clone the engine from github
3. Install the engine, [described here](https://www.marvin-ai.org/book/get-started/working-in-an-existent-engine)


### Training a new model

To train a new model, use the `/pipeline` on API HTTP interface with the following 2 parameters options:

- For regression
```
{"params": {"url": "https://raw.githubusercontent.com/selva86/datasets/master/BostonHousing.csv",
  "separator": ",",
  "encoding": "utf-8",
  "generations": 1,
  "population_size": 50,
  "config": "TPOT sparse",
  "target": "medv",
  "problem_type": "regression"},
 "message": [
   [0.00632, 18, 2.31, "0", 0.538, 6.575, 65.2, 4.09, 1, 296, 15.3, 396.9, 4.98]
 ]}
 ```
 
 - For classification
 ```
{"params": {"url": "https://gist.githubusercontent.com/curran/a08a1080b88344b0c8a7/raw/d546eaee765268bf2f487608c537c05e22e4b221/iris.csv",
 "separator": ",",
 "encoding": "utf-8",
 "generations": 1,
 "population_size": 50,
 "config": "TPOT sparse",
 "target": "species",
 "problem_type": "classification"},
"message": [[5.1, 3.5, 1.4, 0.2]]}

```

After running pipeline, **restart the docker container** to reload the model artifact. This behavior will be fixed shortly.

### Predicting

To predict a new value, use the `/predictor` endpoint on API HTTP interface with the same 2 params above.

The `params` key is used to "configure" the automl code. The `message` key is used to input the values/features to predict.


### Results

These are preliminary comparative data for the purpose of monitoring the advancement of the AutoML solution on the Marvin platform.

The selected datasets have less than 1000 records (rows) and less than 100 features (columns). This selection was made to simplify and streamline the training and optimization process of the models.

The F Measure was used because some datasets has imbalance classes and accurace measure will not perform well in this situation.


| OpenML ID| dataset           | flow_name                                                      | measure   |    value |   automl_value |        delta |
|---------:|:------------------|:---------------------------------------------------------------|:----------|---------:|---------------:|-------------:|
|        2 | anneal            | weka.LogitBoost_DecisionStump(3)                               | f_measure | 0.997506 |       0.95     |  -0.047506   |
|       50 | tic-tac-toe       | weka.SMO_PolyKernel(1)                                         | f_measure | 1        |     nan        | nan          |
|       54 | vehicle           | sklearn.pipeline.Pipeline(..., clf=sklearn.svm.classes.SVC)(1) | f_measure | 0.869092 |       0.829412 |  -0.0396804  |
|      311 | oil_spill         | classif.lda(11)                                                | f_measure | 0.966721 |       0.714286 |  -0.252435   |
|      839 | kdd_el_nino-small | weka.RotationForest_PrincipalComponents_J48(14)                | f_measure | 0.962931 |       0.980392 |   0.0174612  |
|      841 | stock             | weka.Decorate(1)                                               | f_measure | 0.973682 |       0.962963 |  -0.010719   |
|     1016 | vowel             | classif.IBk(5)                                                 | f_measure | 1        |       1        |   0          |
|    40693 | xd6               | weka.kf.RandomForest(1)                                        | f_measure | 1        |       1        |   0          |
|    40705 | tokyo1            | weka.kf.RandomForest(1)                                        | f_measure | 0.934157 |       0.936508 |   0.00235094 |
