FROM gitlab-registry.cern.ch/mbien/spark-artifactory/spark-executor-image/spark3:1.1

RUN pip3 install --upgrade --no-cache-dir six && pip3 install --no-cache-dir jupyter ipython #sparkmonitor

#RUN jupyter nbextension install sparkmonitor --py --user --symlink && jupyter nbextension enable sparkmonitor --py --user && jupyter serverextension enable --py --user sparkmonitor && ipython profile create && echo "c.InteractiveShellApp.extensions.append('sparkmonitor.kernelextension')" >>  $(ipython profile locate default)/ipython_kernel_config.py

ENV PYSPARK_DRIVER_PYTHON /usr/bin/ipython

ENTRYPOINT ["jupyter", "notebook","--no-browser","--ip=0.0.0.0","--port=8888", "--allow-root"]
