# Terraform Quickstart

The terraform configuration in this guide is based on the [simple regional with networking](https://github.com/terraform-google-modules/terraform-google-kubernetes-engine/tree/v17.0.0/examples/simple_regional_with_networking) example in the terraform `gke` module by Google.

All commands in this section should be run from the `terraform/` directory:
```
cd terraform
```

## Overview

Terraform is an open-source infrastructure as code software tool. It allows you to write infrastructure as code using declarative configuration files.

## Inspecting the terraform files

This folder contains four main files containing terraform configuration
* `main.tf`
Contains the terraform block, which is used to configure terraform behavior and required module versions

* `gke-with-network.tf`
Contains modules for defining a VPC network and a GKE cluster

* `variables.tf`
Contains the definitions for the input variables for this terraform configuration

* `outputs.tf`
Contains the output values for the terraform configuration.
In this quickstart the outputs are only used for CLI output of relevant values after a run.


## Setting your input variables

For this quickstart we'll stick to one set of defined variables.


We will stick to a single set of variables in a file called `terraform.tfvars`.

Note: `.tfvars` files are normally NOT tracked by version control

Let's create this file by running:
```
touch terraform.tfvars
```

Edit the file and add
```
project_id = "YOUR PROJECT ID HERE"
region     = "europe-north1"
```

Real world considerations:
* A real-world configuration might contain one set of variables for each environment, as in `prod.tfvars`, `staging.tfvars` and `testing.tfvars`.


## Initializing terraform

Initialize terraform by running
```
terraform init
```

This prepares the current working directory for use with Terraform, such as installing required modules.


## Making your first terraform plan

To start a plan run:
```
terraform plan
```

This causes terraform to plan which changes are required to go from the current state (none) to a state that conforms to what is declared in your terraform configuration. It will then output a list of planned changes, and a summary showing the total amount of resources that are going to be created, changed and destroyed.

Note: If you get errors during this step make sure:
* You have configured `gcloud` correctly with access to your project
* You have enabled the Compute Engine API and the Kubernetes Engine API in Google Cloud Platform


## Applying your first plan

To apply a plan run:
```
terraform apply
```

This causes terraform to apply all planned changes by calling APIs in Google Cloud Platform in an order decided by resource dependencies. This could take a few minutes.

Sit back and relax, and think of all the clicks in Google Cloud Platform you are saving.

Inspect the result by poking around in the [Google Cloud Console](https://console.cloud.google.com)


## Making changes to your plan

Terraform is not just for creating cloud resources, but for managing them and keeping track of the current state of your cloud configuration. This state is by default stored in a file called `terraform.tfstate` and keeps track of the state of all resources managed by terraform.

To verify that terraform tracks state correctly you can do a plan without any changes to the configuration:
```
terraform plan
```
This should output `Your infrastructure matches the configuration`


Our cluster is a regional cluster with redundant nodes in all zones. Let's limit our cluster to only create nodes in the zone `europe-north1-a` by adding a value for `zones` in the terraform input variables in `terraform.tfvars`
```
# terraform.tfvars
project_id = "YOUR PROJECT ID HERE"
region     = "europe-north1"
zones      = ["europe-north1-a"]
```

After the change, run:
```
terraform apply
```
Before typing `yes` make sure you are happy with the changes terraform is suggesting

Real world considerations:
* The `terraform.tfstate` file should NOT be committed as it contains sensitive information such as IP addresses and credentials for accessing cloud resources. To share this state between team members you should set up a [remote backend](https://www.terraform.io/docs/language/settings/backends/index.html) such as `gcs` for storing the file in a Google Cloud Storage bucket, or using state management from [Terraform Cloud](https://www.terraform.io/cloud) (free up to 5 team members).
* For keeping track of terraform state for several environments it's recommended to use [workspaces](https://www.terraform.io/docs/cloud/guides/recommended-practices/part1.html) to separate environments.
* For production infrastructure you should use `terraform plan -out <plan-file>` and `terraform apply <plan-file>` to make sure changes are as expected, or even better have CI/CD or similar trigger terraform runs [GitOps style](https://www.hashicorp.com/resources/unlocking-cloud-with-gitops-with-terraform-and-sentinel).

## Accessing your newly created cluster

If everything went well your cluster is now running and ready for workloads.

To configure `kubectl` for access to your cluster's control plane run:
```
gcloud container clusters get-credentials gke-on-vpc-cluster --region europe-north1
```

Test your access to the GKE cluster by running
```
kubectl get event
```
If successful this will output a log of all the recent events in the cluster


You are now set up and ready for the next section where you will deploy a sample application to the cluster by using `helm`


# Contents
* [1. Getting started with Terraform](/terraform/)
* [2. Getting started with Helm](/helm/)
* [3. Cleanup](/cleanup/)

