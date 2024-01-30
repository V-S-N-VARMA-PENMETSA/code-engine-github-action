# Code Engine GitHub Action

This GitHub Action allows you to interact with IBM Cloud Code Engine. Deploy Apps, Jobs, and Functions. It offers flexibility for different deployment types and provides various configuration options.

## Inputs

| Name            | Required | Default Value |Description |
|-----------------|----------|---------------|----------------------------------------------------------------|
| `api-key`       | ✅      | -             | IAM API Key used to log into the IBM Cloud. Please store your IBM Cloud API key securely in your GitHub repository Secrets.|
| `resource-group`| ❌       | Default       | An IBM Cloud Resource Group, a logical container for organizing and managing related cloud resources.|
| `region`        | ✅      | -             | The geographical area where your Code Engine project is located, like `eu-de` |
| `project`       | ✅      | -             | The unique identifier (GUID) or the name that identifies your IBM Cloud Code Engine project. |
| `component`        | ✅      | -             | The type of component to deploy. allowed values `app`, `function`, `job` |
| `name`          | ✅      | -             | The name of the App, Function, or Job.|
| `runtime`       | ❌ | -             | The runtime used for the Function. Currently supported `nodejs-18` and `python-3.11` see [IBM Code Engine Function Runtimes](https://cloud.ibm.com/docs/codeengine?topic=codeengine-fun-runtime) for more information.|
| `build-source`  | ❌       | .             | Path to the directory containing the source code.|
| `cpu`           | ❌       | -             | CPU value set for your entity [Config for Functions](https://cloud.ibm.com/docs/codeengine?topic=codeengine-fun-runtime), [Codeengine Memory CPU combo](https://cloud.ibm.com/docs/codeengine?topic=codeengine-mem-cpu-combo)|
| `memory`        | ❌       | -             | Memory value set for your entity [Config for Functions](https://cloud.ibm.com/docs/codeengine?topic=codeengine-fun-runtime), [Codeengine Memory CPU combo](https://cloud.ibm.com/docs/codeengine?topic=codeengine-mem-cpu-combo)|



## Usage and Example

To use this action, add it to your GitHub Actions workflow YAML file also make sure to add your IBM Cloud API Key as GitHub Action Repository Secret. There is a example for deploying an App, Job and Python/Nodejs Function.

*Deploy an App: `deploy-app.yml`*: Deploy your app to `Default` resource-group in `eu-de` to the project `MY-PROJECT` with its source code in the root of the repository the name of the app is`my-app`.
```yaml
name: Deploy App to Code Engine

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:

  deploy-app:
    runs-on: ubuntu-latest
    steps:
    - name: Check out code
      uses: actions/checkout@v3

    - name: Deploy Application to Code Engine
      uses: IBM/code-engine-github-action@v1
      with:
        api-key: ${{ secrets.IBM_IAM_API_KEY }}
        resource-group: 'Default'
        region: 'eu-de'
        project: 'MY-PROJECT'
        entity: 'app'
        name: 'my-app'
        build-source: './'
        cpu: 1
        memory: 4G
```

*Deploy a Job: `deploy-job.yml`*: Deploy your Job to `Default` resource-group in `eu-de` to the project `MY-PROJECT` with its source code in the root of the repository the name of the job is`my-job`.
```yaml
name: Deploy App to Code Engine

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:

  deploy-job:
    runs-on: ubuntu-latest
    steps:
    - name: Check out code
      uses: actions/checkout@v3

    - name: Deploy Job to Code Engine
      uses: IBM/code-engine-github-action@v1
      with:
        api-key: ${{ secrets.IBM_IAM_API_KEY }}
        resource-group: 'Default'
        region: 'eu-de'
        project: 'MY-PROJECT'
        entity: 'job'
        name: 'my-job'
        build-source: './'
        cpu: 1
        memory: 4G
```

*Deploy a NodeJS Function: `deploy-nodejs-func.yml`*: Deploy your NodeJS Function to `Default` resource-group in `eu-de` to the project `MY-PROJECT` with its source code in the root of the repository the name of the function is`my-js-fn`.
```yaml
name: Deploy a NodeJS Function to Code Engine

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:

  deploy-nodejs-junc:
    runs-on: ubuntu-latest
    steps:
    - name: Check out code
      uses: actions/checkout@v3

    - name: Deploy JavaScript Function to Code Engine
      uses: IBM/code-engine-github-action@v1
      with:
        api-key: ${{ secrets.IBM_IAM_API_KEY }}
        resource-group: 'Default'
        region: 'eu-de'
        project: 'MY-PROJECT'
        entity: 'fn'
        runtime: nodejs-18 
        name: 'my-js-fn'
        build-source: './'
        cpu: 1
        memory: 4G
```

*Deploy a Python Function: `deploy-python-func.yml`*: Deploy your Python Function to `Default` resource-group in `eu-de` to the project `MY-PROJECT` with its source code in the root of the repository the name of the function is`my-py-fn`.
```yaml
name: Deploy a Python Function to Code Engine

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:

  fn-py:
    runs-on: ubuntu-latest
    steps:
    - name: Check out code
      uses: actions/checkout@v3

    - name: Deploy Python Function to Code Engine
      uses: IBM/code-engine-github-action@v1
      with:
        api-key: ${{ secrets.IBM_IAM_API_KEY }}
        resource-group: 'Default'
        region: 'eu-de'
        project: 'MY-PROJECT'
        entity: 'fn'
        runtime: python-3.11
        name: 'my-py-fn'
        build-source: './'
        cpu: 1
        memory: 4G
```