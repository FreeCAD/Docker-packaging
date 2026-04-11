# FreeCAD Containers

This repository contains example Dockerfiles that can be used to create containers that have all the required dependencies pre-installed that are needed to build FreeCAD from source.
Optional dependencies are not included.

You can use `podman` or `docker` interchangeably to build and run the containers.

The following distributions are supported, and were tested against the FreeCAD 1.1 release branch:
- Arch Linux (`archlinux:base-20260405.0.511327`)
- Debian Trixie (`debian:13`)
- Ubuntu 25.10 (`ubuntu:25.10`)
- Fedora 43 (`fedora:43`)

First, set an environment variable according to the distribution you want to use, as well as your FreeCAD source directory
```bash
export DISTR=<arch,debian,ubuntu,fedora>
export FC_SOURCE=<path to your FreeCAD source directory>
```

Then, create the build environment:
```bash
podman build --file Dockerfile.$DISTR --tag freecad-$DISTR
```

Run the image, mounting both the source and build directories:
```bash
podman run --rm --interactive --tty --volume $FC_SOURCE/builds:/builds:z --volume $FC_SOURCE:/fc freecad-$DISTR
```

Once inside the container, you can build FreeCAD:
```bash
cmake -S /fc -B fc-build -GNinja
cmake --build fc-build -j$(nproc --ignore=2)
```
