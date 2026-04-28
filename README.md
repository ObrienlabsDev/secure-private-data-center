# Secure Private Data Center
Conslidated private data center artifacts.
This repo details various approaches to standing up an air gapped data center that optionally uses or replicates functionality in GDC (Google Distributed Cloud) - both software only and air gapped solutions.


# Hardware
## GDC - Google Distributed Cloud
- https://github.com/ObrienlabsDev/blog/issues?q=state%3Aopen%20label%3A%22GDC%22
- 
GDC is Google's version of private or hybrid cloud within your own data center. 
There are 3 main versions of GDC - where GDC Connected and GDC air-gapped are Google provided hardware and software.  We will concentrate on [GDC software only - for bare metal](https://docs.cloud.google.com/kubernetes-engine/distributed-cloud/bare-metal/docs/concepts/about-bare-metal).

[GDCC](https://docs.cloud.google.com/distributed-cloud/edge/latest/docs/overview) - Google Distributed Cloud connected (edge)

[GDCAG](https://docs.cloud.google.com/distributed-cloud/hosted/docs/ga/gdch) - Google Distributed Cloud air-gapped

[GDCS](https://docs.cloud.google.com/distributed-cloud/docs/overview#gdcv) - Google Distributed Cloud software only

There was a historical variant of GDC Hosted (renamed GDC connected (edge) where a POC can be setup to install GDC software only on your own VMs to prep for eventual delivery and integration of the 4 minimum racks in a 300k/month GDC Hosted - https://docs.cloud.google.com/distributed-cloud/hosted/docs/latest/gdch/resources/faq

### GDC Component Mapping

 Component | Use Case | GCP | GDC  | OSS | Commercial
--- | --- | --- | --- | --- | ---
API Gateway | L7 LB | Apigee | GKE dataplane 2 Gateway API | Ingress | 
Connect Agent (GKE) | anthos fleet registration | . | [Connect Agent](https://docs.cloud.google.com/kubernetes-engine/fleet-management/docs/connect-agent) | . | .
GKE Cluster Management | . | . | Anthos | [CAPI](https://github.com/kubernetes-sigs/cluster-api) | .
Identity/SSO | RBAC | . | . | KeyCloak | .
IDS/IPS | . | . | . | Falco | .
Logging | . | . | . | ELK | .
Networking/eBPF/CNI | . | . | . | Cilium | . 
Observability | . | . | . | Prometheus / OpenTelemetry | .
Org Policies | . | . | . | Open Policy Agent/Kyverno
Project | . | . | . | K8s Namespaces or clusters | .
Service Mesh | . | . | . | Istio | .
Storage | PVC/Block | . | NetApp | . | [GCNV](https://cloud.google.com/netapp-volumes?hl=en) [Symcloud](https://symphony.rakuten.com/telecom-cloud/cloud-native-storage)
VM virtualization | VMs on Kubernetes | GCE | VM Manager | [KubeVirt](https://kubevirt.io/) | .
. | . | . | . | . | .

### GDC Documentation
- https://docs.cloud.google.com/distributed-cloud/docs
- See [VM Runtime](https://docs.cloud.google.com/kubernetes-engine/distributed-cloud/bare-metal/docs/vm-runtime/enable-disable) (wraps Redhat [KubeVirt)](https://kubevirt.io/) https://docs.cloud.google.com/kubernetes-engine/distributed-cloud/bare-metal/docs/vm-runtime/enable-disable
- https://cloud.google.com/blog/topics/hybrid-cloud/using-gdc-sandbox-to-emulate-air-gapped-environments
- https://docs.cloud.google.com/distributed-cloud/sandbox/latest


### GDC Training
- https://partner.skills.google/catalog?keywords=GDC
- https://cloud.google.com/customers/rubin-observatory

### GDC software only for Bare Metal
- https://docs.cloud.google.com/kubernetes-engine/distributed-cloud/bare-metal/docs/concepts/about-bare-metal

This GDC software-only for BM looks to be a rebrand of Anthos where on prem CPUs are billed back to the GCP Project.

#### Lenovo Rack Servers as GDC Simulators
- https://vmware.lenovo.com/content/recipe/SR250%20V3-Raptor%20lake-ESXi9.0.html
- https://serverproven.lenovo.com/
- https://lenovopress.lenovo.com/lp1215.pdf#:~:text=Lenovo%20has%20certified%20the%20ThinkAgile%20VX%20solution,an%20Anthos%20Ready%20virtualized%20platform%2C%20with%20bare%2Dmetal

##### Lenovo SR250 V3 Rack Server
Lenovo XEON blades are cost effective for GDC simulation and include a secondary management CPU/Software stack for configuraiton - lead time is 30 days for shipping.
The following SR250 V3 server can run either Redhat or Ubuntu.

After post shipping diagnostics and setup...
<img width="1254" height="942" alt="Screenshot 2026-03-29 at 20 23 00" src="https://github.com/user-attachments/assets/d9c38913-ae43-408c-bb3d-e065b53b0784" />

The blades can be installed to a standard 19 inch rack in 1U slots and connected to top of rack switches/routers along with separate redundant power sources.
<img width="1485" height="849" alt="Screenshot 2026-03-29 at 20 00 20" src="https://github.com/user-attachments/assets/b3d52b15-0e7c-49e3-8fb6-5ed290194c87" />

For smaller rack depths such as 24 inches - the supplied lenovo specific extendable rails must be replaced with fixed 3rd party rails.
<img width="2052" height="299" alt="Screenshot 2026-03-29 at 21 09 48" src="https://github.com/user-attachments/assets/b06025e4-1298-4761-83a6-88a0aec4a9a2" />

SR250 V3 noise levels - https://youtu.be/E6iNi3QMMcE

#### TOR Networking
Currently using TPLink 10gbps rack switches and routers.



# Software
## Kubernetes Installations
### AWS - EKS Anywhere
- see https://aws.amazon.com/pm/eks-anywhere/
- 
### GCP - Google Distributed Cloud
- see https://github.com/ObrienlabsDev/secure-private-data-center/blob/main/README.md#gdc---google-distributed-cloud

## CAPI - Cluster API
- https://github.com/kubernetes-sigs/cluster-api
CAPI is used under the cover by Anthos.

## KubeVirt
- https://kubevirt.io/
KubeVirt is used ther the cover by VM Manager - https://docs.cloud.google.com/distributed-cloud/connected/latest/docs/virtual-machines

## Storage
### NetApp
- [GCNV](https://cloud.google.com/netapp-volumes?hl=en)
### Rakuten
- Symcloud Storage is used by GDC - https://symphony.rakuten.com/telecom-cloud/cloud-native-storage
- https://docs.cloud.google.com/distributed-cloud/connected/latest/docs/virtual-machines#configure_symcloud_storage
- https://docs.cloud.google.com/distributed-cloud/connected/latest/docs/storage
- 
## Harbor
- https://goharbor.io/
- 
## Openstack

## Open Nebula
- Open Nebula - https://en.wikipedia.org/wiki/OpenNebula


# Use Cases
- https://github.com/ObrienlabsDev/drone-streaming-extraction?tab=readme-ov-file
- https://github.com/ObrienlabsDev/blog/wiki/Drone-Streaming-Extraction
- 
# Issues
- https://github.com/ObrienlabsDev/blog/issues/174

# Links
- https://cloud.google.com/blog/topics/hybrid-cloud/using-gdc-sandbox-to-emulate-air-gapped-environments
-  GDC on bare metal - https://docs.cloud.google.com/kubernetes-engine/distributed-cloud/bare-metal/docs/downloads
- https://www.supermicro.com/en/solutions/google-distributed-cloud-virtual
- https://cloud.google.com/blog/products/infrastructure-modernization/google-distributed-cloud-edge-appliance-use-cases/
- https://docs.cloud.google.com/kubernetes-engine/distributed-cloud/bare-metal/docs/try/admin-user-gce-vms
- https://www.datacenterdynamics.com/en/news/google-makes-distributed-cloud-edge-hardware-generally-available/
- https://cloud.google.com/blog/products/infrastructure-modernization/google-distributed-cloud-edge-appliance-use-cases/
- GDC Edge Appliance is deprecated - https://docs.cloud.google.com/distributed-cloud/edge-appliance/deprecated-notice
- https://docs.cloud.google.com/distributed-cloud/hosted/docs/latest/appliance/overview


# Lenovo Rack Servers
## Lenovo SR250 V3 Rack Servers for GDC


# Private Cloud: PaaS Kubernetes stack with IaaS provisioning - HA and DR
- https://github.com/ObrienlabsDev/blog/blob/main/google-distributed-cloud.md
- https://github.com/ObrienlabsDev/blog/blob/main/private-cloud.md
- https://github.com/ObrienlabsDev/secure-private-data-center
- https://github.com/ObrienlabsDev/blog/issues/172
- https://github.com/ObrienlabsDev/blog/issues/137
- https://github.com/ObrienlabsDev/blog/issues/171
- https://github.com/ObrienlabsDev/blog/issues/170
- https://github.com/ObrienlabsDev/blog/issues/163
- https://github.com/ObrienlabsDev/blog/issues/174
- Google internal-only L300 Anthos Migration notes - https://github.com/ObrienlabsDev/gcp-infrastructure-as-code/issues/22

# Finances
## GKE Costs
GKE control planes are per cluster (regardless of size) - at $0.1/hr - with free credits of 74.4 allocated for a single cluster (autopilot or zonal) - https://cloud.google.com/kubernetes-engine/pricing
### Anthos Costs
Anthos has a one time credit of 1000US - getting details for new accounts.
<img width="910" height="43" alt="Screenshot 2026-03-31 at 16 17 50" src="https://github.com/user-attachments/assets/9b7dfaef-3bc3-404e-93c9-5338951201d2" />



# References

- ONAP - https://onap.org/
- GoC/SSC Aurora (Cross CSP Kubernetes based LZ for use in Public Sector)  - https://github.com/gccloudone-aurora including helm charts - https://github.com/gccloudone-aurora/aurora-platform-charts (not https://github.com/gccloudone/aurora) - tracking https://github.com/ObrienlabsDev/blog/issues/176
