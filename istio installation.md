To install istio: [Istio / Getting Started](https://istio.io/latest/docs/setup/getting-started/)

Newer version is not stable with gke, so we will use the old one:

1- curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.17.1 TARGET_ARCH=x86_64 sh -

2- cd istio-1.17.1

3- export PATH=$PWD/bin:$PATH

4- istioctl install --set profile=demo -y

5- kubectl label namespace default istio-injection=enabled

Note: all these steps must be completed before deploying the pod, since the 5th step will attach the sidecar to the newly created pods.

Then deploy the sample book app:

	-> kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml


After installing and deploying the book demo, you will need a way to view the graph (a visualizer), most famous ones are:

	- Kiali
 
	- Grafana
 
	- Jaeger

#Install Kiali

To install kiali, make sure that you are in the istio directory and create the deployment: 

	-> kubectl apply -f samples/addons

#Uninstall Kiali

To uninstall kiali:

	kubectl delete serviceaccounts -n istio-system kiali
 
	kubectl delete configmaps -n istio-system kiali
 
	kubectl delete clusterroles.rbac.authorization.k8s.io -n istio-system kiali-viewer
 
	kubectl delete clusterroles.rbac.authorization.k8s.io -n istio-system kiali
 
	kubectl delete clusterrolebindings.rbac.authorization.k8s.io -n istio-system kiali
 
	kubectl delete roles.rbac.authorization.k8s.io -n istio-system kiali-controlplane
 
	kubectl delete rolebindings.rbac.authorization.k8s.io -n istio-system kiali-controlplane
 
	kubectl delete services -n istio-system kiali
 
	kubectl delete deployments -n istio-system kiali


Then you need to enable port forwarding in order to access kiali, in gke you do that by going to the service and enable it there:

![image](https://github.com/BugsCleaners/Istio-installation/assets/91881471/56e9f163-77c7-4f76-80bd-13f490e97be5)

Then on the same page an option called "open in web preview" will appear under "port forwarding"; just press it.

You can now play with your APIs and you will see the requests relations on kiali graph, but they will be coming from "unknown" location and every API could be reached differently depends on your services.

To create a gateway that maps different URLs to different routes using only one external IP, we need an ingress gateway. 
