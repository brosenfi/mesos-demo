
# Ubuntu 14.04 with Java 7 installed.
FROM mesosphere/mesos-master:1.1.0-2.0.107.ubuntu1404

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64
ENV JRE_HOME ${JAVA_HOME}/jre

# Adding java 8 and python 3.6 to support spark exec
RUN apt-get update && \
    apt-get install -y  software-properties-common && \
    add-apt-repository ppa:openjdk-r/ppa -y && \
    apt-get update && \
    apt-get install -y openjdk-8-jdk && \
	update-alternatives --set java "${JRE_HOME}/bin/java" && \
	java -version && \
    add-apt-repository ppa:jonathonf/python-3.6 && \
    apt-get update && \
    apt-get install -y python3.6 && \
	ln -sf /usr/bin/python3.6 /usr/bin/python3 && \
	ls -alt /usr/bin/python3 && \
    apt-get clean