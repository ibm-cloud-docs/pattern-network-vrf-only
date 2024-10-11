---

copyright:
  years: 2024
lastupdated: "2024-10-11"


subcollection: pattern-network-vrf-only

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Deploying Classic Infrastructure in a Single Region
{: #introduction}

This guide outlines deploying a Classic edge gateway architecture in a single region configuration, the Classic Edge gateway pattern allows on-premise traffic to flow into a set of firewalls prior to routing traffic to IBM Cloud’s PowerVS and Virtual Private Cloud (VPC) environments. It is used to integrate on-premises access with classic infrastructure and Power Virtual Server workloads within the IBM Cloud. The deployment is based on existing deployable architecture modules, as well as a series of manual customizations to tailor the setup to the specific requirements for your environment.

This is designed for customers who need a scalable, single region infrastructure with the flexibility of manual customizations post initial deployment of the base Deployable Architecture. It allows for adapting various components, such as networking and security, to better suit individual business needs after the foundational architecture has been established.

This deployment guide uses IBM Cloud catalog, Terraform IBM modules (TIM), and manual configuration to achieve deployment. It assumes that the landing zone prerequisites have been completed including IBM Cloud account setup, IAM permissions, access roles, and SSH keys. Please follow instructions for setting up your environment for [deployable architecture](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-plan).

![Illustrates a detailed network and component architecture for a single Classic Data Center solution architecture](images/classic-VRF.svg){: caption="Single  Region View" caption-side="bottom"}

## Before you begin
{: #begin}

You need the following items to deploy and configure this reference architecture:

-   An [IBM Cloud account](/registration){: external}.
-   [Required IAM access policies](https://github.com/terraform-ibm-modules/terraform-ibm-web-app-mzr-da/tree/main/solutions/e2e#required-iam-access-policies){: external}.
-   An [IBM Cloud API key](/docs/account?topic=account-userapikey&interface=ui) for the user or service ID with the correct IAM access policies.
-   Review [Planning for the landing zone deployable architectures](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-plan).

## Provision Architecture
{: #provision}

### Direct Link Connect
{: #directlink}

1.  Review the list of Direct Link Connect [providers and locations](/docs/dl?topic=dl-locations#connect-locations) to select a provider.
2.  Review [partner-specific](/docs/dl?topic=dl-how-to-order-ibm-cloud-dl-connect#instructions-partner) instructions.
3.  Order primary [Direct Link Connect](/docs/dl?topic=dl-how-to-order-ibm-cloud-dl-connect) to xcr01
4.  Order secondary [Direct Link Connect](/docs/dl?topic=dl-how-to-order-ibm-cloud-dl-connect) to xcr02 for resiliency.

This deployment guide assumes the remote side of this connection is already integrated with the exchange provider’s network. If not, please contact the provider to establish this connection.{: note}

Alternative: Two [Direct Link Dedicated](/docs/dl?topic=dl-how-to-order-ibm-cloud-dl-dedicated) connections can be ordered.

### Classic Infrastructure
{: #classic}

1.  Create [Security groups and rules](/docs/security-groups?topic=security-groups-creating-security-groups) either by the IBM cloud portal or by using the Terraform IBM Modules (TIMs module) [ibm_security_group](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/security_group){: external} and [ibm_security_group_rule](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/security_group_rule){: external}.
2.  Create [SSH Keys](/docs/ssh-keys?topic=ssh-keys-getting-started-tutorial) in the IBM Cloud portal or by using the TIMs module [ibm_compute_ssh_key](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/compute_ssh_key){: external}.
3.  Provision the gateway appliance of choice:
    1.  [Juniper vSRX](/catalog/infrastructure/gateway-appliance?catalog_query=aHR0cHM6Ly9jbG91ZC5pYm0uY29tL2NhdGFsb2c%2Fc2VhcmNoPWdhdGV3YXklMjUyMGFwcGxpYW5jZSNzZWFyY2hfcmVzdWx0cw%3D%3D){: external}.
    2.  [Virtual Router Appliance (vRA)](/gen1/infrastructure/provision/gateway){: external}, select ATT vRouter for vendor or TIMs module [ibm_network_gateway](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/network_gateway){: external}.
    3.  [FortiGate FSA 10 Gbps](/netsec/firewalls/multi-vlan/provision?catalog_query=aHR0cHM6Ly9jbG91ZC5pYm0uY29tL2NhdGFsb2c%2Fc2VhcmNoPWZvcnRpZ2F0ZSNzZWFyY2hfcmVzdWx0cw%3D%3D#create){: external}.
    4.  [FortiGate vFSA](/gen1/infrastructure/provision/gateway){: external}, select Fortinet for vendor.
    5.  Bring your own Gateway Appliance [(BYOG)](https://cloud.ibm.com/gen1/infrastructure/provision/gateway){: external}, and select other for vendor.
4.  Establish a GRE tunnel (GREa) from the gateway of choice to the client on-premise device.

    Configure BGP to peer your IBM Cloud Classic Gateway with the on-premise device for route exchange over the GRE tunnel (use the GRE tunnel IP addresses as the BGP neighbours).{: tip}

5.  Create VLANs and subnets for compute resources that need to be deployed in Classic using the portal [subnets](/networking/subnets){: external} and [VLANS](/networking/vlans){: external} or terraform [ibm_network_vlan](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/network_vlan){: external} and [ibm_network_vlan_spanning](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/network_vlan_spanning){: external}.
6.  Create Cloud service instances as required. Reference [Service Endpoints](/docs/account?topic=account-service-endpoints-overview) for additional details.
7.  Create Virtual Server Instance for deploying the Bastion Host either by the [portal](/gen1/infrastructure/provision/vs){: external} or terraform [ibm_compute_vm_instance](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/compute_vm_instance){: external}.

    Alternative: Bare metal can be deployed using the [portal](/gen1/infrastructure/provision/bm){: external} or terraform [ibm_compute_bare_metal](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/compute_bare_metal){: external}.

8.  Create Virtual Server Instance for deploying the proxy server either by the [portal](/gen1/infrastructure/provision/vs){: external} or terraform [ibm_compute_vm_instance](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/compute_vm_instance){: external}.

    Alternative: Bare metal can be deployed using the [portal](/gen1/infrastructure/provision/bm){: external} or terraform [ibm_compute_bare_metal](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/compute_bare_metal){: external}.

9.  Create Virtual Server Instance for deploying the Custom Domain Name System (DNS) server either by the [portal](/gen1/infrastructure/provision/vs){: external} or terraform [ibm_compute_vm_instance](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/compute_vm_instance){: external}.

    Alternative: Bare metal can be deployed using the [portal](/gen1/infrastructure/provision/bm){: external} or terraform [ibm_compute_bare_metal](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/compute_bare_metal){: external}.

10. Optionally Provision the [IBM Cloud Load Balancer](/docs/loadbalancer-service?topic=loadbalancer-service-configuring-ibm-cloud-load-balancer-basic-parameters) using the [portal](/catalog/infrastructure/load-balancer-group){: external} or Terraform [ibm_lb](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/lb){: external}.

    Alternative: Citrix VPX can be deployed using the [portal](/catalog/infrastructure/load-balancer-group){: external} or Terraform [ibm_lb_vpx](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/lb_vpx){: external}.

### Power Virtual Server
{: #PVS}

1.  Create the power virtual server workspace in the IBM cloud [portal](/power/create-workspace){: external}, or the terraform [ibm_pi_workspace](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/pi_workspace){: external}, or by using the [Terraform IBM Module](https://github.com/terraform-ibm-modules/terraform-ibm-powervs-workspace){: external}.
2.  Create the power virtual server instance in the IBM cloud [portal](/power/provisioning){: external}, or the terraform [ibm_pi_instance](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/pi_instances){: external}, or by using the [Terraform IBM Module](https://github.com/terraform-ibm-modules/terraform-ibm-powervs-instance){: external}.

### Cloud Internet Services (CIS)
{: #internet-services}

1.  Configure IBM Cloud Internet Services using the [portal](/catalog/services/internet-services?catalog_query=aHR0cHM6Ly9jbG91ZC5pYm0uY29tL2NhdGFsb2c%2Fc2VhcmNoPWNpcyNzZWFyY2hfcmVzdWx0cw%3D%3D){: external}, or the [Terraform IBM Module](https://github.com/terraform-ibm-modules/terraform-ibm-cis){: external}.

### Connecting Power Virtual Server to Classic
{: #cloud-connect}

1.  Provision (2) 5 Gbps Power Virtual Server [Cloud Connections](/docs/power-iaas?topic=power-iaas-cloud-connections#create-cloud-connections) with GRE enabled.

    Specify a GRE subnet, example 192.168.10.0/29, which will be used for GRE communication between Power and Classic. The Cloud Connection automation will assign the first IP of the specified subnet to the Power Gateway IP and the first IP of the second half of the subnet as the local GRE Tunnel IP in Power.{: note}

    Assigning 192.168.10.0/29 as the GRE subnet allows 8 available IPs. Automation would assign the Power Gateway IP as 192.168.10.1 and the PowerVS GRE tunnel IP of that Gateway as 192.168.10.5 (first IP of the second half of the subnet). The next IP (192.168.10.6) is used as the local GRE tunnel IP on your Gateway device in the Classic infrastructure.{: example}

    GRE “Keepalives” are enabled on the PowerVS side of the tunnel. The Classic Gateway must have this feature enabled to successfully establish the GRE tunnel.{: note}

2.  Establish GREc from the classic gateway of choice to power virtual server workspace. View a GRE configuration [example](/docs/power-iaas?topic=power-iaas-cloud-connections#gre-configuration-example) for reference.

    Configure BGP to peer your IBM Cloud Classic Gateway with PowerVS for route exchange over the GRE tunnel. Use the GRE tunnel IP addresses as the BGP neighbours.{: tip}
