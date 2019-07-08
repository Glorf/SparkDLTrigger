FROM gitlab-registry.cern.ch/mbien/spark-artifactory/spark-executor-image/spark-py:1.0

RUN apk add --no-cache linux-headers && python3 -m pip install jupyterlab

EXPOSE 8888

ENV KUBECONFIG /opt/kubernetes/config
ENV PYTHONPATH /opt/spark/python/lib/pyspark.zip:/opt/spark/python/lib/py4j-0.10.7-src.zip

ENTRYPOINT ["jupyter", "lab","--ip=0.0.0.0","--port=8888", "--allow-root"]
