# OpenStack Theory

## start

OpenStack is a cloud operating system. an IaaS server.



## components

### conceptual architecture

<img src="img/conceptual_architecture.png" style="zoom:50%;" />

### `Nova`

- manages provisioning your virtual machine in cloud.
- can be accessed through `Horizon`, `OpentStack` Client and `Nova` Client Python API.
- Nova requires `Keystone` (for authentication), `Glance` (for images) and `Neutron` (for networking) in order to work!
- `Nova` go to `Glance`, pick up the image and spin up the image

### `Cinder`

- provides persistent block storage. volume storage.

### `Neutron`

- networking
- IP address management
- security groups

### `Glance`

- image service
- when start a compute instance, actually the image will be copied from Glance to the instance for it to use.

### `Swift`

- like S3
- object storage to store static data such as images, videos or images of virtual machine
- provides a simple API for storing and retrieving data

### `Keystone`

- identity service, authentication and authorization

### `Horizon`

- UI, or dashboard

### `Heat`

- provides orchestration services
- used to deploy multiple complex cloud resources described in text files called templates (written in .yml). 
- The Heat Service implements an orchestration engine to parse and execute these templates.
- templates:
  - Intrinsic function: such as getting a value of the resource attribute, or referring to another resource within the same template.
  - Resource is used to declare instances, networks, ports, routers, firewall rules, etc.
  - Parameters is used to specify optional output.

### `Helm`

- platform that use to deploy Open Stack on the top of Kubernetes cluster. 
- Hierarchy: Hardware -> Kubernetes -> Helm -> Open Stack -> Components
- `Helm` will deploy the Open Stack automatically.

### components

- [main] virtual computing hardware: `NOVA`
- [main] virtual storage hardware: `Cinder`
- [main] virtual network hardware: `Neutron`
- `Horizon`

# Learn from Cisco Lab

## create and configure Ubuntu Instance in OpenStack

### create a security group

- enable ingress of all ICMP and TCP

  <img src="img/security_group.png" style="zoom:50%;" />

### create an internal network

- enable Admin State
- select a Class C network address, such as `192.168.2.0/24`, network should be 0 at the last bit.
- add a gateway IP, it is better to select `192.168.2.1`, when creating the router interface, this will be its IP address
- configure allocation pools, from `192.168.2.2` to `192.168.2.254`, exclude `192.168.2.1` since it has been allocated to the gateway.

### create a router

- add an interface

### create an image

### create an SSH KeyPair

1. ```shell
   ssh-keygen -t rsa -b 4096
   ```

2. ```shell
   cat ~/.ssh/id_rsa.pub
   ```

3. change the permission

   ```shell
   chmod 400 <id_rsa location>
   ```

### launch an Instance

- assemble your instance with your configured security group, network, router, image and SSH keypair.

### Assign a floating IP

- private IPs are assigned to the instance to connect between instances.
- public IP is used to communicate with the outer world, so that you can ping it from the platform where the OpenStack deployed in.
- what you need to do in HEAT template (`yaml` file)
  - create a floating IP, allocate an external network(public)
  - assign an IP address from your internal network to the port
  - associate the floating IP with the port

### connect to the instance from the platform

- ```shell
  ssh ubuntu@<Instance Floating IP> -i ~/.ssh/id_rsa
  ```

- to check the internet connectivity, try `ping 8.8.8.8`;

### firewall

- the access to the instance is not only block by the security group but also block by the `ufw` (uncomplicated firewall) in the instance, so don't forget to configure two "walls".

- enable `ufw`

  ```shell
  sudo ufw enable
  ```

- allow Nginx HTTP and SSH

  ```shell
  sudo ufw allow 'Nginx HTTP'
  
  sudo ufw allow 'OpenSSH'
  
  sudo ufw status	
  ```

- create a new rule to allow the HTTP connection in security group!

## configure by `.yaml`

### yaml

