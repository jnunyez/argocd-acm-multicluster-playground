#Following guide in --> https://rcarrata.com/openshift/argo-and-acm/

* Create a ManagedClusterSet called all-openshift-cluster in Red Hat Advanced Cluster Management. To add clusters in this clusterSet, we need to add the following label cluster.open-cluster-management.io/clusterset=all-openshift-clusters to the ManagedClusters to import:

    ```
    oc apply -f managed-cluster-set.yaml
    ```

* In this case mark cluster named volante-cars-lab and so we have all the
    cluster grouped in a clusterset:

    ```
    oc label managedcluster volante-cars-lab cluster.open-cluster-management.io/clusterset=all-openshift-clusters
    ```

* Now import the desired clusters to ArgoCD. This is done with GitOpsCluster that defines where
    is the ArgoCD to integrate with, along with the placement rule to use in
    this `local-argo=True`.

  ```
  oc label managedcluster volante-cars-lab local-argo=True
  oc apply -f gitops-cluster.yaml
  ```

* Create placement rule to select the cluster to be used by the AppSet. Mark the cluster where you want to run the Appset.

    ```
  oc apply -f sriov-placement.yaml
  oc label managed volante-cars-lab sriov-tests=True
  ```

* The Argo Appset uses a clusterDecisionResource generator. This references the placement rule referenced created with `sriov-placement.yaml`.

    ```
    oc apply -f sriov-appset.yaml
    ```
