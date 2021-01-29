# Repo/Image to access ML/AI Capabilities

This repository/image facilitates ML/AI capabilities in relation to Privacy Preserving Techniques (PETs). 




## To build the image 

```
docker build -f /home/pedro/src/mmlspark/tools/docker/spark3/Dockerfile -t mmlspark_exate:0.1 .
```


## To run the image

Go to the right repo/folder that contains your notebooks and type:

```
docker run -it --rm \
           -p 127.0.0.1:8887:8888 \
           -v $PWD/notebooks/docker_files:/notebooks/docker_files \
           mmlspark_exate:0.1 
```


