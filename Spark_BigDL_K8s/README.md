 # An ultimate guide to spark@kubernetes in client mode, with s3 support, interactive notebook, etc etc
1) Copying ROOT data from analytix hdfs to openstack s3 artifactory of your choice:
    * Request access to the hadoop cluster
    * In your shell `ssh it-hadoop-client`
    * If ssh'ed successfully, `source hadoop-setconf.sh <cluster_name>` (where cluster_name is eg. analytix)

    * Now copy the files\
     `hadoop fs -Dfs.s3a.fast.upload=true -Dfs.s3a.access.key="<your_s3_access_key> -Dfs.s3a.secret.key="<your s3 secret key>" -Dfs.s3a.endpoint="https://s3.cern.ch" -Dfs.s3a.impl="org.apache.hadoop.fs.s3a.S3AFileSystem" -cp <path to the file on hfs> s3a://<container name>/<container folder>`

1) Building general pyspark dockerimage
    * Compile spark-2.4 with hadoop-3.2 and scala 2.11 on java8, run the script to make a distribution\
    Why spark-2.4? \
    BigDL framework that we're using is not yet compatible with incoming spark-3.0\
    Why hadoop-3.2? \
    It looks like hadoop-2.7 has some problems to serve s3a properly with kubernetes running in client mode.\
    Why scala 2.11? \
    This is important: spark-root package was build last time with scala 2.11. If you don't want to recompile it, just stick with this version\
    Curator 2.12.0 and zookeeper 3.4.13 are changed to allow hadoop 3.2.0. Spark 2.4 doesn't yet support hadoop-3.2 profile so replaced it with tuned 3.1\
    Why java8? \
    Spark-2.4 comes with some deprecated interfaces that were removed in java9. If you try to compile it, it will crash.\
    Run the following script to start the compilation:\
    `./dev/make-distribution.sh --pip --tgz  -Phadoop-3.1 -Dhadoop.version=3.2.0 -Dcurator.version=2.12.0 -Dzookeeper.version=3.4.13 -Pyarn -Pkubernetes`

    * Prepare spark docker image and push it to the artifactory of your choice\
    `./bin/docker-image-tool.sh -r <registry url> -t <build-tag> build`\
    `./bin/docker-image-tool.sh -r <registry url> -t <build-tag> push`\
    Prebuilt image (feel free to use it): \
    `gitlab-registry.cern.ch/mbien/spark-artifactory/spark-base-image/spark-py:1.0`

1) Building enriched executor dockerimage \
We need to enrich pyspark image with some spicy additions.\
This is in theory entirely possible to do so using spark configuration jars, packages and files flags. But extensive tests show that in the current version of spark client mode on k8s its better to just put everything into the image (it'd have to be downloaded anyway...) For python packages we use `pip install` for java ones, we simply download them from maven repo using `wget`\
    * numpy
    * sklearn
    * BigDL (both java lib and python wrapper, copied local copy to container)
    * org.apache.hadoop:hadoop-aws:3.2.0
    * com.amazonaws:aws-java-sdk-bundle:1.11.375 (hadoop-aws dependency)
    * spark-root_2.11-0.1.17.jar (downladed from: https://github.com/diana-hep/spark-root)
    * org.diana-hep:root4j:0.1.6
    * org.tukaani:xz:1.2 (root4j dependency)
    * org.apache.bcel:bcel:5.2 (root4j dependency)
    * jakarta-regexp:jakarta-regexp:1.4 (bcel dependency)

    Build the image using provided Dockerfile, and push it to your artifactory. It should then work properly without any other alignments needed\
Consider using *relatively* new version of docker (released >= 2018), as spark scripts use latest Docker features\
Prebuilt image:\
` gitlab-registry.cern.ch/mbien/spark-artifactory/spark-executor-image/spark-py:1.0`

1) Building driver dockerimage\
This image will be (probably) run outside of the cluster and have to provide at least
    * pyspark to run the shell with all the plugins and extensions configured the same way as in executors
    * python in the same version as in executor image (!!!)
    * jupyter-notebook/lab to allow session to be accessed remotely
    * decent networking configuration (or `--network="host"` to insecurely escape sandbox)\

    Build the image using provided Dockerfile, and push it to your artifactory if you want.\
Prebuilt image: \
`gitlab-registry.cern.ch/mbien/spark-artifactory/spark-driver-image/spark-py:1.0`



1) Prepare driver machine\
Driver machine should have iptables setup so that:
    * It accepts all ingress from executors subnet
    * It accepts connections to and from jupyter notebook, at least regarding the client computer (but keep in mind that notebooks are protected by token when run outside of localhost anyway)

    There should be also docker service installed and running there.\
You should have connection to your selected k8s cluster configured, then run kubectl proxy to pass the authenticated connection to the spark driver. \
To run the driver, perform:\
`docker run --network="host" gitlab-registry.cern.ch/mbien/spark-artifactory/spark-driver-image/spark-py:1.0`

1) Prepare core-site.xml file\
Due to problems with passing parameters in runtime from spark driver to executors, you should prepare your own core-site.xml file and feed it to environment using sparkContext.addFile. Docker containers are configured the way they will detect core-site.xml file in their workdir and apply configuration to their hadoop clients. The sample core-site.xml file can be found in this directory.

1) Run notebook

Prepare session\
Fill missing configuration, upload files, and run spark session.\
Wait for kubernetes pods to populate \
Run the workflow of your choice \
Enjoy!
