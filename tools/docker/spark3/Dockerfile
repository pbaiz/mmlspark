FROM ubuntu:16.04

ENV SPARK_VERSION=3.0.1
# ARG SPARK_VERSION=2.4.3  # Decided to change from ARG to ENV - this fixes the var after building
ENV HADOOP_VERSION 3.2.1

##################################
# Should we add Spark Scala??? SPARK_HOME
RUN echo "$LOG_TAG Getting SPARK_HOME" && \
    apt-get update && \
    # build deps and deps for c bindings for cntk
    apt-get install -y build-essential && \
    apt-get install -y autoconf automake libtool curl make unzip && \
    mkdir -p /opt && \
    cd /opt && \
    curl http://apache.claz.org/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-without-hadoop.tgz  | \
        tar -xz && \
    ln -s spark-${SPARK_VERSION}-bin-without-hadoop spark && \
    echo Spark ${SPARK_VERSION} installed in /opt/spark && \
    export SPARK_HOME=/opt/spark

##################################
# Config for Python and PySpark
RUN apt-get -qq update && apt-get -qq -y install curl bzip2 \
    && curl -sSL https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -o /tmp/miniconda.sh \
    && bash /tmp/miniconda.sh -bfp /usr/local \
    && rm -rf /tmp/miniconda.sh \
    && conda install -y python=3 \
    && conda update conda \
    && apt-get install default-jre -y \
	&& conda install -c conda-forge pyspark=${SPARK_VERSION} \
    && apt-get -qq -y remove curl bzip2 \
    && apt-get -qq -y autoremove \
    && apt-get autoclean \
    && rm -rf /var/lib/apt/lists/* /var/log/dpkg.log \
    && conda clean --all --yes

ENV PATH /opt/conda/bin:$PATH
ENV JAVA_HOME /usr/lib/jvm/java-1.8.0-openjdk-amd64

##################################
# Config for Hadoop
RUN echo "Downloading hadoop" && \
    apt-get update && \
    apt-get install -y wget && \
    cd /tmp && \
    wget http://apache.claz.org/hadoop/common/hadoop-${HADOOP_VERSION}/hadoop-${HADOOP_VERSION}.tar.gz -O - | \
    tar -xz && \
    mv /tmp/hadoop-${HADOOP_VERSION} /opt/hadoop && \
    echo "export HADOOP_CLASSPATH=/opt/hadoop/share/hadoop/tools/lib/*" >> /opt/hadoop/etc/hadoop/hadoop-env.sh && \
    echo Hadoop ${HADOOP_VERSION} installed in /opt/hadoop && \
    rm -rf /opt/hadoop/share/doc

RUN echo "\nSPARK_DIST_CLASSPATH=/jars:/jars/*:$(/opt/hadoop/bin/hadoop classpath)" >> /opt/spark/conf/spark-env.sh
ENV HADOOP_HOME=/opt/hadoop
ADD jars /jars

##################################
# Config for Jupyter Easy Start
RUN conda install jupyter \
    && jupyter-notebook --generate-config \
    && ipython profile create \
    && echo "c.NotebookApp.token = ''" >> ~/.jupyter/jupyter_notebook_config.py \
    && echo "c.NotebookApp.ip = '0.0.0.0'" >> ~/.jupyter/jupyter_notebook_config.py \
    && echo "c.NotebookApp.allow_root = True" >> ~/.jupyter/jupyter_notebook_config.py \
    && apt-get -qq -y autoremove \
    && apt-get autoclean \
    && rm -rf /var/lib/apt/lists/* /var/log/dpkg.log \
    && conda clean --all --yes

COPY tools/docker/demo/init_notebook.py /root/.ipython/profile_default/startup
COPY notebooks/samples notebooks
WORKDIR notebooks

##################################
# Flexibility to set the MMLSPARK_VERSION version, during build and remains after as ENV
ARG MMLSPARK_VERSION
ENV MMLSPARK_VERSION=${MMLSPARK_VERSION}

EXPOSE 8888
