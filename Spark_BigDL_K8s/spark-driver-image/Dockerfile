FROM gitlab-registry.cern.ch/mbien/spark-artifactory/spark-executor-image/spark-py:1.0

RUN apk add --no-cache linux-headers && python3 -m pip install jupyter

EXPOSE 8888

ENV PYTHONPATH /opt/spark/python/lib/pyspark.zip:/opt/spark/python/lib/py4j-0.10.7-src.zip
ENV PYSPARK_PYTHON /usr/bin/python3
ENV PYSPARK_DRIVER_PYTHON /usr/bin/python3

ENTRYPOINT ["jupyter", "notebook","--no-browser","--ip=0.0.0.0","--port=8888", "--allow-root"]
