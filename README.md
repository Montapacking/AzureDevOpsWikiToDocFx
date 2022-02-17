An Azure DevOps pipeline task to turn your [Azure DevOps wiki](https://docs.microsoft.com/en-us/azure/devops/project/wiki/wiki-create-repo) into an [DocFX website](https://dotnet.github.io/docfx/).

This allows to to create a public documentation website with the nice wiki editing tools of Azure DevOps. 

# Supports

- Links between pages
- Images
- Mermaid diagrams

# Does not support

- Running in a subdirectory: the resulting site must run on its own domain. This is because we don't touch the link in the wiki files which are absolute.

# Template

To use a certain docfx template, place the template files in a directory named ".docfx_template" in your wiki repository.

# Hiding content

To hide content, surround it with "::: private" and ":::". E.g.:

```
Content publicly visible in the DocFX website.

::: private
This will not be visible in the DocFX website.
::: 

This will be visible again. 
```

# Usage

With a azure-pipelines.yml build file below, a artifact will be created which you can release to a webserver.

```
trigger:
- main

pool: 
  vmImage: 'windows-latest'

steps:
- task: AzureDevOpsWikiToDocFx-dev@2
  inputs:
    SourceFolder: '$(System.DefaultWorkingDirectory)'
    TargetFolder: '$(System.DefaultWorkingDirectory)/docfx'
- task: DocFxTask@0
  inputs:
    solution: 'docfx/docfx.json'
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/docfx/_site'
    ArtifactName: 'drop'
    publishLocation: 'Container'
```
