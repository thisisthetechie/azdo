# Azure DevOps Pipelines
A working example how to extend a pipeline for simplified "Bootstrapping" Azure Pipelines.

## Usage
The base `azure-pipelines.yaml` file needs to be included in the root of a repo, with AzDO pointed to this file when setting up your pipeline.

The `type` parameter is used to find the next pipeline to run, for instance a type of `dotnet` will extend onto the `azure-pipelines-dotnet.yaml` pipeline definition. The _template_ property of the `extends` function shows you how that works. The name of the yaml file is requested from the `pipelines` named respository in the `resources` section.

## Setting up named repository
The tricky part of getting this working is setting up your named repository. For this to work you need to have your information handy about where your source code is located. The first step is to create a `Service Connection` to your repo in the Project Settings. You will set up a name for your connection, this will be your `endpoint` in the YAML Definition. 

The `repository` definition is what you will use to refer to the resource, for instance `template: mypipeline.yaml@[repository]`. Here it's simple called "pipelines".

The `name` of the resource is actual name of the repository.

```yaml
# Get the Azure Pipelines templates from Bitbucket
resources:
  repositories:
  - repository: pipelines
    type: github
    name: thisisthetechie/azdo
    endpoint: github
```

## Additional Steps
It is possible to add additional steps into your build pipeline from the base `azure-pipelines.yaml` definition that is stored within the application repo. To achieve this, add new parameters to the `extends.parameters` section to pass in full build step definitions as parameters.

For instance, this example will include an echo command after a _restore_ step but before a _build_ step:

```yaml
# Run the Azure Pipeline template (as defined by the ProjectType)
extends:
  template: azure-pipelines-${{ parameters.ProjectType }}.yaml@pipelines
  parameters:
    BuildNo: ${{ parameters.MajorBuild }}.${{ parameters.MinorBuild }}.$(Build.BuildNumber)
    BuildConfiguration: ${{ parameters.BuildConfiguration }}
    PreBuildSteps:
      - task: Echo a comment
        script: echo "I am running"
```  
