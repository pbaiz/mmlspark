


docker exec -it ubuntu bash

RUN  apt-get update \
  && apt-get install -y wget \
  && rm -rf /var/lib/apt/lists/*

docker build -f /home/pedro/src/mmlspark/tools/docker/spark3/Dockerfile \
    -t mmlspark_exate:0.1 .