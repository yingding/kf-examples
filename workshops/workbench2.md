# Install, upgrade workbench python packages

## 1. Terminal basics
### Open Terminal

1. Click on the `+` Button to open `Laucher`
2. Click on the `Terminal` button on the `Laucher` plane
3. A new `terminal` tab will be open close to the `Laucher` tab
4. Click the `terminal` tab to interact with terminal

![Open Terminal](./images/workbench2_open_terminal.png)


### Show Quotas
In the terminal type:
```shell
kubectl describe quota
```
to see the information regarding your quota in Kubeflow.

## 2. Upgrade Jupyterlab Version
1. Click on `Help` menu button
2. Click on `About JupyterLab` to see the current JupyterLab version

![show jupyterlab version](./images/workbench2_jupyterlab_version.png)

The current `juyterlab` version is `3.4.3` from the default Kubeflow Jupyternotebook Image.

Lets upgrade it to version `3.6.7`

In our opened terminal, you shall see the `base` conda env is activated. 
Lets type in:
```shell
# show python path
which python3;
# You shall see /opt/conda/bin/python3

# upgrade jupyterlab version
# we also need to upgrade the kfp package, due to some old dependency issues
python3 -m pip install --upgrade pip;
python3 -m pip install --user --upgrade jupyterlab==3.6.7 kfp==1.8.22;
```

Now close the `Workbench/Jupyter Notebook` Tab and restart the workbenchserver



## 3. Install and Upgrade Python packages


