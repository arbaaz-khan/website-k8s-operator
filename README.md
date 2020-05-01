# website-k8s-operator
Kubernetes operator for website custom resource.<br/>

# What is it?<br/>
It is a kubernetes operator developed using kubebuilder.

# What it does?
1) It creates a custom resource Website.<br>
2) Defines the specs and status for the Website.<br>
3) Implements the reconciler method to manage websites.<br>
4) It ensures that the required number of pods are running for the website,
along with a service for the website and creates an HPA for it.<br>
5) It also takes care of Horizontally scaling up the pods when the cpu utilisation
hits a certain set limit(_as defined in the website yaml_).<br>
6) It does so by leveraging the HPA resource available with kubernetes.<br>
7) On deletion of the website it deletes all the child resources that it has created.<br>

# How to use it?
[Before creating container images for the operator first customize it according to your need and test it on the cluster.]<br>
0) You need to have kubernetes running.<br>
1) Install go.<br>
2) Install kubebuilder:<br>
   refer [https://github.com/kubernetes-sigs/kubebuilder] .<br>
3) Clone this repo:<br>
   cd ~/go/src/<br>
   git clone https://github.com/arbaaz-khan/website-k8s-operator.git<br>
4) Install the CRDs into the cluster:<br>
   cd website-k8s-operator<br>
   make install<br>
5) Run your controller:<br>
   make run<br>
   (This will run in the foreground, in order to create a website workload run the following commands in another terminal)<br>
6) Create workload:<br>
   kubectl apply -f config/samples/<br>
7) Check whether the child resources created by our operator:<br>
   Deployment:<br>
   kubectl get deployment -n <namespace><br><br>
   Pods:<br>
   kubectl get pods -n <namespace><br><br>
   Service:<br>
   kubectl get svc -n <namespace><br><br>
   HPA:<br>
   kubectl get hpa -n <namespace><br><br>

8) If you want to test the scalability feature, then you need to run the k8s metric server:<br>
   kubectl create -f metric-server/