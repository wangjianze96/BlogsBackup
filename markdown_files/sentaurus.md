# Tutorial for Sentaurus (Synopsys)

## Sentaurus workbench

### Starting Sentaurus

```sh
$ cd <workspace_folder>
$ module load singularity
$ singularitysetup_sentaurus
$ tcsh
$ load_sentaurus
$ setenv STDB <project_folder>
$ swb &
```

### Running Project

![](./sentaurus-imgs/fig1.png)
The tool flow refers to the sequence of simulation tools and their associated input files. 
In the SimpleMOS example, these are `Sentaurus Process`, `Sentaurus Device`, and `Inspect` as seen in Figure 1. 
Below the tool flow, the project parameters (Lg, NWell, and so on) and the corresponding simulation nodes ([n1], [n2], and so on) are listed.

To display node numbers: **`View > Tree Options > Show Node Numbers`** (or press the `F9` key).
