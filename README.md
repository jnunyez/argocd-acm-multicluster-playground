#Following guide in --> https://rcarrata.com/openshift/argo-and-acm/

oc apply -f managed-cluster-set.yaml

oc label managedcluster volante-cars-lab cluster.open-cluster-management.io/clusterset=all-openshift-clusters

oc label managedcluster volante-cars-lab local-argo=True

oc apply -f gitops-cluster.yaml

oc apply -f sriov-placement.yaml

oc label managed volante-cars-lab sriov-tests=True

oc apply -f sriov-appset.yaml
