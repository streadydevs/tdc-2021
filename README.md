# Terraform and Helm on GKE quickstart

This quickstart will walk you through:
* Using `terraform` to create and manage a GKE cluster in a VPC network
* Configuring `kubectl` for access to the GKE cluster's control plane
* Using `helm` to create and manage a release of a sample service on your GKE cluster
* Cleanup/teardown

## Prerequisites
You will need:
* A Google account for use with Google Cloud Platform
* A [GCP project](https://cloud.google.com/resource-manager/docs/creating-managing-projects) with the Compute Engine API and the Kubernetes Engine API enabled
* Basic knowledge in command line interfaces
* A shell with the following installed:
  * Helm v3
  * Google Cloud SDK (and with `gcloud` set up for your account)
  * Terraform
  * Kubectl

If you are attending the TDC session instructions will be given for Google's [Cloud Shell Editor](https://shell.cloud.google.com), which has all of the required tools installed and already configured. It is strongly recommended to use the Cloud Shell Editor if you are new to these tools.

If you are planning on using your local machine and not the Cloud Shell Editor here are some links to installation instructions
* [Installing Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)
* [Installing Helm](https://helm.sh/docs/intro/install/)
* [Installing kubectl](https://kubernetes.io/docs/tasks/tools/)

## Disclaimer
Following this quickstart will incur a cost for the cloud resources that are created. The costs associated with following this quickstart should stay well below a dollar per hour but please make sure to follow the instructions in the [cleanup](/cleanup/) section to avoid unnecessary costs. If you are unsure you did the cleanup correctly you can inspect your billing account in Google Cloud Platform in a day or two, or consider deleting the project you created resources in.


# Contents
* [1. Getting started with Terraform](/terraform/)
* [2. Getting started with Helm](/helm/)
* [3. Cleanup](/cleanup/)

