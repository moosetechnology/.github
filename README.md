# About Workflows

###### Reusable workflows
This repository contains reusable workflows that can be called by other workflows.
They hold the common testing behavior needed accross most moosetechnology repositories.
Feature requests and bug reports about reusable workflows should be expressed by an issue to this repository.

###### Starter workflows 
This repository also contains starter workflows that can be used as templates for continous integration in moosetechnology repositories.
They call the reusable workflows mentioned above, to avoid code duplication.
Their behavior can be configured using the options detailed below.

The starter workflows can be found when creating a new workflow in any moosetechnology repository, in the `Action` tab, when clicking the `New workflow` button:

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/moosetechnology/.github/assets/39184695/f4b22375-ab08-4bf2-8cf2-86ff7bcdbe97">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/moosetechnology/.github/assets/39184695/85522528-62eb-4078-95db-85230afaa694">
  <img alt="Repository -> Actions -> New Workflow">
</picture>

## Available starter workflows

### Run tests
The [tests.yml](workflow-templates/tests.yml) starter workflow is the most frequently used.
The default behavior is simple:
1. Checkout the tested branch
2. Load project
3. Run tests

The `create-artifact` option triggers the generation of an artifact containing all the files necessary to run the image locally will be generated.
See [Options](#Options).

### Run tests and update a release
The [test-and-release.yml](workflow-templates/test-and-release.yml) starter workflow is used to update a release with the generated images after testing.
It runs the tests in the same manner as run-tests.yml.
If the tests succeed, an artifact is created and uploaded as release asset.
In this workflow, `create-artifact` is always `true`.

## Options

### Available options

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
If you do not need workflow information, you can instead use [smalltalkCI custom scripts](https://github.com/hpi-swa/smalltalkCI?tab=readme-ov-file#custom-scripts).

- **release-tag**:
The tag of the release to update.
If no release exists for this tag, it will be created.

### Overriding default option values

Default values for the options are set at the organization level.
They can be overriden at a repository scope.

#### Use the same value for all branches in the repository

To override a default value for all branches in a repository, set the value as input for the reusable workflows when configuring the starter workflow.
Example, to use the latest stable Pharo version: 

```YAML
jobs:
  run:
    uses: moosetechnology/.github/.github/workflows/run-tests.yml@main
      with:
        pharo-versions: [ Pharo64-stable ]
```
#### Use different values for specfic branches

These workflows can read branch-specific informations for the following options: `pharo-versions`, `image-name` and `release-tag`.

To add branch-specific configurations, you must create a repository variable named `BRANCHES_CONFIGURATION`.
To create this variable, go to the `Settings` pane of your repository, then `Secrets and Variables`, `Actions`, `Variables` tab:

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/moosetechnology/.github/assets/39184695/c5054dd8-057b-477e-a01b-0f71273f96f5">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/moosetechnology/.github/assets/39184695/88ddb293-de9a-4277-9c0a-860890952638">
  <img alt="Settings->Secrets and Variables -> Actions -> Variables tab">
</picture>

The variable name must be `BRANCHES_CONFIGURATION` and its value must be a valid JSON string such as below:

```JSON
{
  "<branch1>": {
    "moose-version": "<moose-version-for-branch1>",
    "image-name": "<image-name-for-branch1>",
    "release-tag": "<release-tag-for-branch-1>"
  },
  "<branch2>": {
    "moose-version": "<moose-version-for-branch2>",
    "image-name": "<image-name-for-branch2>",
    "release-tag": "<release-tag-for-branch-2>"
  }
}
```
⚠️
The value of `moose-version`must be a valid Moose version. "Moose9", "Moose10", "Moose11" and so on, or "Moose-latest".
The value of `pharo-versions` will then be set according to this Moose version.
In the variables page, you can see the organization variable that stores the Pharo versions available for each Moose version.

## Limitations

Coverage is not yet supported.
