This repository contains configurations for the moosetechnology prganization.

# Workflows
This repository contains starter workflows that can be used to as templates in repositories in the organization.
We also provide reusable workflows that can be called by another workflow. They are

## Reusable workflows
They are workflows that are called by other workflows.
Their goal is to hold the testing behavior that is common accross moosetechnology repositories.
Bug report and feature requests about reusable workflows should be expressed by an issue to this repository.

## Starter workflows
They are templates that are available when creating a new workflow in a moosetechnology repository. 
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

## About options
[TODO]
