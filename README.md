# About Workflows

###### Reusable workflows
This repository contains reusable workflows that can be called by other workflows.
They hold the common testing behavior needed accross most moosetechnology repositories.
Feature requests and bug reports about reusable workflows should be expressed by an issue to this repository.

###### Starter workflows 
This repository also contains starter workflows that can be used to as templates for coninous integration in moosetechnology repositories.
They call the reusable workflows of this repository to avoid code duplication.
The default behavior can be modified using the options detailed below.

The starter workflows can be found when creating a new workflow in any moosetechnology repository: in the `Action` tab, when clicking the `New workflow` button.

![image](https://github.com/moosetechnology/.github/assets/39184695/fafcd534-e9de-46c6-9aed-4043b7e5113f)
## Available starter workflows

#### Run tests
The [run-tests.yml](workflow-templates/tests.yml) starter workflow is the most frequently used.
The default behavior is simple:
1. Checkout the tested branch
2. Load project
3. Run tests

The `create-artifact` option triggers the generation of an artifact containing all the files necessary to run the image locally will be generated.
See [Options](#Options).

#### Run tests and update a release
The [run-tests.yml](workflow-templates/test-and-release.yml) starter workflow is to be used when you need to update a release with the generated images.
It runs the tests in the same manner as run-tests.yml.
If the tests succeed, an artifact is created and uploaded as release assets.

### Options
The starter workflows can call reusable workflows with several options.

#### Available options

| Option            | Type             | Availability                              | Default value                     |
| ----------------- | ---------------- | ----------------------------------------- | --------------------------------- |
| pharo-versions    | Array of strings | All                                       | Pharo versions for latest Moose   |
| create-artifact   | Boolean          | run-tests.yml                             | false                             |
| image-name        | String           | All (useless if create-artifact is false) | \<RepositoryName\>-\<branchName\> |
| pre-upload-script | String           | All (useless if create-artifact is false) | ''                                |
| release-tag       | String           | update-and-release.yml                    | 'generated-assets'                |

- **pharo-version**: The pharo versions on which the tests will run.
See available images [here](https://github.com/hpi-swa/smalltalkCI?tab=readme-ov-file#images).

- **create-artifact**: A boolean.
If `true`, an artifact containing all the files necessary to run the image locally will be generated.
Note that smalltalkCI will be deleted from this image.

- **image-name**:
The first part of the name of the image generated and the file containing it.
The full name will be \<image-name\>-\<pharo-version\>.

- **pre-upload-script**:
A Pharo script that will be executed in the image after the tests are run and before uploading the artifact.
It is mostly used to register information from the workflow run, for example store a commit ID.
If you do not need workflow information, you can instead use [smalltalkCI custom scripts](https://github.com/hpi-swa/smalltalkCI?tab=readme-ov-file#custom-scripts)

- **release-tag**:
The tag of the release to update.

#### Default values at repository scope

Default values for the options are set at the organization level.
They can be overriden at a repository scope.

##### Use the same value for all branches in the repository
To override a default value for all branches in a repository, set the value as input for the reusable workflows when configuring the starter workflow.
Example, to use the latest stable Pharo version: 

```YAML
jobs:
  run:
    uses: moosetechnology/.github/.github/workflows/run-tests.yml@main
      with:
        pharo-versions: [ Pharo64-stable ]
```
##### Use different values for specfic branches
[TODO]
