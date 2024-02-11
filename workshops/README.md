# Introduction
@author: Yingding Wang\
@date: 23.02.2024\
@version: 1

This workshop introduces the basics regarding Kubeflow Notebook and Kubeflow Pipeline for manifests deployment 1.7.0.

## 1. Agenda of Kubeflow Workshop (Basics)

### Workbench (JupyterLab) Basics 
1. [Create, Monitor, Restart, Connect (Workbench: Jupyter Notebook)](./workbench1.md)
2. [Install, upgrade workbench python packages](./workbench2.md)
3. [Share Data Volume in workbenches](./workbench3.md)
4. [Git Versioning and Env variable](./workbench4.md)
5. [Working with python juypter notebook](./workbench5.md)

### Kubeflow Pipeline Basics
1. [Create Kubeflow Pipeline from Workbench](./pipeline1.md)
2. [Kubeflow Pipeline SDK basics](./pipeline2.md)
3. [Running further examples of Kubeflow Pipeline](./pipeline3.md)

## 2. Further Readings
* Kubeflow features and components: https://www.kubeflow.org/docs/components/
* Canonical Charmed Kubeflow examples: https://github.com/canonical/kubeflow-examples
* Kubeflow examples: https://github.com/kubeflow/examples
* Kubeflow pipeline v2 SDK reference: https://www.kubeflow.org/docs/components/pipelines/v1/sdk/build-pipeline/
* Q&A Kubeflow Community Slack: https://www.kubeflow.org/docs/about/community/

## 3. Attributions

My best thanks to **Andreea Munteanu** from Canonical and **[Ajinkya Bobade](https://github.com/ajinkya933)** from Kubeflow Community for sending me their internal workshop slides and examples. I have incorperated these valuable insights into the content of this workshop with hands-on tutorials.

My deepest thanks to **Prof. Dr. Michael Ingrisch** and **Dr. Balthasar Schachtner** for coordinating and facilitating this kubeflow workshop. Without their help, this workshop will not be possible. Also my thanks to **Simon Leutner** and **Thomas Kluge** for their support in the Kubeflow system operation and maintenance on-site.

Finally, my thanks to all the **Kubeflow maintainers**, **Kubeflow Community** members for creating this fantastic open-source system for making machine learning workflows on Kubernetes simple, portalbe and scalable.