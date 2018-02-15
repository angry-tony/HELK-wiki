# Design
[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/SPARK-Design.png]]

# Spark Packages
## elasticsearch-hadoop-6.2.0
"Elasticsearch for Apache Hadoop is an open-source, stand-alone, self-contained, small library that allows Hadoop jobs (whether using Map/Reduce or libraries built upon it such as Hive, Pig or Cascading or new upcoming libraries like Apache Spark ) to interact with Elasticsearch. One can think of it as a connector that allows data to flow bi-directionaly so that applications can leverage transparently the Elasticsearch engine capabilities to significantly enrich their capabilities and increase the performance. 
Elasticsearch-hadoop provides native integration between Elasticsearch and Apache Spark, in the form of an RDD (Resilient Distributed Dataset) (or Pair RDD to be precise) that can read data from Elasticsearch. The RDD is offered in two flavors: one for Scala (which returns the data as Tuple2 with Scala collections) and one for Java (which returns the data as Tuple2 containing java.util collections). Just like other libraries, elasticsearch-hadoop needs to be available in Spark’s classpath. As Spark has multiple deployment modes, this can translate to the target classpath, whether it is on only one node (as is the case with the local mode - which will be used through-out the documentation) or per-node depending on the desired infrastructure." [Elastic](https://www.elastic.co/guide/en/elasticsearch/hadoop/current/spark.html)

