# Git Versioning and Env variable

## 1 Create a test notebook
Let's create a test notebook

1. Navigate to "Kubeflow UI Dashboard" -> "Notebooks" -> "+ New Notebook"

2. Input the following values in the `New notebook` plane

| Basic Category | Input |
|:--- | :--- |
| Name: | test |
| Type: | JupyterLab |
| Custom Notebook: | kubeflownotebookswg/jupyter-scipy:v1.7.0 |
| CPU: | 0,2 |
| RAM in GiB: | 0,5 |

| Workspace Volumes | Input |
|:--- | :--- |
| New volume | |
| Type | Empty volume |
| Size in Gi | 5 |
| Storage class | homedir |
| Access mode | ReadWriteOnce |
| Mount path | /home/jovyan |

3. Click on `LAUNCH` button to create a new jupyterlab workbench.

## 2 Download workshop git repository

1. Let's `CONNECT` to the `test` notebook
2. Open a terminal in juypterlab and type:
```shell
git clone https://github.com/yingding/kf-examples
```
3. Now you shall see the workshop tutorial and code repository appears in the jupyterlab brower

![](./images/workbench4_git_clone.png)

## 3 Apply PodDefault CRD into your namespace

PodDefault is a Kubeflow CRD (Custom Resource Definition) helps to inject env, vars, volumes into pod in kubernetes.

Reference:
* PodDefault:  https://github.com/kubeflow/kubeflow/blob/master/components/admission-webhook/README.md

1. Open the terminal in JupyterLab, and execute:
```shell

```

