> [!IMPORTANT]
> This repository has been archived and is no longer maintained.
> The code is provided for historical reference and may contain unpatched or unknown vulnerabilities.
> It should not be used in production systems.

# Installation

First, install the main conda environment (`idpp`). Mamba is preferred, but conda can be used as well (but will be slower).

`conda env create -f environment.yml`

Then, activate the environment:

`conda activate idpp`

Next, install pip dependencies:

`pip install -r requirements.txt`

Finally, manually install the following packages according to instructions in the home repos:

- DeepCCS

The above installation can be completed using `sbatch/hpc_setup.sbatch` if installing on Deception.

## GrAFF-MS

To install and use GrAFF-MS, use the associated environment spec.

`conda env create -f envs/graff-ms.yml`

You can also use `sbatch/graff-ms_setup.sbatch` to install the environment on Deception.

Then you will need to manually configure the path to the GrAFF-MS repository.

## Extra Installation Notes

- To install `torch_scatter`, first run `module load gcc/<newest_version>`
- Note that environment specs exist for many of the tools in the workflow, but the only tool requiring the user to pre-install its corresponding environment is GrAFF-MS (also DeepCCS requires the package to be installed in the main environment `idpp`).

# Getting Started

Before running the workflow, you must edit the configuration files to match your system. The following files require manual updating:

- config/cluster.yaml
- config/config.yaml
- profile/idpp.yaml

All parameters that require updating have their values in all uppercase (e.g. in config/config.yaml, the parameter `rtp_path` should be updated by replacing the uppercase text with the specified information: `rtp_path: RTP_REPOSITORY_PATH` -> `rtp_path: /path/to/rtp/`).

## HPC

Our HPC setup assumes Slurm is the system for submitting jobs for the cluster. Slurm parameters (in config/cluster.yaml and profile/idpp.yaml) are detailed in the [official Slurm documentation](https://slurm.schedmd.com/sbatch.html).

# Running the Workflow

To run the workflow, simply use the [`snakemake` command](https://snakemake.readthedocs.io/en/stable/executing/cli.html#all-options). We recommend the following options to start:

```bash
snakemake --jobs <N_JOBS> --latency-wait <WAIT> --scheduler greedy --use-conda --use-envmodules --directory <INPUT_DIRECTORY> --configfile config/config.yaml --cluster-config config/cluster.yaml --cluster "sbatch -A {cluster.account} -t {cluster.time} -J {cluster.name} --ntasks-per-node {cluster.ntasks} -p {cluster.partition}"
```

## Workflow Configuration

Parameters for running the general workflow are in the config/config.yaml file.

## Cluster Configuration

Parameters for running the general workflow are in the cluster/cluster.yaml file.

### Disclaimer:

This material was prepared as an account of work sponsored by an agency of the
United States Government.  Neither the United States Government nor the United
States Department of Energy, nor Battelle, nor any of their employees, nor any
jurisdiction or organization that has cooperated in the development of these
materials, makes any warranty, express or implied, or assumes any legal
liability or responsibility for the accuracy, completeness, or usefulness or
any information, apparatus, product, software, or process disclosed, or
represents that its use would not infringe privately owned rights.

Reference herein to any specific commercial product, process, or service by
trade name, trademark, manufacturer, or otherwise does not necessarily
constitute or imply its endorsement, recommendation, or favoring by the United
States Government or any agency thereof, or Battelle Memorial Institute. The
views and opinions of authors expressed herein do not necessarily state or
reflect those of the United States Government or any agency thereof.

                 PACIFIC NORTHWEST NATIONAL LABORATORY
                              operated by
                                BATTELLE
                                for the
                   UNITED STATES DEPARTMENT OF ENERGY
                    under Contract DE-AC05-76RL01830


# Related Repositories
- [Main identification probability analysis codebase](https://github.com/pnnl/idpp_main)
- [Retention time prediction model](https://github.com/pnnl/idpp_rtp)
