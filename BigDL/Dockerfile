FROM docker.repository.cloudera.com/cdsw/engine:5

RUN apt-get  update && \
    apt-get install -y -q git maven
COPY requirements.txt /tmp/requirements.txt
RUN pip install -r /tmp/requirements.txt
RUN mkdir /opt/Intel
WORKDIR /opt/Intel
ADD https://repo1.maven.org/maven2/com/intel/analytics/bigdl/dist-spark-2.2.0-scala-2.11.8-all/0.7.0/dist-spark-2.2.0-scala-2.11.8-all-0.7.0-dist.zip /opt/Intel
RUN unzip dist*.zip
RUN rm /opt/Intel/*.zip
WORKDIR /opt/Intel/bin
RUN ./python_package.sh
WORKDIR /home/cdsw
ENV DL_ENGINE_TYPE=mklblas
ENV KMP_BLOCKTIME=0
ENV OMP_WAIT_POLICY=passive
ENV OMP_NUM_THREADS=1
ENV BigDL_HOME=/opt/Intel
ENV PYTHON_API_PATH=/opt/Intel/lib/bigdl-0.7.0-python-api.zip
ENV BigDL_JAR_PATH=/opt/Intel/lib/bigdl-SPARK_2.2-0.7.0-jar-with-dependencies.jar
ENV PYTHONPATH=/opt/Intel/lib/bigdl-0.7.0-python-api.zip:/opt/Intel/lib/bigdl-SPARK_2.2-0.7.0-jar-with-dependencies.jar
ENV VENV_HOME=/opt/Intel/bin
