# .NET tips

## Installing azure-cli on Windows and make it available to GitBash

1) Install Azure-cli MSI [https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?view=azure-cli-latest]

2) Open GitBash

3) Try az.cmd. It should start.

4) Writing az.cmd every time is BS. Create a bash alias:
    
    $ alias  az='az.cmd'
    
    
## Deploying Angular frontend as Azure webapp

First understand: there's a resource group, where you group all the services you really use: like webapps, api's and so on.
And this resource group needs a Plan (mainly a money plan as I understood) related to pricing.

### Let's create a resource group.
So, using our azure-cli:

```bash
$ az group create --name myResourceGroup --location westus
```
    
### Then we create a Plan.
The following is a free plan:

```bash
$ az appservice plan create --name myPlan --sku F1
```
    
### And now we create the service.
In this case a webapp, which we'll associate it to our plan and our resource group:

```bash
$ az webapp create --name myExpressApp-chrisdias --plan myPlan --runtime "node|6.9" --resource-group myResourceGroup21980742198476219
```

If you don't know resource group id, just ask for it:

```bash
$ az group list
```

And look for the one you are interested, then take the id.

### Check everything is cool:
```bash
$ az webapp browse --name myExpressApp-chrisdias
```

### Set up a remote

Use Git for deployment. Get git url:

```bash
$ az webapp deployment source config-local-git --name myExpressApp-chrisdias
```

It must return an URL.

Take that url and set it as azure remote:

```bash
$ git remote add azure https://chrisdias@myexpressapp-chrisdias.scm.azurewebsites.net/myExpressApp-chrisdias.git
```

Now if you do git push azure master it won't work because credentials for Git repo are not set up. 
NOTE THAT AZURE CREDENTIALS AND GIT CREDENTIALS ARE DIFFERENT!

Just use:

```bash
$ az webapp deployment user set --user-name <UserName> --password <Password>
```




