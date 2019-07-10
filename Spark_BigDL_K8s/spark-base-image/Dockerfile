FROM openjdk:8-jre

ARG img_path=kubernetes/dockerfiles
ARG k8s_tests=kubernetes/tests

RUN set -ex && \
    apt-get update && apt-get upgrade -y && apt-get install python3 python3-pip -y && apt-get clean && \
    pip3 --no-cache install --upgrade pip setuptools && \
    mkdir -p /opt/spark && \
    touch /opt/spark/RELEASE && \
    rm /bin/sh && \
    ln -sv /bin/bash /bin/sh && \
    echo "auth required pam_wheel.so use_uid" >> /etc/pam.d/su && \
    chgrp root /etc/passwd && chmod ug+rw /etc/passwd

COPY dist/jars /opt/spark/jars
COPY dist/bin /opt/spark/bin
COPY dist/sbin /opt/spark/sbin
COPY entrypoint.sh /opt/
COPY dist/examples /opt/spark/examples
COPY dist/${k8s_tests} /opt/spark/tests
COPY dist/data /opt/spark/data

ENV SPARK_HOME /opt/spark

RUN mkdir ${SPARK_HOME}/python

COPY dist/python/lib /opt/spark/python/lib
ENV PYTHONPATH /opt/spark/python/lib/pyspark.zip:/opt/spark/python/lib/py4j-*.zip

WORKDIR /opt/spark/work-dir
ENTRYPOINT [ "/opt/entrypoint.sh" ]
