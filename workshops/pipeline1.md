# Create Kubeflow Pipeline from Workbench

A Kubeflow pipeline is a **portable** and **scalable** definition of a machine learning (ML) workflow. Each step in your ML workflow, such as preparing data or training a model, is an instance of a pipeline component.

## 1. Create a Workbench with access to Kubeflow

A Kubeflow pipeline can be created with [Kubeflow Python SDK](https://pypi.org/project/kfp/) using its own DSL (domain specific language). Currently, the Kubeflow Pipeline SDK (kfp) is only available in Python programming language.

### Add `Allow access to Kubeflow Pipelines` PodDefault
Let's create a Jupyter Notebook first to interact with the Kubeflow Pipeline API Server backend.

1. Navigate to "Kubeflow UI Dashboard" -> "Notebooks" -> "+ New Notebook"

2. Input the following values in the `New notebook` plane

| Basic Category | Input |
|:--- | :--- |
| Name: | pipeline-test |
| Type: | JupyterLab |
| Custom Notebook: | kubeflownotebookswg/jupyter-scipy:v1.7.0 |
| CPU: | 0,2 |
| RAM in GiB: | 0,5 |

| Workspace Volume | Input |
|:--- | :--- |
| New volume | |
| Type | Empty volume |
| Size in Gi | 5 |
| Storage class | homedir |
| Access mode | ReadWriteOnce |
| Mount path | /home/jovyan |

3. Open the Advanced Options

![](./images/workbench4_advanced_notebook_options.png)

and select the Configurations
* Allow access to Kubeflow Pipelines

![](./images/pipeline1_select_kfp_poddefault.png)

4. Click on `LAUNCH` button to create a new jupyterlab workbench.


Notice:
* If you haven't created `Allow access to Kubeflow Pipelines` PodDefault in your namespace, please follow the instruction in Section [Git Versioning and Env variable](./workbench4.md) to create `Allow access to Kubeflow Pipelines` PodDefault first, and then back to this tutorial section.

## 2. Create first kubeflow pipeline

Let's `CONNECT` to the created JupyterLab workbench `pipeline-test` with the kubeflow access token mounted.

You can create a Python Jupyter Notebook from the `Launcher` by click on the Notebook `Python 3`.

In this tutorial, you will start with a provided `notebook` to start your first pipeline.

1. Fetch the workshop git code repository by typing the following in terminal in the `pipeline-test` JupyterLab workbench:
```shell
cd $HOME;
git clone https://github.com/yingding/kf-examples;
```

2. Navigate the `kf-examples/sdkV1/`