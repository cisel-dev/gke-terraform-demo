This article will cover the creation of a GKE cluster using Terraform Cloud. Google cloud provide some built-in CLI, so why should you consider using Terraform instead?

Well here are some advantages using terraform.




- 
Unified Workflow - If you are already deploying infrastructure to Google Cloud with Terraform, your GKE cluster can fit into that workflow. You can also deploy applications into your GKE cluster using Terraform.

- 
Full Lifecycle Management - Terraform doesn't only create resources, it updates, and deletes tracked resources without requiring you to inspect the API to identify those resources.

- 
Graph of Relationships - Terraform understands dependency relationships between resources. For example, if you require a separately managed node pool, Terraform won't attempt to create the node pool if the GKE cluster failed to create.

source: https://learn.hashicorp.com/

### Prerequisite

We assume that you have installed correcty GCP cli (gcloud) on your computer. Follow these instruction to do so https://cloud.google.com/sdk/docs/quickstarts

You must have a git repo either Github or Gitlab.

You will need to clone this repo : https://github.com/cisel-dev/gke-terraform-demo

**Let's get started**

Push the files cloned from the repo above to a project of you own Git. For example with a repository name like demo-terraform-gke-cluster.
Edit the ge.tf file to put some username and password for your GKE cluster.


```
variable "gke_username" {
  default     = "choose_a_username_and_put_it_here"
  description = "gke username"
}

variable "gke_password" {
  default     = "choose_a_password_and_put_it_here"
  description = "gke password"
}
...
``` 

Now you have to generate a json credential file and replace the file name "account.json".

```
$ gcloud init
$ gcloud auth application-default login
``` 
When authentication is done a json file is created in your home directory.

> 
$HOME/.config/gcloud/application_default_credentials.json

copy this file to your repo and rename it "account.json" (replace the existant file).
This is what will authenticate you with GCP.




Then create a terraform cloud workspace

Go ahead and create an account on app.terraform.io. Then link you Github or Gitlab Version Control System (VCS) with your Terraform Cloud.

To do so, simply follow the Terraform documentation below:


- 
GitLab : terraform.io/docs/cloud/vcs/gitlab-eece.html


- 
GitHub : terraform.io/docs/cloud/vcs/github-app.html

After you successfully added your VCS we will create a Terraform Workspace and select the newly created repo in the list.

![tfwp.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1612799909228/YZkJvY1jk.png)
The Workspace will take some time to synchronize. When everything is good you will see it in the list.


![tempsnip.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1612858825749/r-CU7fxLo.png)

Good, now create some variables in terraform cloud, this will replace the terraform.tfvars file (which is present in the clone repo). We kept this file so you can use the project with terraform cli instead of terraform cloud if you need. In fact Terraform cloud will run an ephemeral VM to execute you terraform operation. It will create a terraform.tfvars based on the the variable you created in this step.

Add 2 project variables:

- 
project_id: your_gcp_prject_id 

- 
region: region_to_use for exemple europe-west1

The project Id can be retrieved the the GCP console, or with the command below.

```
gcloud config get-value project
``` 


![tempsnip2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1612861979261/9l2yR1X6s.png)

OK we are good to go, you can now deploy your GKE cluster with terraform cloud. As your terrform workspace is linked to a VCS, when you update files, it will automatically trigger a terraform plan and ask you to confirm & apply the change. 

![apply.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1612865180266/KezuxnGvO.png)

You can also trigger manually a plan by queuing it like the the screen below.

![queue.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1612864951294/UIiIiDzk8.png)

You can play with your workspace settings in order to trigger a plan withtout confirmation or maybe to trigger it only when some specific files are edited. There is some fine tuning to do!

Feel free to ask questions in the comments section below.


cisel.ch
