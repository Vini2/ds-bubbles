# Docker container

Please note that this container is hosted on [docker hub](https://hub.docker.com/r/linsalrob/phables) and we recommend you use the latest version there.

# Installing guorobi

For the linear solver, you need the [Gurobi WLS](https://www.gurobi.com/features/academic-wls-license/) license. Get that file, which is called `gurobi.lic` by default, and put it in your home directory, or another location that you know where it is.

# Running the container with singularity (recommended)

We need to mount three locations that are writable for `phables` to work with singularity.

1. You need to mount the `gurobi.lic` file, and that needs to end up at `/opt/gurobi/gurobi.lic`. In the example here, it is in the current working directory, `$PWD`.
2. You need a temporary directory where conda can install some files. They are installed on the first run, and reused after that. In this example, I am using `/tmp`. You need to mount this to `/conda` which is actually a symlink to the correct location under the snakemake directory.
3. You need your `.gfa` and `.fastq` files, and a location for the output. In this example, I have a directory called `Sim_Phage`. You should mount this to `/phables`. An important point here is to add the `/` to the end of your directory name, but _not_ to the `/phables`, and then the `.gfa` and `reads` will be in the `/phables` directory. 

> **NOTE:** when you specify the paths, it is important that they are absolute paths (i.e. beginning with `$PWD` or `/`), as relative paths don't work.


## Create the `.sif` image

The first step is to create the .sif image in a directory.

Check [docker hub](https://hub.docker.com/r/linsalrob/phables) for the latest version. In this example, I'm using version 0.6 but it may have been updated after that.

```
IMAGE_DIR=<path to singularity image>
mkdir -p $IMAGE_DIR
singularity pull --dir $IMAGE_DIR docker://linsalrob/phables:v0.5_sneaky_sleeky
```

You can set `IMAGE_DIR` to any path you can write to.


## Run the container

```
singularity exec --bind /tmp:/conda,$PWD/Sim_Phage/:/phables,$PWD/gurobi.lic:/opt/gurobi/gurobi.lic singularity/phables_0.6_gogo phables run --input /phables/assembly_graph_after_simplification.gfa --reads /phables/reads/ --output /phables/phables --threads 32
```

# Running the container with docker

The approach is very similar, except instead of the `--bind` you need to use `--volume`. Note that you will need to have root access for this to work.

```
docker pull linsalrob/phables:v0.6_gogo

sudo docker run --volume=$PWD/Sim_Phage/:/phables --volume=/tmp:/conda --volume=$PWD/gurobi.lic:/opt/gurobi/gurobi.lic:ro phables phables run --input /phables/assembly_graph_after_simplification.gfa --reads /phables/reads/ --output /phables/phables --threads 32
```



singularity exec --bind /scratch/pawsey1018/edwa0468/tmp:/opt/miniforge3/lib/python3.10/site-packages/phables/workflow/conda,$PWD/testy/Sim_Phage/:/phables,$PWD/gurobi.lic:/opt/gurobi/gurobi.lic testy/phables_v0.5_sneaky_sleeky.sif phables run --input /phables/assembly_graph_after_simplification.gfa --reads /phables/reads/ --output /phables/phables --threads 32
