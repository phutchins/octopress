---
layout: post
title: "Kubernetes Master or Minion DNS Failure"
date: 2017-01-27 07:24
comments: true
published: false
categories:
---

# Problem
After upgrading one of my Kubernetes clusters, the master node became available for scheduling pods. When pods were scheduled on the master, DNS from those pods would not resolve.

# Troubleshooting

## Master History
```
    1  docker ps
    2  ping 10.0.0.10
    3  curl
    4  curl 10.0.0.10
    5  traceroute
    6  ls /usr/bin
    7  curl -i http://10.0.0.10
    8  iptables-save
    9  iptables-save | grep 10.0.0.10
   10  kubectl exec busybox cat /etc/resolv.conf
   11  docker ps | grep dns
   12  docker ps
   13  docker ps | grep proxy
   14  ls
   15  pwd
   16  whoami
   17  cd /etc/kubernetes/manifests/
   18  ls
   19  vi kube-proxy.manifest
   20  docker pull gcr.io/google_containers/kube-proxy:697a993392e4f1c74fc6879c59c068e6
   21  docker --help
   22  cat ~/.docker
   23  ps uax
   24  su - kubernetes
   25  sudo -u kubernetes docker ps
   26  id kubernetes
   27  cd /home/kubernetes
   28  echo $HOME
   29  export HOME=/home/kubernetes
   30  docker ps
   31  docker pull gcr.io/google_containers/kube-proxy:697a993392e4f1c74fc6879c59c068e6
   32  docker pull google_containers/kube-proxy:697a993392e4f1c74fc6879c59c068e6
   33  ls
   34  pwd
   35  ls -altr
   36  export HOME=/root
   37  vi ~/.dockercfg
   38  export HOME=/home/kubernetes
   39  cd /home/kubernetes
   40  vi ~/.dockercfg
   41  docker pull google_containers/kube-proxy:697a993392e4f1c74fc6879c59c068e6
   42  docker pull gcr.io/google_containers/kube-proxy:697a993392e4f1c74fc6879c59c068e6
   43  docker --help
   44  docker pull --config=~/.dockercfg gcr.io/google_containers/kube-proxy:697a993392e4f1c74fc6879c59c068e6
   45  docker --config=~/.dockercfg pull gcr.io/google_containers/kube-proxy:697a993392e4f1c74fc6879c59c068e6
   46  docker --config=~/.dockercfg pull google_containers/kube-proxy:697a993392e4f1c74fc6879c59c068e6
   47  pwd
   48  ls
   49  curl -O https://storage.googleapis.com/configs.kuar.io/kube-proxy-pod.yaml
   50  ls
   51  vi kube-proxy-pod.yaml
   52  ls
   53  cd kube-manifests/
   54  ls
   55  cd kubernetes/
   56  ls
   57  vi kube-proxy.manifest
   58  ls
   59  cd addons/
   60  ls
   61  ls
   62  cd ..
   63  ls
   64  vi kubeproxy-config.yaml
   65  curl https://gcr.io/v2/google_containers/hyperkube/tags/list
   66  curl https://gcr.io/v2/google_containers/hyperkube/tags/list | jq '.'
   67  curl https://gcr.io/v2/google_containers/hyperkube/tags/list | grep "kube-proxy"
   68  curl https://gcr.io/v2/google_containers/kube-proxy/tags/list | grep "kube-proxy"
  69  ls
   70  vi kube-proxy.manifest
   71  cd /etc/kubernetes/manifests/
   72  ls
   73  vi kube-proxy.manifest
   74  docker pull --config=~/.dockercfg gcr.io/google_containers/kube-proxy:1.5.2
   75  docker pull gcr.io/google_containers/kube-proxy:1.5.2
   76  ls
   77  vi kube-proxy.manifest
   78  iptables-save | grep 10.0.0.10
   79  iptables-save
   80  iptables-save | grep proxy
   81  iptables-save | grep PROXY
   82  ls
   83  ls
   84  vi kube-proxy.manifest
   85  cd /var/lib/kube-proxy/kubeconfig/
   86  ls
   87  cd ..
   88  ls
   89  rmdir kubeconfig/
   90  vi kubeconfig
   91  ls
   92  pwd
   93  cd /etc/kubernetes/
   94  ls
   95  cd manifests/
   96  ls
   97  vi kube-proxy.manifest
   98  ls /var/lib/kube-proxy/
   99  cd /var/log
  100  ls -altr
  101  vi kube-proxy.log
  102  iptables-save
  103  iptables-save | grep 10.0.0.10
  104  ls
  105  ls /var/lib/kube-proxy/
  106  cat /etc/kubernetes/manifests/kube-proxy.manifest
  107  ls /usr/share/ca-certificates
  108  ls -altr
  109  vi kube-proxy.log
  110  ls -altr
  111  tail -f kube-apiserver.log
  112  tail -f kube-apiserver.log
  113  ls
  114  ls | grep dns
  115  ls -altr
  116  tail -f kube-addon-manager.log
  117  tail -f kube-controller-manager.log
  118  date
  119  ls
  120  ls -altr
  121  tail -f kube-apiserver.log
  122  ls
  123  tail -f ./*.log |grep token
  124  systemctl restart kubelet
  125  tail -f ./*.log |grep token
  126  ls
  127  ls -altr
  128  tail -f kube-apiserver.log
  129  docker ps
  130  cd /etc/kubernetes/manifests/
  131  ls
  132  vi kube-proxy.manifest
  133  ls
  134  history
  135  curl https://gcr.io/v2/google_containers/kubernetes-dashboard/tags/list
  136  curl https://gcr.io/v2/google_containers/kubernetes-dashboard-amd64/tags/list
```

