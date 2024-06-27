# Durable Functions Monitor .Net 8 Isolated for MSSQL

This is purely sample code
The initial code was taken from https://github.com/microsoft/DurableFunctionsMonitor/tree/main/custom-backends/dotnet7isolated-mssql and modifications made to support .net 8

Only basic testing has been done. Authenticated scenarios have not been tested


Custom Durable Functions Monitor .NET 8 Isolated backend project to be used with [Durable Task SQL Provider](https://microsoft.github.io/durabletask-mssql/#/).

## How to run locally

* Clone this repo.
* In the project's folder create a `local.settings.json` file, which should look like this:

```
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsSecretStorageType": "files",
    "DFM_SQL_CONNECTION_STRING": "your-mssql-connection-string",
    "DFM_NONCE": "i_sure_know_what_i_am_doing",
    "FUNCTIONS_WORKER_RUNTIME": "dotnet-isolated",
  },
  "Host": {
    "LocalHttpPort": 7072
  }
}
```

* Go to the project's folder with your command prompt and type the following:

```
func start
```

* Navigate to http://localhost:7072/durable-functions-monitor


## Running on Docker locally

Build the container
```bash 
docker build -t dfmon8-mssql:v3 .
```

Run the container

```
docker run -p 7072:80 -e DFM_SQL_CONNECTION_STRING="YOURCONNECTONSTRINGTOSQL" -e DFM_NONCE="i_sure_know_what_i_am_doing" dfmon8-mssql:v3

```  

## Running in AKS

``` powershell
$resourceGroup="yourRGName"
$acrName="ContainerRegistryName"
$API_NAME="dfmon8-mssql:v1"
az acr build --registry $acrName --image $API_NAME --resource-group $resourceGroup .
``` 

Update the dfmon.yaml with name of the container and then run

``` bash
kubectl apply -f dfmon.yaml
```

run kubectl get services to get the external IP of the service and browse to it
```
kubectl get services
```
