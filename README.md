# miktex
port [miktex/miktex](https://hub.docker.com/r/miktex/miktex) + additional packs for miktex

## Using the image
### Volumes
#### Input files
The host directory containing the input files must be mounted to the container path /miktex/work.

To mount the current host working directory, you would pass option -v $(pwd):/miktex/work to the Docker run command.

#### Package files
The container image contains a bare MiKTeX setup and MiKTeX is configured to install missing package files in the container directory /miktex/.miktex. It is recommended that you mount this directory to a dedicated Docker volume.

### Running as root or not
By default, all commands inside the container are executed as root.

It is possible to execute as a dedicated host user by setting the container environment variables MIKTEX_GID and MIKTEX_UID.

To execute as the current user, you would pass options -e MIKTEX_GID=$(id -g) and -e MIKTEX_UID=$(id -u) to the Docker run command.

### Example
First, create a Docker volume named miktex:

```
docker volume create --name miktex
```

This volume will be used to mount the the container directory /miktex/.miktex in subsequent runs.

Provided that your main input file is located in the current working directory, you can run xelatex as follows:

```
docker run --rm -ti \
  -v miktex:/miktex/.miktex \
  -v $(pwd):/miktex/work \
  -e MIKTEX_GID=$(id -g) \
  -e MIKTEX_UID=$(id -u) \
  zrabadaber/miktex \
  xelatex -interaction=nonstopmode -job-name=main main.tex
```
