# Rancher AzureGov YAML Example

This is an example of declaratively creating AzureGov RKE2 clusters through Rancher provisioning.

This can provide GitOps and automation support around provisioning to AzureGov.

## Getting Your Azure Credentials

You need to create a Service Principal within your respective AzureGov account before utilizing these examples. Utilize the Azure CLI (`az`), update the following, and run this command:

```bash
az ad sp create-for-rbac \
  --name="example-rancher-sp" \
  --role="Contributor" \
  --scopes="/subscriptions/YOUR_SUBSCRIPTION_ID_HERE"
```

This will output something like the following:

```bash
{
  "appId": "abcdefgh-1234-abdc-1234-abcedfghijkl",
  "displayName": "example-rancher-sp",
  "password": "AbCdEfG12345678.A~bCeDeF12345~Ab.C",
  "tenant": "abcdefgh-1234-abdc-1234-abcedfghijkl"
}
```

Take these values and update the [azure-creds.yaml](./azure-creds.yaml) file. **NOTE**: The `appId` maps to the `clientId` in the secret.

For more info, see the [docs](https://ranchermanager.docs.rancher.com/how-to-guides/new-user-guides/launch-kubernetes-with-rancher/use-new-nodes-in-an-infra-provider/create-an-azure-cluster).

## Provisioning

Look through the [cluster.yaml](./cluster.yaml) and [azureconfig.yaml](./azureconfig.yaml) and update any fields based off your requirements and specific environments you're working in. After you update those and the secret with your credentials, ensure your kubecontext is pointing at your Rancher local cluster, and apply the files (from this directory):

```bash
kubectl apply -f .
```