# au-nuclp-study23
Git repository for the Nuclear Power Studygroup at the Department of Physics and Astronomy at Aarhus University 2023


## OpenMC Setup

To install OpenMC follow the steps described in: [OpenMC Install Guide](https://docs.openmc.org/en/latest/quickinstall.html).

If you want to run OpenMC in a docker or podman container but want to run a jupyter-notebook on your
host maching you can use the following Dockerfile:

```Dockerfile
FROM docker.io/openmc/openmc:latest
RUN pip install --upgrade pip ipython ipykernel
RUN wget https://anl.box.com/shared/static/9igk353zpy8fn9ttvtrqgzvw1vtejoz6.xz
RUN tar -xf 9igk353zpy8fn9ttvtrqgzvw1vtejoz6.xz
ENV OPENMC_CROSS_SECTIONS=/endfb-vii.1-hdf5/cross_sections.xml
```

This will install the ipkernel in the Dockerimage. You can the setup jupyter
to connect to the running image.

Place the following in `~/.local/share/jupyter/kernels/openmc/kernel.json`


```json
{
 "argv": [
  "/usr/bin/docker",
  "run",
  "--network=host",
  "-v",
  "{connection_file}:/connection-spec",
  "-v",
  ".:/home/user",
  "-w",
  "/home/user",
  "openmc",
  "python",
  "-m",
  "ipykernel_launcher",
  "-f",
  "/connection-spec"
 ],
 "display_name": "openmc",
 "language": "python"
}
```
