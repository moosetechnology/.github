This repository contains configurations for the moosetechnology organization.

# About Workflows
This repository contains starter workflows that can be used to as templates in repositories in the organization.
We also provide reusable workflows that can be called by another workflow.

## Reusable workflows
Reusable workflows are workflows that are called by other workflows.
Their goal is to hold the testing behavior that is common accross moosetechnology repositories.
Bug report and feature requests about reusable workflows should be expressed by an issue to this repository.

## Starter workflows
Starter workflows are templates that are available when creating a new workflow in a moosetechnology repository. 
They can be found in the `Action` tab of your repo, when clicking the `New workflow` button.

![image](https://github.com/moosetechnology/.github/assets/39184695/fafcd534-e9de-46c6-9aed-4043b7e5113f)

Once you have chosen a template, you can modify it to fit the needs of your repository.

### Run tests
The [run-tests.yml](workflow-templates/tests.yml) starter workflow is the most frequently used.
It calls the [run-tests.yml](.github/workflows/run-tests.yml).
The options available are detailed in the workflow file.

### Run tests and update a release
The [run-tests.yml](workflow-templates/test-and-release.yml) starter workflow is to be used when you need to update a release with the generated images.
It calls the [run-tests.yml](.github/workflows/test-and-release.yml).
The options available are detailed in the workflow file.

### About options
The starter workflows can call reusable workflows with several options.

#### Available options

| Option            | Type             | Availability                              | Default value                     |
| ----------------- | ---------------- | ----------------------------------------- | --------------------------------- |
| pharo-versions    | Array of strings | All                                       | Pharo versions for latest Moose   |
| create-artifact   | Boolean          | run-tests.yml                             | false                             |
| image-name        | String           | All (useless if create-artifact is false) | \<RepositoryName\>-\<branchName\> |
| pre-ulpoad-script | String           | All (useless if create-artifact is false) | ''                                |
| release-tag       | String           | update-and-release.yml                    | 'generated-assets'                |

- **pharo-version**: The pharo versions on which the tests will run.
See available images [here](https://github.com/hpi-swa/smalltalkCI?tab=readme-ov-file#images).

- **create-artifact**: A boolean.
If `true`, an artifact containing all the files necessary to run the image locally will be generated.
Note that smalltalkCI will be deleted from this image.

- **image-name**:
The first part of the name of the image generated and the file containing it.
The full name will be <image-name>-<pharo-version>.

- **pre-upload-script**:
A Pharo script that will be executed in the image after the tests are run and before uploading the artifact.
It is mostly used to register information from the workflow run. For example, store a commit ID.
If you do not need workflow information, you can instead use smalltalkCI custom scripts: https://github.com/hpi-swa/smalltalkCI?tab=readme-ov-file#custom-scripts

- **release-tag**:
The tag of the release to update.
