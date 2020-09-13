# Azure DevOps Pipelines
A working example how to extend a pipeline for simplified "Bootstrapping" Azure Pipelines.

## Usage
The base `azure-pipelines.yaml` file needs to be included in the root of a repo, with AzDO pointed to this file when setting up your pipeline.

The `type` parameter is used to find the next pipeline to run, for instance a type of `dotnet` will extend onto the `azure-pipelines-dotnet.yaml` pipeline definition. 

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
