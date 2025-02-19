# Jupyter

## Overview

Use `Jupyter Task` to create a jupyter-type task and execute jupyter notes. When the worker executes `Jupyter Task`,
it will use `papermill` to evaluate jupyter notes. Click [here](https://papermill.readthedocs.io/en/latest/) for details about `papermill`.

## Conda Configuration
 
- Config `conda.path` in `common.properties` to the path of your `conda.sh`, which should be the same `conda` you use to manage the python environment of your `papermill` and `jupyter`.
Click [here](https://docs.conda.io/en/latest/) for more information about `conda`.
- `conda.path` is set to `/opt/anaconda3/etc/profile.d/conda.sh` by default. If you have no idea where your `conda` is, simply run `conda info | grep -i 'base environment'`.

> NOTE: `Jupyter Task Plugin` uses `source` command to activate conda environment. 
> If your tenant does not have permission to use `source`, `Jupyter Task Plugin` will not function. 


## Python Dependency Management

### Use Pre-Installed Conda Environment

1. Create a conda environment manually or using `shell task` on your target worker.
2. In your `jupyter task`, set `condaEnvName` as the name of the conda environment you just created. 

### Use Packed Conda Environment

1. Use [Conda-Pack](https://conda.github.io/conda-pack/) to pack your conda environment into `tarball`.
2. Upload packed conda environment to `resource center`.
3. Set `condaEnvName` as the name of your packed conda environment in your `jupyter task`, e.g. `jupyter_env.tar.gz`.
4. Select your packed conda environment as `resource` in your `jupyter task`, e.g. `jupyter_env.tar.gz`.

> NOTE: Make sure you follow the [Conda-Pack](https://conda.github.io/conda-pack/) official instructions. 
> If you unpack your packed conda environment, the directory structure should be the same as below:

```
.
├── bin
├── conda-meta
├── etc
├── include
├── lib
├── share
└── ssl
```   

> NOTICE: Please follow the `conda pack` instructions above strictly, and DO NOT modify `bin/activate`.
> `Jupyter Task Plugin` uses `source` command to activate your packed conda environment.
> If you are concerned about using `source`, choose other options to manage your python dependency.   

### Construct From Requirements

1. Upload or create a `.txt` file of requirements with your python dependencies in `Resource Center`.
2. Set `condaEnvName` as the name of your file of requirements in your `jupyter task`, e.g. `requirements.txt`.
3. Select your file of requirements as `resource` in your `jupyter task`, e.g. `requirements.txt`.

Here is an example file of requirements, from which `jupyter task plugin` will automatically 
construct your python dependencies, run your python code and finally tear down the environment:

```text
fastjsonschema==2.15.3
fonttools==4.33.3
geojson==2.5.0
identify==2.4.11
idna==3.3
importlib-metadata==4.11.3
importlib-resources==5.7.1
ipykernel==5.5.6
ipython==8.2.0
ipython-genutils==0.2.0
jedi==0.18.1
Jinja2==3.1.1
json5==0.9.6
jsonschema==4.4.0
jupyter-client==7.3.0
jupyter-core==4.10.0
jupyter-server==1.17.0
jupyterlab==3.3.4
jupyterlab-pygments==0.2.2
jupyterlab-server==2.13.0
kiwisolver==1.4.2
MarkupSafe==2.1.1
matplotlib==3.5.2
matplotlib-inline==0.1.3
mistune==0.8.4
nbclassic==0.3.7
nbclient==0.6.0
nbconvert==6.5.0
nbformat==5.3.0
nest-asyncio==1.5.5
notebook==6.4.11
notebook-shim==0.1.0
numpy==1.22.3
packaging==21.3
pandas==1.4.2
pandocfilters==1.5.0
papermill==2.3.4
``` 

## Create Task

- Click `Project Management-Project Name-Workflow Definition`, and click the `Create Workflow` button to enter the DAG editing page.
- Drag <img src="../../../../img/tasks/icons/jupyter.png" width="15"/> from the toolbar to the canvas.

## Task Parameters

| **Parameter** | **Description** |
| ------- | ---------- |
| Node Name | Set the name of the task. Node names within a workflow definition are unique. |
| Run flag | Indicates whether the node can be scheduled normally. If it is not necessary to execute, you can turn on the prohibiting execution switch. |
| Description | Describes the function of this node. |
| Task priority | When the number of worker threads is insufficient, they are executed in order from high to low according to the priority, and they are executed according to the first-in, first-out principle when the priority is the same. |
| Worker group | The task is assigned to the machines in the worker group for execution. If Default is selected, a worker machine will be randomly selected for execution. |
| Task group name | The group in Resources, if not configured, it will not be used. | 
| Environment Name | Configure the environment in which to run the script. |
| Number of failed retries | The number of times the task is resubmitted after failure. It supports drop-down and manual filling. | 
| Failure Retry Interval | The time interval for resubmitting the task if the task fails. It supports drop-down and manual filling. | 
| Cpu quota | Assign the specified CPU time quota to the task executed. Takes a percentage value. Default -1 means unlimited. For example, the full CPU load of one core is 100%,and that of 16 cores is 1600%. This function is controlled by [task.resource.limit.state](../../architecture/configuration.md). |
| Max memory | Assign the specified max memory to the task executed. Exceeding this limit will trigger oom to be killed and will not automatically retry. Takes an MB value. Default -1 means unlimited. This function is controlled by [task.resource.limit.state](../../architecture/configuration.md). |
| Timeout alarm | Check the timeout alarm and timeout failure. When the task exceeds the "timeout period", an alarm email will send and the task execution will fail. |
| Conda Env Name | Name of conda environment or packed conda environment tarball. |
|Input Note Path | Path of input jupyter note template. |
| Out Note Path | Path of output note. |
| Jupyter Parameters | Parameters in json format used for jupyter note parameterization. |
| Kernel | Jupyter notebook kernel. |
| Engine | Engine to evaluate jupyter notes. |
| Jupyter Execution Timeout | Timeout set for each jupyter notebook cell. |
| Jupyter Start Timeout | Timeout set for jupyter notebook kernel. |
| Others | Other command options for papermill. |

## Task Example

### Jupyter Task Example

This example illustrates how to create a jupyter task node.

![demo-jupyter-simple](../../../../img/tasks/demo/jupyter.png)
