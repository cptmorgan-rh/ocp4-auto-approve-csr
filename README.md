# Auto-approve Openshift 4.x UPI CSRs

This repo is to be used with caution and mainly will help any UPI of Openshift 4.x where the [CSRs](https://docs.openshift.com/container-platform/4.1/installing/installing_bare_metal/installing-bare-metal.html#installation-approve-csrs_installing-bare-metal) need to be approved manually. Nodes that are added to an OpenShift Cluster that have not had their CSRs signed will not function properly.

The manifest in this repository will create a CronJob that runs hourly and a ConfigMap that contain a script that will automatically sign any `Pending` CSRs.

## USAGE

1) Clone the git repository
2) Create the Project by running `oc adm new-project openshift-csr-autoapprove`
3) Provide the default service account of openshift-csr-autoapprove the role of cluster-admin
  ```bash
  oc adm policy add-cluster-role-to-user cluster-admin -z default -n openshift-csr-autoapprove
  ```
4) Create the ConfigMap and CronJob by running `oc create -f ocp4-auto-approve-csr.yml`
5) Verify the Cronjob and ConfigMap has been created.

```bash
$ oc get cronjobs.batch
NAME                    SCHEDULE   SUSPEND   ACTIVE   LAST SCHEDULE   AGE
ocp4-auto-approve-csr   @hourly    False     0        <none>          4s
$ oc get cm
NAME                           DATA   AGE
kube-root-ca.crt               1      74s
ocp4-auto-approve-csr-script   1      21s
```
