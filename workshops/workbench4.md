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
MY_NAMESPACE=<your-namespace>;
echo ${MY_NAMESPACE}
```
change the `MY_NAMESPCE` bash variable to your real namespace, and double check the output.

If your namespace are set, run the following command in terminal to apply the PodDefault examples for further tutorial sessions.
```shell
kubectl -n ${MY_NAMESPACE} apply -f $HOME/kf-examples/workshops/configs/my-env-poddefault.yaml
kubectl -n ${MY_NAMESPACE} apply -f $HOME/kf-examples/workshops/configs/ml-pipeline-poddefault.yaml
```

You shall see the console output
```shell
poddefault.kubeflow.org/my-env-secret created
```

If you see an error 
```shell
Error from server (Forbidden): error when applying patch: ...
```
DON'T panic, it means the PodDefault has already been created and exists in your namespace, you can proceed safely and ignore this error.

## 4 Re-create the test notebook and attach PodDefaults
Let's `stop` and `delete` the `test` notebook, but leave the workspace volume untouched.

Create the `test` notebook again, attach the existing workspace volume and PodDefaults:

1. Navigate to "Kubeflow UI Dashboard" -> "Notebooks" -> "+ New Notebook"

2. Input the following values in the `New notebook` plane

| Basic Category | Input |
|:--- | :--- |
| Name: | test |
| Type: | JupyterLab |
| Custom Notebook: | kubeflownotebookswg/jupyter-scipy:v1.7.0 |
| CPU: | 0,2 |
| RAM in GiB: | 0,5 |

3. "Delete" the "new volume"

![](./images/workbench4_delete_new_workspace_volume.png)

4. Then you will see the option of "+ Attach exisiting volume", click on it.

![](./images/workbench4_attach_existing_workspace.png)

and input the following values:

| Existing volume | Input |
|:--- | :--- |
| Type: | Kubernetes Volume |
| Readonly | <left uncheck, since we want to use the volume as workspace to write> |
| Name: | test-volume |
| Mount path: | /home/jovyan |

![](./images/workbench4_choose_existing_workspace.png)

5. Open the Advanced Options

![](./images/workbench4_advanced_notebook_options.png)

and select the two Configurations
* Allow access to Kubeflow Pipelines
* add-my-env-secret

![](./images/workbench4_select_poddefaults.png)

Note:
* the select Configuraitons are the `spec.desc` in the two PodDefault file you have been applied to your namespace.
* Please revisit the PodDefault files in `KF-examples/workshops/configs` folder to see the content.

6. Click on `LAUNCH` button to create a new jupyterlab workbench.

## 5. View the mounted token partition and ENV variables in test notebook

Let's `CONNECT` to our recreated `test` notebook

1. You shall see that our tutorial git repository is in the recreated notebook, since you attached the workspace volume prevously, so you still have the data inside the prevous workspace volume.

2. Let's open a terminal




