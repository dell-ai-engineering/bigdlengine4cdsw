FROM docker.repository.cloudera.com/cdsw/engine:4

RUN apt-get  update && \
    apt-get install -y -q git maven
COPY requirements.txt /tmp/requirements.txt
RUN pip install -r /tmp/requirements.txt
RUN mkdir /opt/Intel
WORKDIR /opt/Intel
ADD https://oss.sonatype.org/content/groups/public/com/intel/analytics/zoo/analytics-zoo-bigdl_0.6.0-spark_2.1.1/0.2.0/analytics-zoo-bigdl_0.6.0-spark_2.1.1-0.2.0-dist-all.zip /opt/Intel
RUN unzip analytics*.zip
RUN rm /opt/Intel/*.zip
WORKDIR /opt/Intel/bin
RUN ./python_package.sh
WORKDIR /home/cdsw
ENV DL_ENGINE_TYPE=mklblas
ENV KMP_BLOCKTIME=0
ENV OMP_WAIT_POLICY=passive
ENV OMP_NUM_THREADS=1
ENV ANALYTICS_ZOO_HOME=/opt/Intel
ENV ANALYTICS_ZOO_PY_ZIP=/opt/Intel/lib/analytics-zoo-bigdl_0.6.0-spark_2.1.1-0.2.0-python-api.zip
ENV PYTHONPATH=/opt/Intel/lib/analytics-zoo-bigdl_0.6.0-spark_2.1.1-0.2.0-python-api.zip:/opt/Intel/lib/analytics-zoo-bigdl_0.6.0-spark_2.1.1-0.2.0-jar-with-dependencies.jar:${PYTHONPATH}
ENV VENV_HOME=/opt/Intel/bin
