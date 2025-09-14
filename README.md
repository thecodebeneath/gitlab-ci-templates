# Gitlab CI Pipeline & Job Templates

## Template Project Setup

1. Create a Gitlab group: `codebeneath`
2. Create a group project for the templates: `codebeneath\project-templates`
3. Add and commit the README.md and job template yml files to the project
   ```
   README.md
   templates/java-template-base.yml
   templates/java-default-pipeline.yml
   templates/tf-template-base.yml
   templates/tf-default-pipeline.yml
   ```

## Template Usage

Other group projects can now reuse the templates, based on the source code type (e.g. terraform, java, etc.). These can be included and optionally extended.

### Standardized Project Example:

When the consuming project is setup to use the template default pipeline & jobs without any customization.

Consuming project `.gitlab-ci.yml` file:
```
# ---
# Standard project can use the default template pipeline as-is
# ---
include:
  - project: "codebeneath/project-templates"
    ref: "main"
    file: "templates/tf-default-pipeline.yml"
```

### Customized Project Example:

When the consuming project needs to tweak the template vars or jobs, it should include the template base file. It also needs to take care to extend the `.base::tf-base` hidden job as well as the targeted hidden job for the stage.

Consuming project `.gitlab-ci.yml` file:
```
# ---
# Custom project needs to extend the base template vars and jobs
# ---
include:
  - project: "codebeneath/project-templates"
    ref: "main"
    file: "templates/tf-template-base.yml"

variables:
  TF_IMAGE: "hashicorp/terraform:1.12.0"

validate::tf-validate:
  extends:
    - .base::tf-base
    - .validate::tf-validate
  stage: validate
  before_script:
    - !reference [.validate::tf-validate, before_script]
    - echo "Running before_script..."
    - echo "tofu -version"
```

## Smoketest Project Stubs

For testing the CI templates, these following projects can be created in the same `codebeneath` group. Setup a Gitlab runner with tags for `tf` and `java` so the pipelines can be tested.

> There are three smoketest projects to showcase the naming conventions for templates, stages and jobs.

### Terraform Project
This project contains terraform modules that include the templates for terraform pipelines.
- Populate a new `codebeneath/tf-ci-smoketest` project with the folder structure under `./smoketest-projects/tf-ci-smoketest`

### Java Project
This project contains terraform modules that include the templates for terraform pipelines.
- Populate a new `codebeneath/java-ci-smoketest` project with the folder structure under `./smoketest-projects/java-ci-smoketest`

### Microservice Project
This microservice project contains both application code and supporting infrastructure, representing a self-contained service repo. It has terraform modules and java source code. It therefore will include multiple pipeline templates supporting the build and deployment of the service.
- Populate a new `codebeneath/microservices-ci-smoketest` project with the folder structure under `./smoketest-projects/microservices-ci-smoketest`
