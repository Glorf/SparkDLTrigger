FROM gitlab-registry.cern.ch/mbien/spark-artifactory/spark-executor-image/spark-py:1.0

RUN apk add --no-cache linux-headers && python3 -m pip install jupyter

ENV PYSPARK_DRIVER_PYTHON /usr/bin/python3

ENTRYPOINT ["jupyter", "notebook","--no-browser","--ip=0.0.0.0","--port=8888", "--allow-root"]
