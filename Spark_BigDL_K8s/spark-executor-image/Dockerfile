FROM gitlab-registry.cern.ch/mbien/spark-artifactory/spark-base-image/spark:1.0

RUN yum install python36 python36-pip numpy scipy wget -y && pip3 --no-cache install --upgrade pip setuptools && pip3 install numpy scipy && pip3 install -U scikit-learn[alldeps] && yum clean all

ADD root4j-0.1.7.jar /opt/spark/jars
ADD spark-root_2.11-0.1.18.jar /opt/spark/jars

#AWS connector setup
RUN wget -P /opt/spark/jars/ https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/3.2.0/hadoop-aws-3.2.0.jar https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-bundle/1.11.375/aws-java-sdk-bundle-1.11.375.jar https://repo1.maven.org/maven2/org/apache/bcel/bcel/5.2/bcel-5.2.jar https://repo1.maven.org/maven2/jakarta-regexp/jakarta-regexp/1.4/jakarta-regexp-1.4.jar

#OCI connector setup
ADD oci-hdfs-connector-3.2.0.0.jar /opt/spark/jars/
RUN wget -P /opt/spark/jars/ https://repo1.maven.org/maven2/com/oracle/oci/sdk/oci-java-sdk-common/1.5.14/oci-java-sdk-common-1.5.14.jar https://repo1.maven.org/maven2/org/bouncycastle/bcpkix-jdk15on/1.60/bcpkix-jdk15on-1.60.jar https://repo1.maven.org/maven2/org/bouncycastle/bcprov-jdk15on/1.60/bcprov-jdk15on-1.60.jar https://repo1.maven.org/maven2/com/oracle/oci/sdk/oci-java-sdk-objectstorage/1.5.14/oci-java-sdk-objectstorage-1.5.14.jar https://repo1.maven.org/maven2/com/oracle/oci/sdk/oci-java-sdk-objectstorage-generated/1.5.14/oci-java-sdk-objectstorage-generated-1.5.14.jar https://repo1.maven.org/maven2/com/oracle/oci/sdk/oci-java-sdk-objectstorage-extensions/1.5.14/oci-java-sdk-objectstorage-extensions-1.5.14.jar https://repo1.maven.org/maven2/com/google/code/findbugs/jsr305/3.0.2/jsr305-3.0.2.jar https://repo1.maven.org/maven2/com/google/guava/guava/27.1-jre/guava-27.1-jre.jar https://repo1.maven.org/maven2/org/glassfish/jersey/media/jersey-media-json-jackson/2.27/jersey-media-json-jackson-2.27.jar

RUN wget -P /opt/spark/jars/ https://repo1.maven.org/maven2/org/tukaani/xz/1.2/xz-1.2.jar http://central.maven.org/maven2/com/intel/analytics/zoo/analytics-zoo-bigdl_0.8.0-spark_2.4.3/0.5.1/analytics-zoo-bigdl_0.8.0-spark_2.4.3-0.5.1-jar-with-dependencies.jar

RUN wget -P /opt/spark/python/lib http://central.maven.org/maven2/com/intel/analytics/zoo/analytics-zoo-bigdl_0.8.0-spark_2.4.3/0.5.1/analytics-zoo-bigdl_0.8.0-spark_2.4.3-0.5.1-python-api.zip

ENV PYTHONPATH /opt/spark/python/lib/pyspark.zip:/opt/spark/python/lib/py4j-0.10.7-src.zip:/opt/spark/python/lib/analytics-zoo-bigdl_0.8.0-spark_2.4.3-0.5.1-python-api.zip
ENV PYSPARK_PYTHON /usr/bin/python3
