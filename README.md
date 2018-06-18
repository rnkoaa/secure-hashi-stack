# Creating a Secure Hashi-Stack Cluster

Create a Hashi stack which includes Consul, Nomad and Vault. These are all secured with self signed tls for authenticated connections among the clusters as well as communication to external applications.



## Initialize Vault Instance

To Initialize a vault cluster, the command below must be ran on the vault server

```sh
    vault operator init -key-shares=1 -key-threshold=1
    vault operator init -n 1 -t 1
```