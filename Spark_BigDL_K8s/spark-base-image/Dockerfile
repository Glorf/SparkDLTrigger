FROM cern/cc7-base:20190724

ARG img_path=kubernetes/dockerfiles
ARG k8s_tests=kubernetes/tests

RUN set -ex && \
    yum update -y && yum clean all && yum install java-1.8.0-openjdk-headless -y && \
    mkdir -p /opt/spark && \
    touch /opt/spark/RELEASE && \
    rm /bin/sh && \
    ln -sv /bin/bash /bin/sh && \
    echo "auth required pam_wheel.so use_uid" >> /etc/pam.d/su && \
    chgrp root /etc/passwd && chmod ug+rw /etc/passwd

RUN yum clean all

COPY dist_luca/jars /opt/spark/jars
COPY dist_luca/bin /opt/spark/bin
COPY dist_luca/sbin /opt/spark/sbin
COPY entrypoint.sh /opt/
COPY dist_luca/examples /opt/spark/examples
COPY dist_luca/${k8s_tests} /opt/spark/tests
COPY dist_luca/data /opt/spark/data

ENV SPARK_HOME /opt/spark

RUN mkdir ${SPARK_HOME}/python

COPY dist_luca/python/lib /opt/spark/python/lib
ENV PYTHONPATH /opt/spark/python/lib/pyspark.zip:/opt/spark/python/lib/py4j-*.zip

WORKDIR /opt/spark/work-dir
ENTRYPOINT [ "/opt/entrypoint.sh" ]
