# Java

### Compile java sources

```bash
javac JavaClass.java
```

### Run java class

```bash
java JavaClass
java -cp /path/to/java/classes/* JavaClass
```

### Create a jar file

```bash
jar cf java_class-v1.0.jar JavaClass.class
jar cf java_class-v1.0.jar *.class
```

### Viewing the contents of a jar file

```bash
jar tf java_class-v1.0.jar
```

### CLASSPATH example

```bash
export CLASSPATH=.:$HIVE_HOME/lib/*:$HIVE_HOME/jdbc/*:$HADOOP_HOME/lib/*:$HOME/local/*
```