**YAML** (a [recursive acronym](https://en.wikipedia.org/wiki/Recursive_acronym) for "YAML Ain't Markup Language") is a [human-readable](https://en.wikipedia.org/wiki/Human-readable) [data-serialization language](https://en.wikipedia.org/wiki/Serialization). It is commonly used for [configuration files](https://en.wikipedia.org/wiki/Configuration_file) and in applications where data is being stored or transmitted.

### create an instance

```yaml
heat_template_version: 2017-02-24
description: Simple template to deploy a single compute instance
resources:
    my_instance:
        type: OS::Nova::Server
        properties:
            image: ubuntu_LSY
            flavor: default
            key_name: LSY-SSH
            networks:
                - network: LSY-network
```

### output

```yaml
description: Simple template to deploy a single compute instance
heat_template_version: '2017-02-24'

resources:
  my_instance:
    properties:
      flavor: default
      image: ubuntu_LSY
      key_name: LSY-SSH
      networks: 
      - network: LSY-network
    type: OS::Nova::Server
outputs:
  instance_ip:
    description: ip address of the deployed compute instance
    value:
      get_attr:
        - my_instance
        - first_address
```

### add parameters

```yaml
description: Simple template to deploy a single compute instance
heat_template_version: '2017-02-24'

parameters:
  flavor: 
    description: the flavor
    default: default
    type: string
  image:
    description: the image
    default: ubuntu_LSY
    type: string
  key_name:
    description: the ssh
    default: LSY-SSH
    type: string

resources:
  my_instance:
    properties:
      flavor:
        get_param: flavor
      image:
        get_param: image
      key_name:
        get_param: key_name
      networks: 
      - network: LSY-network
    type: OS::Nova::Server

outputs:
  instance_ip:
    description: ip address of the deployed compute instance
    value:
      get_attr:
        - my_instance
        - first_address
```

### advanced

```yaml
heat_template_version: 2017-02-24
description: Simple template to deploy a single compute instance
parameters:
  net:
    description: the network named LSY-network 192.168.2.0/24
    type: string
    default: LSY-network

resources:
  web_secgroup:
    type: OS::Neutron::SecurityGroup
    properties:
      rules:
        - protocol: tcp
          remote_ip_prefix: 0.0.0.0/0
          port_range_min: 22
          port_range_max: 22
        - protocol: ICMP
          remote_ip_prefix: 0.0.0.0/0

  instance_port_1:
    type: OS::Neutron::Port
    properties:
      network:
        get_param: net
      security_groups:
        - default
        - get_resource: web_secgroup
  instance_port_2:
    type: OS::Neutron::Port
    properties:
      network:
        get_param: net
      security_groups:
        - default
        - get_resource: web_secgroup
          
  instance_1:
    type: OS::Nova::Server
    properties:
      image: ubuntu_LSY
      flavor: default
      key_name: LSY-SSH
      networks:
        - port:
            get_resource: instance_port_1
  instance_2:
    type: OS::Nova::Server
    properties:
      image: ubuntu_LSY
      flavor: default
      key_name: LSY-SSH
      networks:
        - port:
            get_resource: instance_port_2

  floating_ip_1:
    type: OS::Neutron::FloatingIP
    properties:
      floating_network: public
  floating_ip_2:
    type: OS::Neutron::FloatingIP
    properties:
      floating_network: public

  association_1:
    type: OS::Neutron::FloatingIPAssociation
    properties:
      floatingip_id:
        get_resource: floating_ip_1
      port_id:
        get_resource: instance_port_1
  association_2:
    type: OS::Neutron::FloatingIPAssociation
    properties:
      floatingip_id:
        get_resource: floating_ip_2
      port_id:
        get_resource: instance_port_2

outputs:
  instance_ip:
    description: ip address of the deployed compute instance
    value:
      get_attr:
        - instance_1
        - first_address
```





# Questions

- if I want to create an instance, the type that I need to specify
  - OS::Nova::Server
- four properties that need to be specified in the instance
  - image
  - flavor
  - key_name
  - networks
- `get_param`, `get_attr`, `get_resource`
  - `get_param`: This value is resolved at runtime based on the inputs.
  - `get_attr`: attribute of the resource. This value is resolved at runtime using the resource instance created from the respective resource definition.
  - `get_resouces`: refers to a resource defined within the same template. resolved at runtime to the reference ID of the requested resource. 
- **YAML** (a [recursive acronym](https://en.wikipedia.org/wiki/Recursive_acronym) for "YAML Ain't Markup Language") is a [human-readable](https://en.wikipedia.org/wiki/Human-readable) [data-serialization language](https://en.wikipedia.org/wiki/Serialization). It is commonly used for [configuration files](https://en.wikipedia.org/wiki/Configuration_file) and in applications where data is being stored or transmitted.