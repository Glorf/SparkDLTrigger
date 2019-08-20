FROM gitlab-registry.cern.ch/mbien/spark-artifactory/spark-base-image/spark:1.0-2.4

RUN yum install https://kojipkgs.fedoraproject.org/packages/python36/3.6.8/1.el7/$(rpm --eval %_arch)/python36{,-libs}-3.6.8-1.el7.$(rpm --eval %_arch).rpm -y
RUN yum install python36-pip numpy scipy wget -y && pip3 --no-cache install --upgrade pip setuptools && pip3 install numpy scipy six && pip3 install -U scikit-learn[alldeps] && yum clean all

ADD root4j-0.1.7.jar /opt/spark/jars
ADD spark-root_2.11-0.1.18.jar /opt/spark/jars

#AWS connector setup
RUN wget -P /opt/spark/jars/ https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/3.2.0/hadoop-aws-3.2.0.jar https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-bundle/1.11.375/aws-java-sdk-bundle-1.11.375.jar https://repo1.maven.org/maven2/org/apache/bcel/bcel/5.2/bcel-5.2.jar https://repo1.maven.org/maven2/jakarta-regexp/jakarta-regexp/1.4/jakarta-regexp-1.4.jar

#OCI connector setup
ADD oci-hdfs-connector-2.7.7.3.jar /opt/spark/jars/
ADD sparkexecutorplugins_2.11-0.1.jar /opt/spark/jars/

RUN wget -P /opt/spark/jars/ https://repo1.maven.org/maven2/org/tukaani/xz/1.2/xz-1.2.jar http://central.maven.org/maven2/com/intel/analytics/zoo/analytics-zoo-bigdl_0.8.0-spark_2.4.3/0.5.1/analytics-zoo-bigdl_0.8.0-spark_2.4.3-0.5.1-jar-with-dependencies.jar

RUN wget -P /opt/spark/python/lib http://central.maven.org/maven2/com/intel/analytics/zoo/analytics-zoo-bigdl_0.8.0-spark_2.4.3/0.5.1/analytics-zoo-bigdl_0.8.0-spark_2.4.3-0.5.1-python-api.zip

ENV PYTHONPATH /opt/spark/python/lib/pyspark.zip:/opt/spark/python/lib/py4j-0.10.8.1-src.zip:/opt/spark/python/lib/analytics-zoo-bigdl_0.8.0-spark_2.4.3-0.5.1-python-api.zip
ENV PYSPARK_PYTHON /usr/bin/python3
