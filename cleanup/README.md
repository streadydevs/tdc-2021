# Cleanup

In a normal project you might easily lose track of which cloud resources you are using and which you are not using but still paying for. If only there was a tool that could track which resources we are actually using!

Hint: it's Terraform

Terraform makes cleanup very simple. Navigate to the `terraform/` folder and run
```
terraform destroy
```

Terraform will now destroy all the infrastructure you created earlier. It might take a few minutes.

If you're having trouble removing your cloud resources you can navigate to the Google Cloud Console and delete (in order):
* Your Kubernetes cluster
* The VPC subnet called `gke-subnet`
* The VPC network called `gke-network`
* The service account called something along the lines of `tf-gke-gke-on-vpc-clus-xxxx@...iam.gserviceaccount.com`
* The IAM permission granted to the service account above

# Contents
* [1. Getting started with Terraform](/terraform/)
* [2. Getting started with Helm](/helm/)
* [3. Cleanup](/cleanup/)
