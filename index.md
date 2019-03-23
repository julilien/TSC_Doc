# Introduction

The JAICore TSC framework provides fast Java implementations of popular time series classification models. The implemented classifiers were compared to several Java reference implementations to ensure appropriate performance.

The frameworks has been developed within a subproject of the [On-The-Fly Machine Learning project group](https://cs.uni-paderborn.de/de/is/teaching/courses/ss-2018/project-group/) at Paderborn University.

# How to use

## Import a dataset

In order to import a time series dataset as an object of `jaicore.ml.tsc.dataset.TimeSeriesDataset` from a given `.arff` file using `jaicore.ml.tsc.util.SimplifiedTimeSeriesLoader`, you have to do the following:

```java
Pair<TimeSeriesDataset, ClassMapper> trainPair = SimplifiedTimeSeriesLoader.loadArff(trainingArffFilePath);
Pair<TimeSeriesDataset, ClassMapper> testPair = SimplifiedTimeSeriesLoader.loadArff(testArffFilePath);

TimeSeriesDataset trainingDS = trainPair.getX();
TimeSeriesDataset testDS = testPair.getY();
```

The `ClassMapper` object can be used to map `String` classes to `Integer` values which are commonly used within the implemented classifiers.

## Train and evaluate a classifier

To train a classifier, you have to create an object of the desired subclass of `ASimplifiedTSClassifier`. This class provides the methods `train` and `predict`, which are implemented by the dervied subclasses. The training itself then looks as follows (here: using `LearnShapelets`:

```java
LearnShapeletsClassifier ls = new LearnShapeletsClassifier(K, learningRate, regularization, scaleR,
				minShapeLength, maxIter, seed);
ls.train(trainingDS);
```

To predict values given the `testDS` from above, the following code has to be executed:

```java
List<Integer> predictions = ls.predict(testDS);
```

These predictions can then be evaluated to determine the performance quality, e.g. by accuracy based on the targets stored in `testDS`.