## Local History 1
```
 551  kube exec -it filebeat-57bsf /bin/bash
  552  kubectl get pods --namespace=kube-system -l k8s-app=kube-dns
  553  kube get logs kube-dns-4101612645
  554  kube logs kube-dns-4101612645
  555  kube logs -f kube-dns-4101612645-5dbmw
  556  kube describe kube-dns-4101612645-5dbmw
  557  kube describe pod kube-dns-4101612645-5dbmw
  558  kube get pods
  559  kube get pods
  560  kube get pods
  561  cd github/kube-infra/
  562  ls
  563  cd kube-stag
  564  cd kube-nonprod/
  565  ls
  566  cdk ub
  567  cd kubernetes
  568  ls
  569  cd cluster/
  570  ls
  571  cd addons
  572  ls
  573  cd ..
  574  ls
  575  cd gce
  576  ls
  577  cd ..
  578  cd ..
  579  find ./ -name kube-proxy*
  580  cd docs/admin/
  581  vi kube-proxy.md
  582  kube --version
  583  kubectl version
  584  kube get pods
  585  kube delete kube-proxy-kubernetes-master
  586  kube delete pod kube-proxy-kubernetes-master
  587  kube get pods
  588  kube get pods
  589  kube get pods | grep dns
  590  kube logs -f kube-dns-4101612645-5dbmw
  591  kube describe pod kube-dns-4101612645-5dbmw
  592  kube get pods
  593  kube delete pod kube-proxy-kubernetes-master
  594  kube get pods
  595  kube logs -f kube-proxy-kubernetes-master
  596  kube get services
  597  kube proxy --help
  598  kube get daemonsets
  599  kube daemonset --help
  600  kubectl --help
  601  kube config
  602  kubectl delete secrets/default-token-bkghp --namespace=kube-system
  603  kube get secrets
  604  kubectl delete secret default-token-1rq5h --namespace=kube-system
  605  kns
  606  kenv
  607  kube get secrets
  608  kube get secret token-kube-proxy
  609  kube describe secret token-kube-proxy
  610  kube get pods
  611  kube delete pod kube-dns-4101612645-5dbmw
  612  kube get pods
  613  kube get pods
  614  kube get pods
  615  kube get pods
  616  kube get pods
  617  kube get pods
  618  kube get pods
  619  kube get pods
  620  kube get pods
  621  kube get pods
  622  kube get pods
  623  kube get pods
  624  kube get services
  625  kube info
  626  kube help
 627  kube cluster-info
  628  kube get deployments
  629  kube edit deployment kubernetes-dashboards
  630  kube edit deployment kubernetes-dashboard
  631  kube get pods
  632  kubectl cluster-info
  633  kube get pods
  634  kube get pods | grep insight
  635  kubectl --namespace=default get pods
  636  kubectl --namespace=default cluster-info
  637  kube get services
  638  kns default
  639  kube get services
  640  kube describe service cluster-insight
  641  kube get rc
  642  kube get pods
  643  kube describe pod cluster-insight-controller-t3x3p
  644  kubectl --namespace=default cluster-info
  645  kube describe service cluster-insight
  646  kube get nodes
  647  history
```


# Solution
+ Install kube-proxy on master node
kube-proxy handles creating iptables records for proxying traffic from containers to dns services (10.0.0.10)
  The kube-proxy is currently installed as a manifest on each actual node in /etc/kubernetes/manifest/kube-proxy.manifest



## Links
  https://github.com/kubernetes/kubernetes/issues/19332
https://github.com/kelseyhightower/kubernetes-the-hard-way/tree/master/docs
https://github.com/kubernetes/kubernetes/issues/15108
https://kubernetes.io/docs/admin/out-of-resource/
https://blog.docker.com/2015/02/orchestrating-docker-with-machine-swarm-and-compose/

  ctrl+p
