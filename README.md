# Azure Quickstarts

**This is a community based project. As such, F5 does not provide any offical support for this project**

Azure quickstarts are a result from creating the Cloud and Automation Workshop which can be found here: https://github.com/gwolfis/cloud-automation-workshop.

Azure quickstarts deliver a full deployment, including:
 * F5 BIG-IP pre-designed solutions
 * Deployed by using Terraform
 * Leveraging F5 Runtime-init
 * Include F5 Automation Toolchain: AS3, Declarative Onboarding (DO), Telemetry Streaming (TS) and Cloud Failover Extension (CFE)
 * Uses application service discovery
 * Use Azure Telemetry by deploying an Azure Workbook

 The Runtime-Init scripts used are from the F5 validated Cloud Solution Templates v2 (CSTv2). These templates can be found here: https://github.com/F5Networks/f5-azure-arm-templates-v2

 ## How to Start
 Download the code from github into your own repository.

 **git clone https://github.com/gwolfis/f5-bigip-terraform.git**

 Make sure you copy the terraform.tfvars.example to terraform.tfvars.

 **cp terraform.tfvars.example terraform.tfvars**

 Make sure you have both Terraform and Azure CLI installed on your system.

To deploy a quickstart, select a design of choice like auto-scale, failover or stand-alone and deploy by using Terraform. Each design can be used either through PAYG or by using BIG-IQ License Manager if you have one deployed. The easiest way to go is to leverage PAYG.

**Be sure that you subscribe to the F5 BIG-IP product of choice on the Azure Marketplace before deploying!**

 ```
 terraform init
 terraform plan
 terraform apply -auto-approve
 ```

* Time to deploy will take between 10 - 15 minutes.

* Terraform will deliver its output after 3 - 5 minutes.

## After Deployment
Once the BIG-IP design has been deployed in Azure you can use the Terraform output to find the BIG-IP management public IP to login. Also the Azure Console can be used to find this information.

The application can be tested and validated by using the application public IP or VIP provided through the Terraform output.

Make sure to test and generate some traffic to fill up the Azure Telemetry.

One test you shoul make for sure is providing an illegal action to awake the included WAF configuration.

Use the following curl command to create this illegal action:

**curl -sk -X DELETE https://<your_apps_public_ip_address>**

## Azure Telemetry
Logging can be watched by using the Azure Workbook which has been created during the deployment. In the Azure Console you can go to the resource group and search for **f5telemetry** and select it. Now select **Workbooks** and select **F5 BIG-IP WAF View** and check the graphs.

## Troubleshooting
When for some reason your deployment doesn't work as expected or you are just curious how things work under the hood, here are some things you can check out **on the BIG-IP**:

* To check the runtime-init deployment: **vim /config/cloud/runtime-init-conf**
* Re-running runtime-init: **f5-bigip-runtime-init --config-file /config/cloud/runtime-init-conf.yaml**
* To watch the logging generated by runtime-init colllected on the BIG-IP: **tail -f /var/log/cloud/bigIpRuntimeInit.log**
* To watch Service Discovery and Telemetry Streaming traffic: **tail -f /var/log/restnoded/restnoded.log**