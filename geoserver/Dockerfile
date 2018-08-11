
# Geoserver 2.13.2 Dockerfile

FROM tomcat:latest

ARG S3_BUCKET_URL

# This Dockerfile is maintained by Vivek Subedi
MAINTAINER shree665@gmail.com

# Set Dockerfile environment variables
ENV GEOSERVER_VERSION 2.13.2
ENV GEOSERVER_TEMP_NAME geoserver.zip
ENV GEOTOOLS_TEMP_NAME geotools.zip
ENV ELASTICGEO_NAME elasticgeo-2.13.0.jar
ENV CONTROL_FLOW gs-control-flow-2.13.2.jar
ENV GUAVA_NAME guava-17.0.jar
ENV GENERIC_SLD generic.sld
ENV GENERIC_XML generic.xml
ENV S3_BUCKET_URL ${S3_BUCKET_URL}/geoserver
ENV GEOSERVER_XSTREAM_WHITELIST com.bah.na.asc.**;org.geoserver.**

# Update curl and bash
RUN apk add --update curl bash && \
    rm -rf /var/cache/apk/*

# Download GeoServer Jars: GeoServer, ElasticGeo, and Guava
RUN set -x \
    && curl -fSLk $S3_BUCKET_URL/geoserver-2.13.2-war.zip -o $GEOSERVER_TEMP_NAME \
    && curl -fSLk $S3_BUCKET_URL/elasticgeo-2.13.0.jar -o $ELASTICGEO_NAME \ 
    && curl -fSLk $S3_BUCKET_URL/guava-17.0.jar -o $GUAVA_NAME \
    && curl -fSLk $S3_BUCKET_URL/gs-control-flow-2.13.2.jar -o $CONTROL_FLOW

# Set up the webapps folder
RUN unzip $GEOSERVER_TEMP_NAME -d $CATALINA_HOME/webapps \
    && ls -la $CATALINA_HOME \
    && mkdir $CATALINA_HOME/webapps/geoserver \
    && unzip $CATALINA_HOME/webapps/geoserver.war -d $CATALINA_HOME/webapps/geoserver \
    && rm  $CATALINA_HOME/webapps/*txt \
    && rm -rf $CATALINA_HOME/webapps/target


# Copy in Generic Styling
COPY $GENERIC_XML $GENERIC_XML
COPY $GENERIC_SLD $GENERIC_SLD

# Cleanup and move files to correct dir
RUN rm $CATALINA_HOME/webapps/geoserver/WEB-INF/lib/guava*.jar \
	&& rm $CATALINA_HOME/webapps/geoserver/data/styles/generic.xml \
    && mv $ELASTICGEO_NAME $CATALINA_HOME/webapps/geoserver/WEB-INF/lib/ \
    && mv $GUAVA_NAME $CATALINA_HOME/webapps/geoserver/WEB-INF/lib/ \
    && mv $CONTROL_FLOW $CATALINA_HOME/webapps/geoserver/WEB-INF/lib/ \
    && mv $GENERIC_XML $CATALINA_HOME/webapps/geoserver/data/styles/ \
    && mv $GENERIC_SLD $CATALINA_HOME/webapps/geoserver/data/styles/

# Replace web.xml for CORS
RUN rm $CATALINA_HOME/conf/web.xml
ADD web.xml $CATALINA_HOME/conf/

RUN set -x; rm -rf $GEOSERVER_TEMP_NAME

CMD ["catalina.sh", "run"]
