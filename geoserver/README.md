# GeoServer 2.13.2

This is inspired by https://github.com/kartoza/docker-geoserver

GeoServer version 2.13.2 docker image. This docker image builds with ElasticGeo plugin to connect to ElasticSearch. The ElasticGeo version is 2.13.0. The ElasticGeo version should match with GeoSever version to make it work.

There is nothing different other than downloading required jars from AWS S3 and adding ElasticGeo Jar to connect to ElasticSearch. 

Please make sure all the required jars are in S3 before you run following command.
IMAGE_TAG=latest
S3_BUCKET_URL=<BUCKET_URL>

docker build --build-arg IMAGE_TAG=${IMAGE_TAG} --build-arg S3_BUCKET_URL=${S3_BUCKET_URL} -t geoserver:${IMAGE_TAG}