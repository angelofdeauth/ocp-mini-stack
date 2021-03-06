### Prerequisites:
  + [01 Host Hypervisor - Bare Metal]
  + [02 CloudCtl RDP Bastion - LXD Container]
  + [03 VFW Firewall & Gateway - LXD Container]
  + [04 DNS & DHCP Service			- OCI Podman Container]
  + [05 Application Router Proxy - OCI Podman Container]
  + [06 Simple Artifact Server - OCI Podman Container]
--------------------------------------------------------------------------------
    
# Part 08 -- Deploy: Write Final Configuration Files & Build/Deploy Nodes
####    Step.01 Stash OCP Pull Secrets
```sh
 . ~/.ccio/ocp-mini-stack/module/cloudctl/aux/bin/init-ocp-pull-secrets
```
####    Step.02 Initialize Cluster Libvirt Virtual Machines & Restart Services
```sh
 . ~/.ccio/ocp-mini-stack/module/cloudctl/aux/bin/init-nodes-libvirt
 for i in ocp-tftpd ocp-nginx ocp-haproxy ocp-dnsmasq; do docker restart $i; done
```
####    Step.02 
```sh
curl -L https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/openshift-install-linux-4.3.0.tar.gz | sudo tar xzvf - --directory /usr/local/bin/ openshift-install
curl -L https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/openshift-client-linux-4.3.0.tar.gz | sudo tar xzvf - --directory /usr/local/bin/ kubectl oc
```
####    Step.02 Generate ignition files
```sh
 cp ~/.ccio/ocp-mini-stack/build/install-config.yaml ~/.ccio/ocp-mini-stack/module/nginx/aux/html/ignition
 openshift-install create manifests --dir=${HOME}/.ccio/ocp-mini-stack/module/nginx/aux/html/ignition/
 openshift-install create ignition-configs --dir=${HOME}/.ccio/ocp-mini-stack/module/nginx/aux/html/ignition/
```
####    Step.02 Initialize Cluster Libvirt Virtual Machines
```sh
 . ~/.ccio/ocp-mini-stack/module/cloudctl/aux/bin/start-nodes
```
    
    
---------------------------------------------------------------------------------
    
### Next Steps:
    
---------------------------------------------------------------------------------
    
######  + [Repo Module] Index
```sh
.
├── aux
│   ├── pxelinux.cfg
│   │   ├── AC1E00
│   │   └── default
│   └── systemd
│       └── tftp.service
└── README.md
```

<!-- Markdown link & img dfn's -->
[Repo Module]:/module/tftpd
[podman]: https://podman.io
[Alpine Linux]:https://alpinelinux.org/
[TFTPd]:http://freshmeat.sourceforge.net/projects/tftp-hpa/
[tftp-hpa]:http://freshmeat.sourceforge.net/projects/tftp-hpa/
[01 Host Hypervisor				- Bare Metal]:/01_HostSetup.md
[02 CloudCtl RDP Bastion		- LXD Container]:/02_CloudCTL.md
[03 VFW Firewall & Gateway		- LXD Container]:/03_Gateway.md
[04 DNS & DHCP Service			- OCI Podman Container]:/04_Dnsmasq.md
[05 Application Router Proxy	- OCI Podman Container]:/05_HAProxy.md
[06 Simple Artifact Server		- OCI Podman Container]:/06_Nginx.md
[07 TFTP Boot Artifact Server	- OCI Podman Container]:/07_Tftpd.md
[08 Deploy OpenShift Red Hat CoreOS Nodes]:/08_DeployNodes.md
