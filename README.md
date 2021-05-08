# RKE2 cluster template

This project contains rke2 cluster template helm chart, which can be applied with values.yaml as configurations to create clusters.

### How to use

It is a standard helm chart which creates CR `clusters.provisioning.cattle.io` and nodeconfigs.

The general cluster configuration options are available through [values.yaml](./charts/values.yaml).

```yaml
# cluster specific values
cluster:
  # specify cluster name
  name: cluster-example

  # specify cluster labels
  labels: {}

  # specify cluster annotations
  annotations: {}

# specify cloud credential secret name, do not need to be provided if using custom driver
cloudCredentialSecretName: example

# specify cloud provider, options are amazonec2, digitalocean, azure, vsphere or custom
cloudprovider: ""

# enable network policy
enableNetworkPolicy: false

kubernetesVersion: "v1.21.0-alpha2+rke2r1"

# specify rancher helm chart values deployed into downstream cluster
rancherValues: {}

# specify extra env variables in cluster-agent deployment
# agentEnvs:
#  - name: HTTP_PROXY
#     value: foo.bar

# general RKE options
rke:
  # specify rancher helm chart values deployed into downstream cluster
  chartValues: {}

  # controlplane/etcd configuration settings
  controlPlaneConfig:
    # Path to the file that defines the audit policy configuration
    # audit-policy-file: ""
    # IPv4/IPv6 network CIDRs to use for pod IPs (default: 10.42.0.0/16)
    # cluster-cidr: ""
    # IPv4 Cluster IP for coredns service. Should be in your service-cidr range (default: 10.43.0.10)
    # cluster-dns: ""
    # Cluster Domain (default: "cluster.local")
    # cluster-domain: ""
    # CNI Plugin to deploy, one of none, canal, cilium (default: "canal")
    cni: calico
    # Do not deploy packaged components and delete any deployed components (valid items: rke2-coredns, rke2-ingress-nginx, rke2-kube-proxy, rke2-metrics-server)
    # disable: false
    # Disable automatic etcd snapshots
    # etcd-disable-snapshots: false
    # Expose etcd metrics to client interface. (Default false)
    # etcd-expose-metrics: false
    # Directory to save db snapshots. (Default location: ${data-dir}/db/snapshots)
    # etcd-snapshot-dir: ""
    # Set the base name of etcd snapshots. Default: etcd-snapshot-<unix-timestamp> (default: "etcd-snapshot")
    # etcd-snapshot-name: ""
    # Number of snapshots to retain (default: 5)
    # etcd-snapshot-retention: 5
    # Snapshot interval time in cron spec. eg. every 5 hours '* */5 * * *' (default: "0 */12 * * *")
    # etcd-snapshot-schedule-cron: "0 */12 * * *"
    # Customized flag for kube-apiserver process
    # kube-apiserver-arg: ""
    # Customized flag for kube-scheduler process
    # kube-scheduler-arg: ""
    # Customized flag for kube-controller-manager process
    # kube-controller-manager-arg: ""
    # Validate system configuration against the selected benchmark (valid items: cis-1.5, cis-1.6 )
    # profile: "cis-1.6"
    # Enable Secret encryption at rest
    # secrets-encryption: false
    # IPv4/IPv6 network CIDRs to use for service IPs (default: 10.43.0.0/16)
    # service-cidr: "10.43.0.0/16"
    # Port range to reserve for services with NodePort visibility (default: "30000-32767")
    # service-node-port-range: "30000-32767"
    # Add additional hostnames or IPv4/IPv6 addresses as Subject Alternative Names on the server TLS cert
    # tls-san: []

  # worker configuration settings
  workerConfig:
  - config:
      # Node name
      # node-name: ""
      # Disable embedded containerd and use alternative CRI implementation
      # container-runtime-endpoint: ""
      # Override default containerd snapshotter (default: "overlayfs")
      # snapshotter: ""
      # IP address to advertise for node
      # node-ip: "1.1.1.1"
      # Kubelet resolv.conf file
      # resolv-conf: ""
      # Customized flag for kubelet process
      # kubelet-arg: ""
      # Customized flag for kube-proxy process
      # kube-proxy-arg: ""
      # Kernel tuning behavior. If set, error if kernel tunables are different than kubelet defaults. (default: false)
      # protect-kernel-defaults: false
      # Enable SELinux in containerd (default: false)
      # selinux: true
      # Cloud provider name
      # cloud-provider-name: ""
      # Cloud provider configuration file path
      # cloud-provider-config: ""
    machineLabelSelector:
      matchLabels:
        foo: bar

  # enable local auth endpoint
  localClusterAuthEndpoint: 
    enabled: false
  # specify fqdn of local access endpoint
  # fqdn: foo.bar.example
  # specify cacert of local access endpoint
  # caCerts: ""

  # Specify upgrade options
  upgradeStrategy: 
    controlPlaneDrainOptions: 
      enabled: false
      # deleteEmptyDirData: false
      # disableEviction: false
      # gracePeriod: 0
      # ignoreErrors: false
      # skipWaitForDeleteTimeoutSeconds: 0
      # timeout: 0
    workerDrainOptions:
      enabled: false
      # deleteEmptyDirData: false
      # disableEviction: false
      # gracePeriod: 0
      # ignoreErrors: false
      # skipWaitForDeleteTimeoutSeconds: 0
      # timeout: 0
    workerConcurrency: "1"

# Specify nodepool options. Can add multiple node groups, specify etcd, controlplane and worker roles. Not needed if using custom driver.
# nodepools:
#
#   ==== amazonec2 driver example ====
#
# - etcd: true
#   controlplane: true
#   worker: true
#   specify node labels
#   labels: {}
#   specify node taints
#   taints: {}

#   specify nodepool size
#   quantity: 1

#   specify nodepool name
#   name: ec2-nodepool-1

#   AWS machine image
#   ami: ""

#   AWS spot instance duration in minutes (60, 120, 180, 240, 300, or 360)
#   blockDurationMinutes: 0

#   AWS root device name
#   deviceName: "/dev/sda1"

#   Encrypt the EBS volume using the AWS Managed CMK
#   encryptEbsVolume: false
  
#   Optional endpoint URL (hostname only or fully qualified)
#   endpoint: ""

#   AWS IAM Instance Profile
#   iamInstanceProfile: ""

#   Disable SSL when sending requests
#   insecureTransport: false

#   AWS instance type
#   instanceType: t3a.medium

#   AWS region
#   region: us-west-2

#   Whether to create `rancher-node` security group. If false, can provide with existing security group
#   createSecurityGroup: true
#   createSecurityGroup: false
#   securityGroups: []

#   AWS keypair to use
#   keypairName: ""

#   Skip adding default rules to security groups
#   securityGroupReadonly: false

#   File contents for sshKeyContents
#   sshKeyContents: ""

#   AWS VPC subnet id
#   subnetId: ""

#   Set this flag to enable CloudWatch monitoring
#   monitoring; false

#   Make the specified port number accessible from the Internet
#   openPort: ["8080", "8443"]

#   Only use a private IP address
#   privateAddressOnly: false

#   Set this flag to request spot instance
#   requestSpotInstance: false

#   AWS Tags (e.g. key1,value1,key2,value2)
#   tags: "foo,bar"

#   Set retry count for recoverable failures (use -1 to disable)
#   retries: 5

#   AWS root disk size (in GB)
#   rootSize: 16

#   AWS spot instance bid price (in dollar)
#   spotPrice: 0.5

#   Set the name of the ssh user
#   sshUser: ubuntu

#   Amazon EBS volume type
#   volumeType: gp2

#   AWS VPC id
#   vpcId: ""

#   Create an EBS optimized instance
#   useEbsOptimizedInstance: false

#   Force the usage of private IP address
#   usePrivateAddress: false

#   File contents for userdata
#   userdata: ""

#   AWS zone for instance (i.e. a,b,c,d,e)
#   zone: a
#
#   ==== vsphere driver example ====
#
# - etcd: true
#   controlplane: true
#   worker: true

#   specify node labels
#   labels: {}

#   specify node taints
#   taints: {}

#   specify nodepool size
#   quantity: 1

#   specify nodepool name
#   name: vsphere-nodepool-1

#   vSphere vm configuration parameters (used for guestinfo)
#   cfgparam: []

#   If you choose creation type clone a name of what you want to clone is required
#   cloneFrom: ""

#   Filepath to a cloud-config yaml file to put into the ISO user-data
#   cloudConfig: ""

#   vSphere cloud-init filepath or url to add to guestinfo, filepath will be read and base64 encoded before adding
#   cloudinit: ""

#   If you choose to clone from a content library template specify the name of the library
#   contentLibrary: ""

#   vSphere CPU number for docker VM
#   cpuCount: "2"

#   'Creation type when creating a new virtual machine. Supported values: vm, template, library, legacy'
#   creationType: "vm"

#   vSphere custom attribute, format key/value e.g. '200=mycustom value'
#   customAttribute: ["200=mycustom value"]

#   vSphere datacenter for virtual machine
#   datacenter: ""

#   vSphere datastore for virtual machine
#   datastore: ""

#   vSphere datastore cluster for virtual machine
#   datastoreCluster: ""

#   vSphere size of disk for docker VM (in MB)
#   diskSize: "20480"

#   vSphere folder for the docker VM. This folder must already exist in the datacenter
#   folder: ""

#   vSphere compute resource where the docker VM will be instantiated. This can be omitted if using a cluster with DRS
#   hostsystem: ""

#   vSphere size of memory for docker VM (in MB)
#   memorySize: "2048"

#   vSphere network where the virtual machine will be attached
#   network: ""

#   vSphere resource pool for docker VM
#   pool: ""

#   If using a non-B2D image you can specify the ssh port
#   sshPort: "22"

#   If using a non-B2D image the uploaded keys will need chown'ed, defaults to staff e.g. docker:staff
#   sshUserGroup: staff

#   vSphere tag id e.g. urn:xxx
#   tag: ["urn:xxx"]

#   'vSphere vApp IP allocation policy. Supported values are: dhcp, fixed, transient and fixedAllocated'
#   vappIpallocationpolicy: ""

#   'vSphere vApp IP protocol for this deployment. Supported values are: IPv4 and IPv6'
#   vappIpprotocol: ""

#   vSphere vApp properties
#   vappProperty: []

#   'vSphere OVF environment transports to use for properties. Supported values are: iso and com.vmware.guestInfo'
#   vappTransport: ""

#   vSphere IP/hostname for vCenter
#   vcenter: ""

#   vSphere Port for vCenter
#   vcenterPort: 443
#
#   ==== Digitalocean driver example ====
#
# - etcd: true
#   controlplane: true
#   worker: true

#   specify node labels
#   labels: {}

#   specify node taints
#   taints: {}

#   specify nodepool size
#   quantity: 1

#   # specify nodepool name
#   name: digitalocean-nodepool-1

#   enable backups for droplet
#   backups: true

#   Digital Ocean Image
#   image: ubuntu-16-04-x64

#   enable ipv6 for droplet
#   ipv6: false

#   enable monitoring for droplet
#   monitoring: false

#   enable private networking for droplet
#   privateNetworking: false

#   Digital Ocean region
#   region: nyc3

#   Digital Ocean size
#   size: s-1vcpu-1gb

#   File contents for sshKeyContents
#   sshKeyContents: ""

#   SSH key fingerprint
#   sshKeyFingerprint: ""

#   SSH port
#   sshPort: 22

#   SSH username
#   sshUser: root

#   comma-separated list of tags to apply to the Droplet
#   tags: ""

#   File contents for userdata
#   userdata: "" 
```

To provide your own configuration, modify the original values.yaml and create your own version, and pass it to helm. For example:

```bash
helm install --namespace fleet-default --values ./charts/your-own-values.yaml do-cluster ./charts
```

For different cloud provider options on nodepools, checkout

[Amazonec2](./charts/values-aws.yaml)

[DigitalOcean](./charts/values-do.yaml)

[Vsphere](./charts/values-vsphere.yaml)

### Version control

The version control is implemented as helm release history and can easily be implemented by UI to provide operation history and rollback.

# License

Copyright (c) 2014-2021 [Rancher Labs, Inc.](http://rancher.com)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

[http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.