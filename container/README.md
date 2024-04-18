# Docker container

Please note that this container is hosted on [docker hub](https://hub.docker.com/r/linsalrob/phables) and we recommend you use the latest version there.

# Running the container

You will need to use the [Gurobi WSL]( ) license, and then you can use a command along the lines of:

```
sudo docker run --volume=$PWD/Sim_Phage:/Sim_Phage --volume=$PWD/gurobi.lic:/opt/gurobi/gurobi.lic:ro phables phables run --input /Sim_Phage/assembly_graph_after_simplification.gfa --reads /Sim_Phage/reads/
```

We are actively making this work.
