Apache Spark installation + ipython/jupyter notebook integration guide for macOS
================================================================================

Tested with Apache Spark 2.1.0, Python 2.7.13 and Java 1.8.0_112

For older versions of Spark and ipython, please, see also [previous version of text](https://gist.github.com/ololobus/4c221a0891775eaa86b0/956c90bceef6424ef74cc68c4b8b1acd688e1c82).


Install Java Development Kit
----------------------------
Download and install it from [oracle.com](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

Add following code to your e.g. `.bash_profile`
```bash
# For Apache Spark
if which java > /dev/null; then export JAVA_HOME=$(/usr/libexec/java_home); fi
```

Install Apache Spark
--------------------
You can use Mac OS package manager Brew ([http://brew.sh/](http://brew.sh/))
```shell
brew update
brew install scala
brew install apache-spark
```

Set up env variables
--------------------
Add following code to your e.g. `.bash_profile`
```bash
# For a ipython notebook and pyspark integration
if which pyspark > /dev/null; then
  export SPARK_HOME="/usr/local/Cellar/apache-spark/2.1.0/libexec/"
  export PYTHONPATH=$SPARK_HOME/python:$SPARK_HOME/python/build:$PYTHONPATH
  export PYTHONPATH=$SPARK_HOME/python/lib/py4j-0.10.4-src.zip:$PYTHONPATH
fi
```

You can check `SPARK_HOME` path using following brew command
```
$ brew info apache-spark
apache-spark: stable 2.1.0, HEAD
Engine for large-scale data processing
https://spark.apache.org/
/usr/local/Cellar/apache-spark/2.1.0 (1,312 files, 213.9M) *
  Built from source on 2017-02-13 at 00:58:12
From: https://github.com/Homebrew/homebrew-core/blob/master/Formula/apache-spark.rb
```


Ipython profile
----------------------

Since [profiles are not supported](http://jupyter.readthedocs.io/en/latest/migrating.html#since-jupyter-does-not-have-profiles-how-do-i-customize-it) in `jupyter` and now you can see following deprecation warning
```shell
$ ipython notebook --profile=pyspark
[TerminalIPythonApp] WARNING | Subcommand `ipython notebook` is deprecated and will be removed in future versions.
[TerminalIPythonApp] WARNING | You likely want to use `jupyter notebook` in the future
[W 01:45:07.821 NotebookApp] Unrecognized alias: '--profile=pyspark', it will probably have no effect.
```
It seems that it is not possible to run various custom startup files as it was with `ipython` profiles. Thus, the easiest way will be to run `pyspark` init script at the beginning of your notebook manually or follow [alternative way](#alternatively).

Run ipython
-----------
```
$ jupyter-notebook
```

Initialize `pyspark`
```ipython
In [1]: import os
        execfile(os.path.join(os.environ["SPARK_HOME"], 'python/pyspark/shell.py'))
Out[1]: <pyspark.context.SparkContext at 0x10a982b10>
```

`sc` variable should be available
```ipython
In [2]: sc
Out[2]: <pyspark.context.SparkContext at 0x10a982b10>
```

Alternatively
-------------

You can also force `pyspark` shell command to run ipython web notebook instead of command line interactive interpreter. To do so you have to add following env variables:
```shell
export PYSPARK_DRIVER_PYTHON=jupyter
export PYSPARK_DRIVER_PYTHON_OPTS=notebook
```
and then simply run
```shell
$ pyspark
```
which will open a web notebook with `sc` available automatically.
