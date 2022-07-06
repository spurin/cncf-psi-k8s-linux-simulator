# CNCF PSI Linux Environment Simulator

![CNCF](https://raw.githubusercontent.com/spurin/cncf-psi-k8s-linux-simulator/main/cncf.png)

This tutorial provides you with a Linux Desktop environment that will be similar to that used in the new CKA/CKAD/CKS exams running under the new PSI exam framework.  By following this tutorial you'll have a simple Kubernetes setup (single node - command can be modified for multiple nodes) and a Linux desktop with kubectl configured for Minikube.  The browser (Firefox) is also configured for the kubernetes.io homepage.

Firstly, we'll setup our kubernetes environment using minikube -

```bash
minikube start
```

We'll then run a Linux Desktop in a container.  As we do this, we'll pass in the minikube configuration files and we'll bind to the bridge network, that was created by minikube, therefore allowing our Desktop environment to communicate with Minikube -

```bash
docker run -d \
  --name=webtop \
  --security-opt seccomp=unconfined \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Europe/London \
  -e SUBFOLDER=/ \
  -e KEYBOARD=en-us-qwerty \
  -e KUBECONFIG=/config/.kube/config \
  -p 8080:3000 \
  -v ~/.kube:/config/.kube \
  -v /google/minikube:/google/minikube \
  --shm-size="1gb" \
  --restart unless-stopped \
  --network minikube \
  spurin/webtop-k8s:latest
```

To access the Desktop, click the Web Preview Icon, if you cant find it, click -> <walkthrough-web-preview-icon>here</walkthrough-web-preview-icon> for a walkthrough on where to find it.  

Select 'Preview on Port 8080' and you're good to go!  
