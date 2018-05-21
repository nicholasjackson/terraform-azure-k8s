1. Init remote state

```bash
$ terraform init \
  -backend-config "storage_account_name=nictfremotestate" \
  -backend-config="container_name=tfstate"
```

1. Create a plan

```bash
$ terraform plan -out out.plan
Acquiring state lock. This may take a few moments...
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.


------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + azurerm_kubernetes_cluster.k8s
      id:                                         <computed>
#...

Plan: 2 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------

This plan was saved to: out.plan

To perform exactly these actions, run the following command to apply:
    terraform apply "out.plan"

Releasing state lock. This may take a few moments...
```

1. Apply

```bash
$ terraform apply out.plan
Acquiring state lock. This may take a few moments...
Releasing state lock. This may take a few moments...
Acquiring state lock. This may take a few moments...
azurerm_resource_group.k8s: Creating...
  location: "" => "eastus"
  name:     "" => "nic-k8s-vault"
  tags.%:   "" => "<computed>"

  #...

azurerm_kubernetes_cluster.k8s: Still creating... (12m50s elapsed)
azurerm_kubernetes_cluster.k8s: Still creating... (13m0s elapsed)
azurerm_kubernetes_cluster.k8s: Still creating... (13m10s elapsed)
azurerm_kubernetes_cluster.k8s: Creation complete after 13m10s (ID: /subscriptions/c0a607b2-6372-4ef3-abdb-...tainerService/managedClusters/k8svault)

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
Releasing state lock. This may take a few moments...

Outputs:

# Redacted
```

1. Save kube_config

```bash
$ echo "$(terraform output kube_config)" > ~/.kube/azurek8s.tf
```

