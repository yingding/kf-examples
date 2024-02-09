# Kubeflow Notebook/Workbench: Create, Monitor, Restart, Connect

## 1. Create a Kubeflow Jupyter Notebook

Notebooks -> "Choose a Namespace" -> 
"+ New Notebook"  to create a `Notebook/Workbench` in Kubeflow Web UI.

![create workbench](./images/workbench1_choose_namespace.png)

| Basic Category | Input |
|:--- | :--- |
| Name: | test |
| Type: | JupyterLab |
| Custom Notebook: | kubeflownotebookswg/jupyter-scipy:v1.7.0 |
| CPU: | 0,2 |
| RAM in GiB: | 0,5 |

| Data Volumes | Input |
|:--- | :--- |
| New volume | |
| Type | Empty volume |
| Size in Gi | 5 |
| Storage class | homedir |
| Access mode | ReadWriteOnce |
| Mount path | /home/jovyan |

Note: 
* Do NOT change the mount path "/home/jovyan"

Click on "LAUNCH" to create a `Notebook Server / Workbench`

![Connect Workbench](./images/workbench1_connect_1.png)

You will see a `Notebook/Workbench'` is created in your namespace, the status shows a wheel spinning.

## 2. Monitor the Workbench events

It is possible to see the events during the creation of `Notebook/Workbench`.

... (TODO) make screenshots

## 3. Restart the Workbench

After the first creation of workbench, the workbench server will be started automatically.

You can stop the Workbench by click on the `stop` button at the end of the workbench.
![stop workbench](./images/workbench1_stop.png)

To restart then click on the `start` button at teh end of the workbench.
![start workbench](./images/workbench1_start.png)

Through `stop` and `start`, you can restart a workbench.

## 4. Connect the Workbench 

After the workbench is created successfully, you will see a check mark before the workbench.

![workbench successfully created](./images/workbench1_created_successfully.png)

Click on `CONNECT` to connect with the workbench, you will see a new tab opens up.

Note:
* Sometimes you may not connect with workbench due to session and networking issues successfully

Workaround:
* If you still can not connect with workbench after 1 minute, close the `opened workbench tab` and click on `CONNECT` again