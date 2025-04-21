# unas-pro-kubernetes-nfs-provisioner
A Kubernetes NFS provisioner compatible with the UNAS Pro and Hashicorp Vault.

First add the ip-adress of your UNAS Pro drive to `deployment.yml`.
This can be found within the topology in the Unifi web UI.
Also add the folder names of your shared drives and potentially subfolders.

Please make sure you also added every ip-adress of your cluster nodes to the
UNAS Pro. This can be done in the web UI of the drive under:

```text 
Settings > Services > Add NFS connections
```

Then apply kubernetes files:

```shell
kubectl apply -f deployment.yml
kubectl apply -f storage-class.yml
kubectl apply -f service-account.yml
kubectl apply -f cluster-role.yml
kubectl apply -f clustor-role-binding.yml
```

After applying the Kubernetes resources a NFS client provisioner will start running on
your cluster.

Try it out by installing the HashiCorp Vault. 
Also see [HashiCorp documentation](https://developer.hashicorp.com/vault/docs/platform/k8s/helm)

```shell
helm repo add hashicorp https://helm.releases.hashicorp.com
helm install vault hashicorp/vault
```

Now find the pod in your Kubernetes cluster in the namespace `default`.
Shell into it and use the command:

```vault operator init```

This will give you something like this:

```text
Unseal Key 1: <token>
Unseal Key 2: <token>
Unseal Key 3: <token>
Unseal Key 4: <token>
Unseal Key 5: <token>

Initial Root Token: <token>

Vault initialized with 5 key shares and a key threshold of 3. Please securely
distribute the key shares printed above. When the Vault is re-sealed,
restarted, or stopped, you must supply at least 3 of these keys to unseal it
before it can start servicing requests.

Vault does not store the generated root key. Without at least 3 keys to
reconstruct the root key, Vault will remain permanently sealed!

It is possible to generate new unseal keys, provided you have a quorum of
existing unseal keys shares. See "vault operator rekey" for more information.
```

Store the keys and have fun!

PS: Shout out to woutceu for mentioning the mount options.
https://community.ui.com/questions/NFS-File-shares-in-UNAS/fa03aa65-afec-4106-90bd-77c7b6e044c4#answer/bf4c757e-83c9-4870-8278-d006d1dbc19b
