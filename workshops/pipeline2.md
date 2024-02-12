# Kubeflow Pipeline SDK basics

## 1. Kubeflow Pipeline components

In the Kubeflow Pipeline SDK v1, one way to run a ML tasks is to use a `Python function-based component`.

As its name suggested, you can define a Python function, and executed it in a base container python image.

Let's `CONNECT` to the `pipeline-test` JupyterLab workbench, and open the `kf-examples/sdkV1/toy_v1_add.ipynb` file in JupyterLab file browser.

**Note:**
* If you deleted `pipeline-test` workbench, please visit the [prevous tuturial seciton](./pipeline1.md) to recreate the workbench again and come back to continue.

In the `toy_v1_add.ipynb`, you can find the cell block
```python
from functools import partial
from kfp.components import create_component_from_func

'''
The kfp.components.create_component_from_func() create a ContainerOp obj, which is used in pipeline
'''
@partial(
    create_component_from_func,
    output_component_file=f"{pipeline_path_dir}/square_component.yaml",
    base_image='python:3.10.13-bullseye', # a base image with python installed
)
def add_op(a: float, b: float) -> float:
    '''Calculates sum of two arguments'''
    return float(a + b)
```

This cell is calling from `kfp.components.create_component_from_func` method to create a ML task for you kubeflow pipeline.

The decorator `functools.partial` is just a convenient way for you to define a python function to be executed, and this python function will run as a part of your pipeline.

In this example, the python function-based component called `add_op` takes in two python parameters `a` and `b` can sums the two float input and return the sum as a float number.

It is as simple as that to create a Kubeflow pipeline component with python function-based component by just write a python function and adding the decorator to this user defined function.

The decorator part 
```
@partial(
    create_component_from_func,
    output_component_file=f"{pipeline_path_dir}/square_component.yaml",
    base_image='python:3.10.13-bullseye', # a base image with python installed
)
def add_op():
    ...
```
defines parameters
* `create_component_from_func`, always stay in this way
* `ouput_component_file` : defines the path/location, where the compiled python function-based component Yaml file shall be saved.
* `base_image`: defines the python base OCI image, you want to use to run this python function. You can choose any OCI image from `dockerhub` as long as python is installed in this base image.


Please visit the official documents to learn more about `python function-based components` at https://www.kubeflow.org/docs/components/pipelines/v1/sdk/python-function-components/


## 2. Kubeflow Pipeline DAG

As you have defined your first Kubeflow component as  `python function-based` component, you now want to put it in a sequential order to be executed in a pipeline, it is called Kubeflow Pipeline DAG (Directed Acyclic Graph).

In the `kf-examples/sdkV1/toy_v1_add.ipynb` file, the following cell defines the pipeline DAG.

```python
from kfp.dsl import pipeline
import kfp.dsl as dsl

EXPERIMENT_NAME = "demo"
EXPERIMENT_DESC = 'toy sum pipeline'

@pipeline(
    name = EXPERIMENT_NAME,
    description = EXPERIMENT_DESC
)
def sum(a: float, b: float):
    '''local variable'''
    no_artifact_cache = "P0D"
    artifact_cache_today = "P1D"
    # cache_setting = artifact_cache_today
    cache_setting = no_artifact_cache
    
    first_add_task = add_op(a=a, b=b)
    # if the limit is set to small, will be OOMKilled by kubernetes
    first_add_task  = set_res_limit(task=first_add_task , cpu_req='500m', mem_req='200M')
    first_add_task.execution_options.caching_strategy.max_cache_staleness = cache_setting
```

You can use `kfp.dsl.pipeline` decorator to define a Kubeflow Pipeline, the `name` and `description` parameters are optional.

The `def sum(a: float, b: float):` python function defines your pipeline DAG. In this example, the `a` and `b` are the pipeline args, which can be dynamically inputed during the pipeline run creating for user to input different statical values to change the run behaviour of a kubeflow pipeline.

You `python function-based` component names `add_op` can be called with its input parameters. The kubeflow component itself is also a factory function, it takes in the parameter from pipeline, and sum it up, the return of `add_op` is a `ContainerOp`. You can think of `ContainerOp` as a representation of Pod object in Kubernetes.

You need to set the resource requirements and limits for the `ContainerOp` with the helper function `set_res_limit`, since your namespace have quota limit activated.

Using the `.execution_options.caching_strategy.max_cache_staleness` variable of the `ContainerOp`, you can define whether the ML task need to be executed everytime if its input variable doesn't change, or the ML task shall use the prevous cached result without executed the ML task pod.

You can find more details to Kubeflow Pipeline DAG from the following references:
* Intro Kubeflow pipeline: https://www.kubeflow.org/docs/components/pipelines/v1/introduction/
* Kubeflow pipeline SDK v1: https://www.kubeflow.org/docs/components/pipelines/v1/sdk/sdk-overview/
* Pipeline v1 caching https://www.kubeflow.org/docs/components/pipelines/v1/overview/caching/


## 3. Share Kubeflow Pipeline

As you can define a Kubeflow ML pipeline from Python SDK, you can also compile the Kubeflow v1 pipeline to a Yaml format for the underlying run engine.

To compile a Kubeflow pyython pipeline object, please see the following code snippet from the `kf-examples/sdkV1/toy_v1_add.ipynb` file

```python
from kfp import compiler

PIPE_LINE_NAME="sum"
PIPE_LINE_FILE_NAME=f"{pipeline_path_dir}/{PIPE_LINE_NAME}"
compiler.Compiler().compile(my_pipeline, f"{PIPE_LINE_FILE_NAME}.yaml")
```

You can use the `kfp.compiler` class from the `kfp` python SDK.
Use `compiler.Compiler().compile()` method to compile a python pipeline object to its serialized Yaml text representation.

You can then share this `.yaml` pipeline with others to run in different Kubeflow environment. This makes the kubeflow pipeline portable and sharable. You can also upload your compiled pipeline to the `Kubeflow Dashboard UI` from the `Pipelines` section and start `run` from this pipeline version.

## 4. Start pipeline run from kubeflow pipeline SDK

You can also start a pipeline `run` with its pipeline input parameters from the python pipeline object directly using the Kubeflow pipeline python SDK.

```python
import kfp
client = kfp.Client()
run = client.create_run_from_pipeline_func(
    pipeline_func=my_pipeline,
    arguments = pipeline_args,
    run_name = RUN_NAME,
    pipeline_conf=pipeline_config,
    experiment_name=EXPERIMENT_NAME,
    namespace=NAMESPACE,
)
```

You shall get the client `kfp.Client()` object, and call `client.create_run_from_pipeline_func` to create a `Run` from `kfp` python sdk.

Recall that `Run` is the parameterized execution of your kubeflow ML pipeline. (https://www.kubeflow.org/docs/components/pipelines/v2/run-a-pipeline/)

## 4. (optional) Multiple ML tasks in a pipeline DAG

Please execute the `kf-examples/sdkV1/toy_v1_pythagorean.ipynb` in your JupyterLab workbench and following this python notebook example on your own pace to study how you can use multiple tasks in a Kubeflow pipeline DAG.

### 5. Summary

You have learned:
* Kubeflow function-based component
* Create a Kubeflow pipeline from Python SDK
* Compile and share Kubeflow pipeline from Python SDK
* Run Kubeflow pipeline from Python SDK