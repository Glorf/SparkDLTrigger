FROM gitlab-registry.cern.ch/mbien/spark-artifactory/spark-base-image/spark-py:1.0

RUN apk update && apk del libc6-compat
RUN wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub && wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.29-r0/glibc-2.29-r0.apk && apk add glibc-2.29-r0.apk

RUN apk add --no-cache gcc gfortran musl-dev g++ python3-dev openblas-dev && python3 -m pip install --no-cache-dir --upgrade pip && python3 -m pip install --no-cache-dir cython && python3 -m pip install --no-cache-dir numpy scikit-learn

RUN wget -P /opt/spark/jars/ https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/3.2.0/hadoop-aws-3.2.0.jar https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-bundle/1.11.375/aws-java-sdk-bundle-1.11.375.jar https://repo1.maven.org/maven2/org/diana-hep/root4j/0.1.6/root4j-0.1.6.jar https://repo1.maven.org/maven2/org/apache/bcel/bcel/5.2/bcel-5.2.jar https://repo1.maven.org/maven2/jakarta-regexp/jakarta-regexp/1.4/jakarta-regexp-1.4.jar https://repo1.maven.org/maven2/org/tukaani/xz/1.2/xz-1.2.jar https://raw.githubusercontent.com/diana-hep/spark-root/master/jars/spark-root_2.11-0.1.17.jar http://central.maven.org/maven2/com/intel/analytics/zoo/analytics-zoo-bigdl_0.8.0-spark_2.4.3/0.5.1/analytics-zoo-bigdl_0.8.0-spark_2.4.3-0.5.1-jar-with-dependencies.jar

RUN wget -P /opt/spark/python/lib http://central.maven.org/maven2/com/intel/analytics/zoo/analytics-zoo-bigdl_0.8.0-spark_2.4.3/0.5.1/analytics-zoo-bigdl_0.8.0-spark_2.4.3-0.5.1-python-api.zip

ENV PYTHONPATH /opt/spark/python/lib/pyspark.zip:/opt/spark/python/lib/py4j-0.10.7-src.zip:/opt/spark/python/lib/analytics-zoo-bigdl_0.8.0-spark_2.4.3-0.5.1-python-api.zip
ENV PYSPARK_PYTHON /usr/bin/python3
ENV HADOOP_CONF_DIR /opt/spark/work-dir
