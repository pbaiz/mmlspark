


docker exec -it ubuntu bash

RUN  apt-get update \
  && apt-get install -y wget \
  && rm -rf /var/lib/apt/lists/*

# To build the image 

docker build -f /home/pedro/src/mmlspark/tools/docker/spark3/Dockerfile -t mmlspark_exate:0.1 .

# To run the image

Go to the right repo/folder that contains your notebooks and type:


docker run -it --rm \
           -p 127.0.0.1:8887:8888 \
           -v $PWD/notebooks/docker_files:/notebooks/docker_files \
           mmlspark_exate:0.1 



