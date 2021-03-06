## Install KubeVirt

In this section, let's download the `kubevirt.yaml` manifest file upstream github repo and explore it. Then, we will deploy kubevirt with it.

Explore `~/kubevirt.yaml` file. Review the ClusterRole's, CRDs, ServiceAccounts, DaemonSets, Deployments, and Services.

Now, Install KubeVirt. You should see several objects were created.
 
```
oc apply -f ~/kubevirt.yaml
```

The following [SCCs](https://docs.openshift.com/container-platform/3.7/admin_guide/manage_scc.html) need to be added so that kubevirt controllers can launch privileged pods:

```
oc adm policy add-scc-to-user privileged -n kubevirt -z kubevirt-privileged
oc adm policy add-scc-to-user privileged -n kubevirt -z kubevirt-controller
oc adm policy add-scc-to-user privileged -n kubevirt -z kubevirt-apiserver
```

Review the objects that KubeVirt added.

```
oc project kubevirt
oc get sa | grep kubevirt
oc describe sa kubevirt-apiserver
oc get pod
HANDLER_POD=$(oc get pod -l kubevirt.io=virt-handler -o=custom-columns=NAME:.metadata.name --no-headers=true)
oc describe pod $HANDLER_POD
# review the files on the root of the filesystem of the pod, see the virt-handler executable
oc exec -it $HANDLER_POD ls /
oc get svc
oc describe svc virt-api
```

There are other services and objects to take a look at.

To review the objects through the OpenShift web console, access the console and log in as the `admin` user at `https://student<number>.cnvlab.gce.sysdeseng.com:8443`. Remember, you can use `oc cluster status` to get your URL. Ignore the self signed certificate warning

Open that URL in a browser, log in as the `admin` user with a password of `admin`.

![openshift](images/openshift-console-login.png)

Explore the `kubevirt` project by clicking on the project link in the right hand navigation pane.

![openshift](images/openshift-console-view-all.png)

Click on the different objects, explore the environment.

#### Install virtctl

Return to the CLI and install virtctl. This tool provides quick access to the serial and graphical ports of a VM, and handle start/stop operations. Also run `virtctl` to get an idea of it's options.

```
export VERSION=v0.13.0
curl -L -o /usr/bin/virtctl https://github.com/kubevirt/kubevirt/releases/download/$VERSION/virtctl-$VERSION-linux-amd64
chmod -v +x /usr/bin/virtctl
virtctl version
```

This concludes this section of the lab.

[Next Lab](../lab6/lab6.md)\
[Previous Lab](../lab4/lab4.md)\
[Home](../../README.md)
