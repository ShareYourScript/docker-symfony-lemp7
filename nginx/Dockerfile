FROM nginx:stable

MAINTAINER Mathieu A. <mathieu@shareyourscript.me>

ENV DEBIAN_FRONTEND noninteractive

ENV TZ=Europe/Paris
RUN echo $TZ | tee /etc/timezone
RUN dpkg-reconfigure --frontend noninteractive tzdata

RUN addgroup --system --gid 1008 wit
RUN useradd --system --create-home --shell /bin/bash --uid 1008 --gid wit wit
