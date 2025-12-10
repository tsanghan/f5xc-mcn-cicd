# f5xc-mcn-cicd
[![Deploy to F5XC](https://github.com/tsanghan/f5xc-mcn-cicd/actions/workflows/f5xc-mcn.yaml/badge.svg)](https://github.com/tsanghan/f5xc-mcn-cicd/actions/workflows/f5xc-mcn.yaml)[![Destroy](https://github.com/tsanghan/f5xc-mcn-cicd/actions/workflows/destroy.yaml/badge.svg)](https://github.com/tsanghan/f5xc-mcn-cicd/actions/workflows/destroy.yaml)

f5xc-mcn-cicd

### How to use this repository

1. `git clone https://github.com/tsanghan/f5xc-mcn-cicd.git`
2. `cd f5xc-mcn-cicd`
3. `cp .env.example .env`
4. Wiht your favourite editor, edit `.env` file, replace `<REPLACE ME>` with appririate, correct and accurate information.
5. If you are no using Terraform S3 backend, comment out all `AWS_*` line in `.env` file.
6. Source the `.env` file, i.e., `source .env`
7. To check that the environment variable is in your current shell environment, `env | egrep "VES_|TF_" | sed 's/=.*/=<...>/'`
8. `cd tofu_xc_resources`
9. If you are NOT using Terraform S3 backend,
    - a. If you are using `Terraform`, comment out all line in `backend.tf` and ignore `backend.tofu` file.
    - b. If you are using `OpenTofu`, comment out all line in `backend.tofu` and ignore `backend.tf` file.
10. If you ARE using Terraform S3 backend,
    - a. `cp backend_config.tfvars.example backend_config.tfvars`
    - b. Wiht your favourite editor, edit `backend_config.tfvars` file, replace `<REPLACE ME>` with appririate, correct and accurate information.
11. Wiht your favourite editor, edit `vars.tf` file, change values of available `variables` to suites your needs.
12. With `Terraform`, `terraform init -backend-config="./backend_config.tfvars"`
13. With `OpenTofu`, `tofu init -backend-config="./backend_config.tfvars"`
14. Do a `plan` command, `terraform|tofu plan -out myplan.tfplan`
15. Then `apply`, `terraform|tofu apply "myplan.tfplan"`
16. Wait till `terraform|tofu` runs till completion.
17. `cd ../manifests`
18. Deploy microservices application `Brewz` into F5XC CE, `kubectl apply --server-side -f app.yaml`
19. Go to F5XC console `Multi-Cloud Network Connect` Workspace
    - Go to `Manage->Site Management->AWS VPC Sites`
    - Select your `AWS VPC Site` created by `Terraform|OpenTofu`
    - Monitor the resource till succesfully created.
20. Go to F5XC console `Distributed Apps` Workspace
    - Go to `Application->Virtual K8s`
    - Select your `Virtual K8s` created by `Terraform|OpenTofu`
    - Click `Deployment` tab at the top of the console page.
    - Monitor the deployment of the 4 microservices of application `Brewz` till a numeric `1` are shown in the `Running Pods` column for all 4 microservices.