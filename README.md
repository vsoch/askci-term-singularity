# singularity


## Definition

A [Singularity](https://sylabs.io/singularity/) container is a linux container optimized for HPC workloads. <span id="question-how-was-singularity-started"></span> It was started in 2016 at Lawrence Berkeley National Labs.

## Usage


### Build from Docker

<span id='question-why-cant-docker-be-used-on-hpc'></span> Docker is not used on many HPC sites due to security concerns. That is primarily because Docker can be misused by a regular user to obtain superuser access. However, alternative container runtimes do exist which try to avoid the security issue and offer similar features. As an added bonus they usually support importing existing Docker images. Conversion of images becomes trivial.

<span id='question-how-do-i-convert-from-docker-to-singularity'></span>Docker containers can be imported to Singularity, meaning that the Docker manifest is obtained to download the layers, and the layers are built into a Singularity binary. You would first want to make sure that your cluster supports Singularity, meaning that it's installed:

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

<span id='question-how-do-i-build-from-a-local-docker-daemon'></span>With recent singularity version 3.x and greater, you can also build directly from a local docker daemon. If I had a docker container image tagged as `my_container:latest` on my local computer, I would build from it as follows:

``` bash
singularity build my_container.sif docker-daemon://my_container:latest
```

Once you build your container you can transfer the `my_container.sif` file to the cluster of choice.

## History

<span id="question-where-does-the-term-singularity-originate"></span>"Singularity" was meant to indicate the final, holistic entrypoint for creating encapsulated environments. We'll see how that works out.

## Tutorials

### Academic

 - [Cyverse Container Camp](https://learning.cyverse.org/projects/container_camp_workshop_2019/en/latest/singularity/singularityintro.html) updated in 2019.
 - [Using Singularity on an HPC Cluster](https://github.com/bdusell/singularity-tutorial) <span id='question-how-do-i-use-singularity-on-an-hpc-cluster'> This tutorial is for using Singularity on the CRC computing cluster at the University of Notre Dame, which uses the SGE job scheduling system. It serves as a general introduction to containers and may be applicable to other computing clusters as well. The tutorial walks you through the process of building a container and running a GPU-accelerated PyTorch program on a GPU node. It also has tips for managing Python packages efficiently.
 - [Harvard Using Singularity on Odyssey](https://www.rc.fas.harvard.edu/resources/documentation/software/singularity-on-odyssey/): a tutorial for using SBATCH or an interactive session with slurm to build containers.
 - [Using Singularity](https://www.osc.edu/resources/getting_started/howto/howto_use_docker_and_singularity_containers_at_osc):  at the Ohio Supercomputing Center
 - [Singularity at Argonne](https://www.alcf.anl.gov/support-center/theta/singularity-theta) published February 2019
 - [Washington State University](https://hpc.wsu.edu/programmers-guide/singularity/) Singularity 3.0.x guide.
 - [OSIP 2019](https://github.com/rainsworth/osip2019-containerisation-workshop/) containerization workshop.
 - [UL HPC Tutorials](https://ulhpc-tutorials.readthedocs.io/en/latest/containers/singularity/) published in 2018.
 - [Vanderbilt University](https://www.vanderbilt.edu/accre/documentation/singularity/) running containers on ACCRE.
 - [Singularity on Sherlock](https://www.sherlock.stanford.edu/docs/software/using/singularity/) at Stanford University.
 - [Singularity at Case Western](https://sites.google.com/a/case.edu/hpcc/installed-software/container-platforms/singularity)
 - [Singularity at Yale](https://docs.ycrc.yale.edu/clusters-at-yale/guides/singularity/)

### Singularity Verison 2.x

 - [Singularity Tutorial](https://singularity-tutorial.github.io/) originally derived from an NIH tutorial
 - [Singularity Quick Start](https://singularity-docs.readthedocs.io/en/latest/) on readthedocs
- [Singularity 2.x Tutorials](https://singularity.lbl.gov/tutorials) although 2.x is now deprecated, these tutorials might still be useful to interested users.

### Industry

- [Using GPU with Nextflow](https://medium.com/@lucacozzuto/using-gpu-within-nextflow-19cd185d5e69?) and see the [singularity page](https://www.nextflow.io/docs/latest/singularity.html) on the Nextflow documentation.
- [Singularity Containers](https://cloud.google.com/community/tutorials/singularity-containers-with-cloud-build) with Google Cloud Build.
- [Singularity with Tensorflow](https://medium.com/singularityapp/singularity-containers-tensorflow-and-the-nvidia-jetson-nano-an-experiment-5be66ebbd4ac?): using Singularity containers with Tensorflow and the Nvidia jetson Nano, an experiment.
 - [Containers with CUDA Support](https://medium.com/@lpryszcz/containers-with-cuda-support-5467f393649f?)


## Presentations

 - [Singularity with Nomad](https://www.hashicorp.com/resources/singularity-nomad-task-driver-plugins) By Eduardo from Sylabs.
 - [Singularity for Public Health Bioinformatics](https://www.youtube.com/watch?v=juPLTMnFrcI&feature=youtu.be) on youtube.
 - [Singularity Containers for Science](https://www.slideshare.net/VanessaSochat/pearc17-reproducibility-and-containers-the-perfect-sandwich) PEARC 2017 presentation by [@vsoch](https://github.com/vsoch) of Stanford Research Computing. 
 - [Singularity for Scientific Compute](https://www.slideshare.net/VanessaSochat/singularity-containers-for-scientific-compute) a presentation on Singularity and Singularity Hub to the SCG users group, circa 2017 or 2018.

## References

 - [Sylabs User Guide](https://sylabs.io/guides/latest/user-guide/)
 - [Singularity on GitHub](https://github.com/sylabs/singularity)



