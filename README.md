# Azure Pipeline Template for Continuous Delivery/Continuous Deployment Versioning

## Overview

This repository contains an example Azure Pipeline template designed for versioning Continuous Delivery / Continuous Deployment (CD) processes. The pipeline template updates the image version in the infrastructure repository (e.g., written in Terraform) in the `defaults/product.yaml` file based on the release branch version of the Continuous Integration (CI) process in the application code repository.

## Versioning Format

The versioning format used in this pipeline is `vX.Y.Z-I`, where:
- **X**: Major version
- **Y**: Minor version
- **Z**: Patch version
- **I**: Infrastructure version

## Usage

To utilize this pipeline template, follow these steps:

1. **Setup Repository**:
   - Ensure you have two repositories:
     1. **Application Code Repository**: Contains the application code and CI pipeline.
     2. **Infrastructure Repository**: Contains the infrastructure code (e.g., Terraform scripts) and this pipeline template.

2. **Define the Pipeline in Application Code Repository**:
   - In the application code repository, set up a release branch (manually or automatic by script) in semantic versioning `release/vX.Y.Z` which triggers the CI pipeline and generates a version number. Succesfully finished pipeline CI should trigget pipeline CD (point 3)

3. **Configure the Versioning Pipeline**:
   - In the infrastructure repository, create a pipeline file (e.g., `azure-pipelines.yml`) which reference the `base/cd.yml` pipeline template.  Add the following triggering mechanism to your pipeline:

   ```yaml
   resources:
     pipelines:
     - pipeline: CI_Pipeline
       project: CI_Project
       source: 'CI_Pipeline_Name'
       trigger:
         branches:
           include:
             - release/*
           stages:
           - ci
