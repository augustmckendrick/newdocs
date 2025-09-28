# Understanding Regional Components Segregation

Up until 1.4.0, all {{{ docsVersionInfo.k0rdentName }}} components, including the API and cluster resources,
ran in the management cluster. With regional support, you’ll now be able to register another cluster as
a regional cluster. As a result:

* The management cluster is focused on control plane operations. The management cluster is a single place to manage
`ClusterDeployment`, `Credential`, `Backup`, `Template`, and other objects.
* The regional cluster is where user workloads and related infrastructure are actually created.

Depending on the setup, the management cluster in {{{ docsVersionInfo.k0rdentName }}} can be dedicated exclusively
to control-plane operations, with no providers enabled (in `spec.providers` of the `Management` object). In this setup,
it can serve only as a single pane of glass for managing the {{{ docsVersionInfo.k0rdentName }}} platform, hosting no user workloads.

## Regional Cluster Components

A regional cluster contains the following regional core components installed by {{{ docsVersionInfo.k0rdentName }}}:

1. CAPI operator
2. CAPI providers (enabled in the `spec.providers` of the Region object)
3. Cert Manager
4. Velero and so on.

The following cluster-related resources are created within the regional cluster (only for `ClusterDeployments`
that were created in the region):

1. All infrastructure objects (CAPI cluster, machines, etc)
2. `ClusterIdentity` objects for accessing cloud resources
3. Sveltos objects

## Management Cluster Components

A management cluster contains the following components:

1. All regional core components (CAPI operator, Cert Manager, Velero, etc)
2. CAPI providers (enabled in the `spec.providers` of the Management object)
3. K0rdent API (`Credential`, `ClusterDeployment`, `Templates`, etc)
4. Flux's Helm and Source controllers to install and manage subcomponents and so on.

Additionally, the cluster-related resources are created within the management cluster for `ClusterDeployments` that
were created in the management cluster.
