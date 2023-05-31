# Tanzu GitOps Reference Implementation

Use this archive contains an opinionated approach to implementing GitOps workflows on Kubernetes clusters.

This reference implementation is pre-configured to install Tanzu Application Platform.

For detailed documentation, refer to [VMware Tanzu Application Platform Product Documentation](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.5/tap/install-gitops-intro.html).


# Workshop Attendee Instructions

1. The URL of the jumpbox is `jumpbox.$CODE_NAME.azure.tanzu-studio.com`, where `$CODE_NAME` is provided by the instructor.  SSH into the jumpbox using the provided credentials.  eg.

   ```bash
   ssh -i tap-green-user1 user1@jumpbox.tap-green.azure.tanzu-studio.com
   ```

1. The following environment variables have been created for you, that provide you with all the resource access information you require.  If you prefer, you can copy them to your local machine and work from there.
    * `KUBECONFIG`: Path to your cluster config file
    * `CODE_NAME`: Instance identifier for this workshop
    * `INGRESS_DOMAIN`: Assigned domain where TAP GUI, workloads, etc. will be accessible
    * `INSTALL_BUNDLE`: Cluster Essentials SHA256
    * `INSTALL_REGISTRY_HOSTNAME`: FQDN to the ACR container registry where the TAP images have been copied to
    * `INSTALL_REGISTRY_USERNAME`: ACR username
    * `INSTALL_REGISTRY_PASSWORD`: ACR password
    * `LOCAL_REGISTRY_REPO`: Local ACR repo for your TAP artifacts
    * `LOCAL_REGISTRY_USERNAME`: Local ACR username
    * `LOCAL_REGISTRY_PASSWORD`: Local ACR password
    * `TAP_PKGR_REPO`: TAP packages repo (relocated to ACR for this workshop). Used by tanzu-sync.

1. In your user home directory, you'll find:
   * a kubectl config file to access the cluster that's been provisioned for you

1. In your user home `downloads` directory, you'll find:
   * a `tanzu-cluster-essentials` tarball
   * a `tanzu-gitops-ri` tarball

1. Verify you are connected to the corresponding kubernetes cluster.  The cluster name should be the `$CODE_NAME-$USER` (eg. `tap-green-user1`):

   ```bash
   kubectl config get-contexts
   ```

1. Install cluster essentials into your cluster:

   ```bash
   tar xvf downloads/tanzu-cluster-essentials-linux-amd64-1.5.0.tgz -C $HOME/tanzu-cluster-essentials
   ./install.sh
   ```
   Ref docs [here](https://docs.vmware.com/en/Cluster-Essentials-for-VMware-Tanzu/1.5/cluster-essentials/deploy.html#deploy-onto-cluster-5).


1. Continue and install TAP using the instructions from the documentation [here](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.5/tap/install-gitops-sops.html#create-a-new-git-repository-2).  There are some things to note before starting:

   * Sample values files can be found [here](https://github.com/dbuchko/tap-gitops/tree/main/clusters/samples/01-basic)
   * You must create and use your own github repo to manage your installation.
   * Keep in mind the files that have already been downloaded into `downloads` folder of your user home directory.
   * All values required for the installation have been provided in the environment variables above.
   * TAP images have already been copied to the Azure registry.
   * TAP packages are located in the Azure registry under `$CODE_NAME/tap-packages`

   Once you get TAP with the initial basic supply chain installed, provide the instructor the external IP address of your Contour system ingress using
   
   ```bash
   kubectl get service envoy -n tanzu-system-ingress
   ```

   The instructor will create the DNS entry for the TAP GUI.  The URL format will be you can access the TAP GUI at the following URL:

      [https://tap-gui.$USER.$CODE_NAME.azure.tanzu-studio.com](https://tap-gui.$USER.$CODE_NAME.azure.tanzu-studio.com).  
      
      eg. [https://tap-gui.user1.tap-yellow.azure.tanzu-studio.com](https://tap-gui.user1.tap-yellow.azure.tanzu-studio.com)



