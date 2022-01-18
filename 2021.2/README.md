# docker-ubuntu-vitis
Dockerfile to create Vitis 2021.2 installation under Ubuntu.

Modified from the 2019.2 version from <https://github.com/laysakura/docker-ubuntu-vivado>.

To build:

```bash
docker image build -t vitis-2021.2 .
```

To run:
```bash
docker run -e DISPLAY=`hostname`:0 -it --rm -v $PWD:/home/jlamperez/work -w /home/jlamperez vivado-2021.2
```
