FROM nginx

# For serving up the spark binary - can also use a dockerized spark image
RUN apt-get update && \
    apt-get install -y wget && \
	cd /usr/share/nginx/html && \
    wget http://apache.claz.org/spark/spark-2.2.1/spark-2.2.1-bin-hadoop2.7.tgz