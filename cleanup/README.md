# Cleanup

In a regular project you might easily lose track of which cloud resources you are using and which you are not using but still paying for. If only we had some way to track which resources we are using for this project.

Hint: it's Terraform

Terraform makes cleanup very simple. Navigate to the `terraform/` folder and run
```
terraform destroy
```

And terraform will destroy all the infrastructure you created earlier.

If you're having trouble removing your cloud resources you can navigate to the Google Cloud Console and delete (in order):
* Your Kubernetes cluster
* The VPC subnet called `gke-subnet`
* The VPC network called `gke-network`

# Contents
* [1. Getting started with Terraform](/terraform/)
* [2. Getting started with Helm](/helm/)
* [3. Cleanup](/cleanup/)
