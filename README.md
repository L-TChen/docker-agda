# Agda Docker Images on Apline Linux 

This is the Git repo of (the Dockerfiles for building) [Docker (Unofficial) Image](https://hub.docker.com/r/ltchentw/agda) for [`Agda`](https://hub.docker.com/repository/docker/ltchentw/agda/general).

## Image Details

### Included Tools

* [Agda](https://agda.readthedocs.io/).
* [Emacs](https://www.gnu.org/software/emacs/) for browsing Agda code.

### Processor Architecture Support

* amd64
* aarch64

## Maintenance

### Running

You can run the images locally without building:

```bash
$ docker run -it ltchentw/agda:2.7.0.1
```

### Building + Running Locally

If uncertain, you can build an image yourself and run the image locally:

```bash
$ docker build -t agda 2.7.0.1 && docker run -it agda
```
