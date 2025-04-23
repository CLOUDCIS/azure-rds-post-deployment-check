# Azure RDS Post Deployment Check

Powershell script to connect to Azure rds deployment after deploying from our Azure RDS deployment template. Script will check connectivity to RDWeb and also checks certificates.

Azure RDS Deployment
<a href="https://cloudinfrastructureservices.co.uk/how-to-setup-remote-desktop-services-rds-2019-farm-on-azure/">Azure RDS Farm</a>

Script does the following:

** REQUIRES AT LEAST WMF 5.0 AND AZURERM SDK **
    Script authenticates to Azure rm 
    queries all resource groups for public ip name
    gives list of resource groups
    enumerates public ip of specified resource group
    downloads certificate from RDWeb
    adds cert to local machine trusted root store
    tries to resolve subject name in dns
    if not the same as public loadbalancer ip address it is added to hosts file
    
    start with -verbose if you need to troubleshoot script

Open up PowerShell in Administrator mode and connect to your tenant where you've deployed RDS in Azure:

`Connect-AzAccount -TenantId '000000000000'`
                
### EXAMPLE 1
`.\azure-rm-rdp-post-deployment.ps1`

Query azure rm for all resource groups with for all public ips.

### EXAMPLE 2
`.\azure-rm-rdp-post-deployment.ps1 -rdWebUrl https://contoso.eastus.cloudapp.azure.com/RDWeb`


Used to bypass Azure enumeration and to copy cert from url to local cert store


### Note

If you receive an error message about PowerShell execution policyand, run the following command:

`Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Unrestricted -Force`

Then try running the script again

**NOTE:** to remove certs from all stores `Get-ChildItem -Recurse -Path cert:\ -DnsName *<%subject%>* 
