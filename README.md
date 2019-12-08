# singularity


## Definition

A [Singularity](https://sylabs.io/singularity/) container is a linux container optimized for HPC workloads. <span id="question-how-was-singularity-started"></span> It was started in 2016 at Lawrence Berkeley National Labs.

## Usage

### Docker

<span id='question-how-do-i-convert-from-docker-to-singularity'>Docker containers can be imported to Singularity, meaning that the Docker manifest is obtained to download the layers, and the layers are built into a Singularity binary. You would first want to make sure that your cluster supports Singularity, meaning that it's installed:

```bash
which singularity
```

should return a path. If not, you should check your cluster's documentation to see if it might be loaded as a module. Once you have confirmed that you have it, since you'll be pulling a container it's important to not be on a login node. Grab a development or interactive node (on some clusters this is done with `sdev`, or you can do `srun --pty bash`). Once you have an interactive node, then Singularity provides an easy way to [pull directly from Docker Hub](https://sylabs.io/guides/3.4/user-guide/cli/singularity_pull.html). Do that:

```bash
$ singularity pull docker://vanessa/salad
```
The container is now a binary file (a squashfs read only file called a SIF) on your file system and you can run it as you please:

```bash
singularity run salad_latest.sif
./salad_latest.sif
singularity shell salad_latest.sif
singularity inspect salad_latest.sif
singularity exec salad_latest.sif echo "hello!"
```

There are a few important points to make. The user that you are outside the container is the same as within, so if the Docker container is built with content in root's home (/root) or needing a root permission, you won't be able to do this. If you find you are running into trouble, take a look at the Dockerfile and see if you can figure out why. If it comes down to permissions, you can push your own container to Docker Hub to pull as you just did above.

<span id='question-how-do-i-build-from-a-local-docker-daemon'>With recent singularity version 3.x and greater, you can also build directly from a local docker daemon. If I had a docker container image tagged as `my_container:latest` on my local computer, I would build from it as follows:

``` bash
singularity build my_container.sif docker-daemon://my_container:latest
```

Once you build your container you can transfer the `my_container.sif` file to the cluster of choice.

## History

<span id="question-where-does-the-term-singularity-originate"></span>"Singularity" was meant to indicate the final, holistic entrypoint for creating encapsulated environments. We'll see how that works out.

## Tutorials

 - [Using Singularity on an HPC Cluster](https://github.com/bdusell/singularity-tutorial) <span id='question-how-do-i-use-singularity-on-an-hpc-cluster'> This tutorial is for using Singularity on the CRC computing cluster at the University of Notre Dame, which uses the SGE job scheduling system. It serves as a general introduction to containers and may be applicable to other computing clusters as well. The tutorial walks you through the process of building a container and running a GPU-accelerated PyTorch program on a GPU node. It also has tips for managing Python packages efficiently.
 - [Harvard Using Singularity on Odyssey](https://www.rc.fas.harvard.edu/resources/documentation/software/singularity-on-odyssey/): a tutorial for using SBATCH or an interactive session with slurm to build containers.
- [Using Singularity](https://www.osc.edu/resources/getting_started/howto/howto_use_docker_and_singularity_containers_at_osc):  at the Ohio Supercomputing Center

## References

 - [Sylabs User Guide](https://sylabs.io/guides/latest/user-guide/)
 - [Singularity on GitHub](https://github.com/sylabs/singularity)





