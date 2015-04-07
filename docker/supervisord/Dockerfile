#
# This Docker image encapsulates Maltrieve, a tool to retrieve malware
# directly from the source for security researchers.
# which was created by Kyle Maxwell (technoskald) and is
# available at https://github.com/technoskald/maltrieve.
#
# The file below is based on ideas from the original Dockerfile contained in the
# repository one level above, but modified to leverage supervisord
#

FROM ubuntu:14.10
MAINTAINER Ryan Berg <ryan@cylentsystems.com>

RUN apt-get update && \
  apt-get install -y --no-install-recommends \
    gcc \
    git \
    libpython2.7-stdlib \
    python2.7 \
    python2.7-dev \
    python-pip \
    supervisor \
    python-setuptools && \

  rm -rf /var/lib/apt/lists/* && \

  groupadd -r maltrieve && \
  useradd -r -g maltrieve -d /home/maltrieve -s /sbin/nologin -c "Maltrieve User" maltrieve
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
WORKDIR /home
RUN git clone -b dev https://github.com/cylentsystems/maltrieve.git && \
  cd maltrieve && \
  pip install -r requirements.txt && \
  chown -R maltrieve:maltrieve /home/maltrieve
COPY maltrieve.cfg /home/maltrieve/maltrieve.cfg
CMD ["/usr/bin/supervisord -c /etc/supervisor/conf.d/supervisord.conf"]
