# Get to know a dataset - Jupyter Notebook Tutorial Template

This directory contains a template for participants of the [Open Data Sponsorship Program](https://opendata.aws/) to use when creating their dataset's introductory tutorial.

## Directions
1. Download a copy of `get-to-know-a-dataset-template.ipynb`. (We recommend NOT forking / cloning this repository unless your intention is to propose changes to the template.)

2. Open / edit the notebook using the Jupyter Notebook environment of your choosing. [Amazon SageMaker Studio](https://aws.amazon.com/sagemaker-ai/studio/) offers a single web-based interface for end-to-end ML development. You may also request a free account on [Amazon SageMaker Studio Lab](https://studiolab.sagemaker.aws/). Amazon SageMaker Studio Lab is a notebook studio in the cloud, offering both CPU and GPU access with your free account.

   You may also choose to work on your personal computer using [JupyterLab](https://jupyter.org/) or [Microsoft Visual Studio Code](https://code.visualstudio.com/) and the [Python](https://www.python.org/) distribution of your choice.

3. Answer the questions / complete the directives in the template notebook using both your narrative, written text as well as code examples where possible. Replace or adapt sample text / code as needed. Clicking into text blocks will typically reveal additional instructions and contextual guidance.

4. **(IMPORTANT!)** Be sure to clear all your notebook's outputs before sharing. You can do this using the `nbconvert` tool:

```sh
jupyter nbconvert --clear-output --inplace YOUR-NOTEBOOK.ipynb
```

5. Upload your notebook to your Github repository. We strongly recommend creating a Github repository to house notebooks and other documentation related to your AWS-sponsored dataset(s). This provides a convenient mechanism for users to provide feedback and suggest changes to tutorials, documentation, or other aspects of your dataset.

6. Make sure to add this notebook to the list of tutorials in your dataset's YAML metadata entry on the [Registry of Open Data on AWS](https://github.com/awslabs/open-data-registry/). This tutorial should be the first tutorial entry and should follow the naming convention "Get To Know A Dataset - DATASETNAME". For example:

```yaml
DataAtWork:
  Tutorials:
    - Title: Get To Know A Dataset - DATASETNAME
      URL: https://github.com/EXAMPLE/MYDATASET-DOCS/get-to-know-a-dataset-MYDATASET.ipynb
      NotebookURL: https://github.com/EXAMPLE/MYDATASET-DOCS/get-to-know-a-dataset-MYDATASET.ipynb
      AuthorName: AUTHOR NAME
      AuthorURL: AUTHOR URL
```

7. Please reach out to us opendata\[at\]amazon.com if you get stuck along the way.