## graphframes:graphframes:0.5.0-spark2.1-s_2.11
"This is a prototype package for DataFrame-based graphs in Spark. Users can write highly expressive queries by leveraging the DataFrame API, combined with a new API for motif finding. The user also benefits from DataFrame performance optimizations within the Spark SQL engine." [SparkPackages](https://spark-packages.org/package/graphframes/graphframes)
"It aims to provide both the functionality of GraphX and extended functionality taking advantage of Spark DataFrames. This extended functionality includes motif finding, DataFrame-based serialization, and highly expressive graph queries." [Graphframes](https://graphframes.github.io/)

## org.apache.spark:spark-sql-kafka-0-10_2.11:2.2.1
"Structured Streaming integration for Kafka 0.10 to poll data from Kafka" [Structured Streaming Kafka](https://spark.apache.org/docs/2.0.2/structured-streaming-kafka-integration.html)

## databricks:spark-sklearn:0.2.3
"This package contains some tools to integrate the Spark computing framework with the popular scikit-learn machine library. Among other tools: 1) train and evaluate multiple scikit-learn models in parallel. It is a distributed analog to the multicore implementation included by default in scikit-learn. 2) convert Spark's Dataframes seamlessly into numpy ndarrays or sparse matrices. 3) (experimental) distribute Scipy's sparse matrices as a dataset of sparse vectors." [SparkPackages](https://spark-packages.org/package/databricks/spark-sklearn)

# Jupyter Integration
"The Jupyter Notebook is an open-source web application that allows you to create and share documents that contain live code, equations, visualizations and narrative text. Uses include: data cleaning and transformation, numerical simulation, statistical modeling, data visualization, machine learning, and much more."Jupyter Reference." [Jupyter](http://jupyter.org/)
HELK integrates the Jupyter Notebook project with Spark via the **PYSPARK_DRIVER_PYTHON**. Basically, when the HELK runs **/bin/pyspark**, Jupyter notebook is executed as PYSPARK's Python Driver. The **PYSPARK_DRIVER_PYTHON_OPTS** value is the following:
```
"notebook --NotebookApp.open_browser=False --NotebookApp.ip='*' --NotebookApp.port=8880 --allow-root"
```
# Test Spark, GraphFrames & Jupyter Integration
By default, the Jupyter server gets started automatically after installing the HELK.
* Access the Jupyter Server: 
	* Go to your <HELK's IP>:8880 in your preferred browser
	* Enter the token provided after installing the HELK
* Go to the training/jupyter_notebooks/getting_started/ folder
* Open the Check_Spark_Graphframes_Integrations notebook
	* Check the saved output (Make sure that you have Sysmon & Windows Security event logs being sent to your HELK. Otherwise you will get errors in your Jupyter Notebook when trying to replicate the basic commands)
	* Clear the output from the notebook and run everything again

[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/HELK_checking_integrations.png]]

# Other Python Packages
## OTXv2
"OTX Direct Connect provides a mechanism to automatically pull indicators of compromise from the Open Threat Exchange portal into your environment. The DirectConnect API provides access to all Pulses that you have subscribed to in Open Threat Exchange" [AlienVault](https://otx.alienvault.com). The HELK uses this package along with pandas and the [helk_otx.py](script https://github.com/Cyb3rWard0g/HELK/blob/master/scripts/helk_otx.py) to pull IOCs to then be exported as a CSV file. That CSV file is then used in Logstash filters by its [Translate](https://www.elastic.co/guide/en/logstash/current/plugins-filters-translate.html) plugin.

## Pandas (0.22.0)
"Pandas is an open source, BSD-licensed library providing high-performance, easy-to-use data structures and data analysis tools for the Python programming language." [Pandas Pydata](https://pandas.pydata.org/pandas-docs/stable/overview.html)

## Scipy (1.0.0)
"It is a Python-based ecosystem of open-source software for mathematics, science, and engineering." [Scipy Org.](https://www.scipy.org/)

## Scikit-learn (0.19.1)
"Simple and efficient tools for data mining and data analysis. Built on NumPy, SciPy, and matplotlib." [Scikit-Learn Org.](http://scikit-learn.org/stable/index.html)

## Nltk (3.2.5)
"NLTK is a leading platform for building Python programs to work with human language data. It provides easy-to-use interfaces to over 50 corpora and lexical resources such as WordNet, along with a suite of text processing libraries for classification, tokenization, stemming, tagging, parsing, and semantic reasoning, wrappers for industrial-strength NLP libraries, and an active discussion forum." [Ntlk Org.](http://www.nltk.org/)

## Matplotlib (2.1.2)
"Matplotlib is a Python 2D plotting library which produces publication quality figures in a variety of hardcopy formats and interactive environments across platforms. Matplotlib can be used in Python scripts, the Python and IPython shell, the jupyter notebook, web application servers, and four graphical user interface toolkits." [Matplotlib](https://matplotlib.org/index.html)

## Seaborn (0.8.1)
"Seaborn is a Python visualization library based on matplotlib. It provides a high-level interface for drawing attractive statistical graphics." [Seaborn Pydata](https://seaborn.pydata.org/index.html)

## Datasketch (1.2.5)
"Datasketch gives you probabilistic data structures that can process and search very large amount of data super fast, with little loss of accuracy." [Datasketch Github](https://github.com/ekzhu/datasketch)

## Tensorflow (1.5.0)
"TensorFlow is a programming system in which you represent computations as graphs. Nodes in the graph are called ops (short for operations). An op takes zero or more Tensors, performs some computation, and produces zero or more Tensors. In TensorFlow terminology, a Tensor is a typed multi-dimensional array. For example, you can represent a mini-batch of images as a 4-D array of floating point numbers with dimensions [batch, height, width, channels].

A TensorFlow graph is a description of computations. To compute anything, a graph must be launched in a Session. A Session places the graph ops onto Devices, such as CPUs or GPUs, and provides methods to execute them. These methods return tensors produced by ops as numpy ndarray objects in Python, and as tensorflow::Tensor instances in C and C++." [Tensorflow](https://www.tensorflow.org/versions/r0.12/get_started/basic_usage)

## Keras (2.1.3)
"Keras is a high-level neural networks API, written in Python and capable of running on top of TensorFlow, CNTK, or Theano. It was developed with a focus on enabling fast experimentation. Being able to go from idea to result with the least possible delay is key to doing good research." [Keras](https://keras.io/)

## Pyflux (0.4.15)
"PyFlux is an open source time series library for Python. The library has a good array of modern time series models, as well as a flexible array of inference options (frequentist and Bayesian) that can be applied to these models. By combining breadth of models with breadth of inference, PyFlux allows for a probabilistic approach to time series modelling." [Pyflux Github](https://github.com/RJT1990/pyflux)

## Imbalanced-learn (0.3.2)
"imbalanced-learn is a python package offering a number of re-sampling techniques commonly used in datasets showing strong between-class imbalance. It is compatible with scikit-learn and is part of scikit-learn-contrib projects." [Imbalanced Learn](https://github.com/scikit-learn-contrib/imbalanced-learn)

## Lime (0.1.1.29)
"This project is about explaining what machine learning classifiers (or models) are doing. Lime is able to explain any black box classifier, with two or more classes. All we require is that the classifier implements a function that takes in raw text or a numpy array and outputs a probability for each class. Support for scikit-learn classifiers is built-in." [Lime](https://github.com/marcotcr/lime)