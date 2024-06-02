# ADO_Publish_Artifact.ado

Use this task in a build pipeline to publish build artifacts to Azure Pipelines, TFS, or a file share.

* Note: TFS is now known as Azure DevOps

## Parameters

The pipeline requires the following parameters to be defined:

| Name  | Displayname | type | Default | Values | Opional/Required | Comments |
| ------------- | ------------- | :-------------: | :-------------: | ------------- | :-------------: | ------------- |
| PathtoPublish | Path to the Artifact | string | '$(Build.ArtifactStagingDirectory)' | | Required | |
| ArtifactName | Name of Artifact to be published | string | | | Required | Specifies the name of the artifact to create in the publish location. The following special characters are not allowed: +, %, {, } |
| publishLocation  | Location where to publish artifact | string | 'Container' | 'Container'/'FilePath' | Optional | **Alias: ArtifactType**, Specifies whether to store the artifact in Azure Pipelines (Container), or to copy it to a file share (FilePath) that must be accessible from the build agent. |
| MaxArtifactSize | Max Artifact Size  | string | '0' |  | Optional | Put 0 if you don't want to set any limit. |
| TargetPath  | Target path in File Share. | string |  | | Optional |  Required when ArtifactType = FilePath, The path must be a fully qualified path or a valid path relative to the root directory of your repository. Publishing artifacts from a Linux or macOS agent to a file share is not supported. |
| Parallel | Condition for a Parallel copy | boolean | false | true/false | Optional | Required when ArtifactType = FilePath, whether to copy files in parallel using multiple threads for greater potential throughput. If this setting is not enabled, a single thread will be used.  |
| ParallelCount | Count of Parallel Copy | string |'8'|  | Optional | when ArtifactType = FilePath && Parallel = true, egree of parallelism (the number of threads) used to perform the copy. The value must be at least 1 and not greater than 128. Choose a value based on CPU capabilities of the build agent. |
| StoreAsTar | Condition to Tar the artifact before uploading | boolean | false | true/false | Optional | Adds all files from the publish path to a tar archive before uploading. This allows you to preserve the UNIX file permissions. |
--------------------------------------------------------------------------------------------------------------------------------------------------

These parameters provide multiple use case options for the template, enable/disable flags for the utilization of different templates as per the requirements.

## Use Cases

You can directly call a particular template as per the requirement. for example:

  ```yaml
  # azure-pipeline.yml
  resources:
  repositories:
    - repository: Template
      type: github
      name: your_username/AzureDevopsPublishArtifact
      ref: <respective branch name>
      endpoint: 'githubServiceConnectioNname'

  steps:
  # passing the parameters
  - template: ADO_Download_Package.yml@Template
    parameters:
      pathToPublish: $(Build.ArtifactStagingDirectory)
      artifactName: MyBuildOutputs

```

* Note: Make sure to adjust the repository name, branch name, and parameter values according to your project's requirements.
