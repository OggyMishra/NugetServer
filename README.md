NugetServer
===========

Nuget is Microsoft supported mechanism of code sharing. It defines the way how packages are created, hosted and consumed by other developers. This repostiory is to demonstrate the procedure of how to create an inhouse nuget server and host your local nuget packages on it.

## Getting Started
Hosting an in-house Nuget server can be very useful in terms of code-sharing. It's a great way to ensure that the build artifacts of each team are Nuget packages and that other teams are consuming those packages, rather than loose DLLS.

## Create a new package:
Nuget relies on *.nuspec file to create the nuget package.
### File Structure of *.nuspec:
- Metadata: It contains all the information about the package. This file has the following general form: 
![metadata] (https://github.com/OggyMishra/NugetServer/blob/master/docs/metadata.png)

- id: id is the unique identifier for a package. It's an case insensitive identifier. it should not contain any spaces and invalid character.
- version: Version of the package should follow [Major].[Minor].[Patch] pattern. You can also include sub-version as pre-release suffix. For eg. alpha, beta etc.
- authors: A comma separated list of package authors.
- owners: A comma separated list of package owner.
- licenseUrl: A url for the package license.
- projectUrl: A url for project url.
- iconUrl: URL of a image. This will provide the icon of the package.
- requireLicenseAcceptance: A boolean variable if you want your customer to accept the license before installing the package.
- description: Summary about the package.
- releaseNotes: Any release notes about the package
- copyright: Copyright info.
- tags: keywords that describe the package and help through search and filtering.

### Files
![files] (docs/files.png)

### Platform specific DLLS:
In few scenarios 'Any CPU' configuration doesn't work. So package owners have to ensure that they support different platform architecture. In order to support that, package owners have to create *.props and *.targets file. 
- In the *.props file, that paths are pointing to the folder structure of the target platform.
![platform] (docs/platform.png)

- *.target file:
![target] (docs/target.png)
### Packing
nuget provides 'pack' command to pack the dependencies.
`'nuget pack [ProjectName].nuspec'`
After executing this command, nuget will create a *.nupkg file. *.nupkg file is a compress file which contains all the metadata and dependencies of the package. *.nupkg file will be having this format: [ProjectName].[VERSION].nupkg, where the version is the same version specified in the *.nuspec file.

### Package deployment:
`nuget push [ProjectName].[VERSION].nupkg [API_KEY] -Source [NUGET_STORE]`

## Nuget Store:
Once you houst the nuget server on IIS and it's running at http://localhost:xxxx/nuget. Replace [NUGET_STORE] with this url while pushing the package.
- Follow these steps in order to consume the packages deployed on VANuget,
1.Open visual studio.
2.Goto Tools > Nuget Package Manager > PackageMangerSetting
3.Click Package Sources under 'Nuget Package Manager'
4.Click on '+' icon to add new package source.
5.Specify the name as VANuget and source as 'http://localhost:xxxx/nuget'
