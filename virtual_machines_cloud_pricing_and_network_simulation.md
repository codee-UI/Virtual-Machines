# Virtual Machines, Cloud Computing, and Virtual Network Simulation

## Table of Contents

1. [Introduction to Virtualization](#1-introduction-to-virtualization)
2. [Virtual Machines and Containers](#2-virtual-machines-and-containers)
3. [Virtual Machines by Operating System](#3-virtual-machines-by-operating-system)
4. [Virtual Machines by Resource Allocation](#4-virtual-machines-by-resource-allocation)
5. [Cloud Virtual Machine Pricing Models](#5-cloud-virtual-machine-pricing-models)
6. [Virtual Network Architecture](#6-virtual-network-architecture)
7. [Exercise: Virtual Network Simulation](#7-exercise-virtual-network-simulation)
8. [Security Considerations](#8-security-considerations)
9. [Practical Lab](#9-practical-lab)
10. [Knowledge Check](#10-knowledge-check)
11. [Key Takeaways](#11-key-takeaways)
12. [Glossary](#12-glossary)

---

# 1. Introduction to Virtualization

**Virtualization** is a technology that allows software to create virtual versions of computing resources, including servers, operating systems, storage devices, and network components.

Instead of dedicating one physical server to a single workload, virtualization can allow multiple isolated virtual machines to share the same physical hardware.

## 1.1 What Is a Virtual Machine?

A **virtual machine (VM)** is a software-defined computer that runs its own operating system and applications using virtualized hardware resources.

A VM typically has:

- Virtual CPUs (vCPUs).
- Allocated memory (RAM).
- Virtual storage disks.
- Virtual network interfaces.
- A guest operating system.
- Applications and services.

### Example

A physical server might host three virtual machines:

- VM 1: Linux web server.
- VM 2: Linux database server.
- VM 3: Linux server hosting containerized applications.

Each VM operates as a separate computing environment, although all three share the underlying physical server.

## 1.2 How Virtualization Works

A virtualization layer manages access to the physical server's resources.

```text
+------------------------------------------------+
|             Physical Server                    |
|                                                |
|  CPU | RAM | Storage | Network Interfaces      |
|                                                |
|  +------------------------------------------+  |
|  | Hypervisor                               |  |
|  +------------------------------------------+  |
|         |              |             |         |
|         v              v             v         |
|  +-------------+ +-------------+ +----------+  |
|  | VM 1        | | VM 2        | | VM 3     |  |
|  | Linux       | | Linux       | | Windows  |  |
|  | Web Server  | | Database    | | App      |  |
|  +-------------+ +-------------+ +----------+  |
+------------------------------------------------+
```

A **hypervisor** is the software layer that creates and manages virtual machines.

## 1.3 Types of Hypervisors

### Type 1: Bare-Metal Hypervisor

A Type 1 hypervisor runs directly on physical hardware.

Examples include:

- VMware ESXi.
- Microsoft Hyper-V in its hypervisor role.
- Xen.
- KVM-based virtualization environments.

Common use cases:

- Enterprise server virtualization.
- Cloud infrastructure.
- Data centers.
- Production workloads.

### Type 2: Hosted Hypervisor

A Type 2 hypervisor runs as an application on a host operating system.

Examples include:

- Oracle VirtualBox.
- VMware Workstation.
- VMware Fusion.

Common use cases:

- Learning and training.
- Software development.
- Testing operating systems.
- Cybersecurity laboratories.
- Network simulation.

## 1.4 Benefits of Virtualization

- Better utilization of physical hardware.
- Isolation between workloads.
- Faster provisioning of servers.
- Easier creation of test environments.
- Support for snapshots and recovery workflows.
- Simplified deployment and migration.
- Flexible allocation of computing resources.

## 1.5 Limitations

- VMs consume physical CPU, RAM, storage, and network resources.
- Too many VMs can cause resource contention.
- A physical host failure may affect multiple VMs.
- Licensing and cloud costs may increase with resource usage.
- Misconfigured virtual networking can expose systems to unauthorized access.

---

# 2. Virtual Machines and Containers

Virtual machines and containers are related but different technologies.

## 2.1 What Is a Container?

A **container** packages an application and its required dependencies into an isolated environment.

Containers generally share the host operating system's kernel rather than running a complete independent guest operating system.

Docker is a widely used container platform. Kubernetes is a platform for orchestrating containerized applications across systems.

## 2.2 VM vs. Container

| Feature | Virtual Machine | Container |
|---|---|---|
| Operating system | Usually includes a guest OS | Usually shares the host kernel |
| Isolation | Virtualized hardware and guest OS boundary | Process and namespace-based isolation, depending on configuration |
| Resource overhead | Generally higher | Generally lower |
| Startup | Often slower | Often faster |
| Best suited for | Different OS environments and isolated servers | Portable application deployment |
| Common tools | Hyper-V, VirtualBox, KVM | Docker, containerd, Kubernetes |
| Kernel | Guest kernel | Host kernel, in typical container setups |

Containers are not automatically secure. Their isolation depends on the runtime, kernel, permissions, configuration, and host security.

## 2.3 Containers Inside a VM

A VM can host a container runtime and multiple containers.

```text
Physical Server
      |
      v
Hypervisor
      |
      v
Linux Virtual Machine
      |
      v
Container Runtime
      |
      +------------------+
      |                  |
      v                  v
Container 1          Container 2
Web API              Background Worker
```

This architecture combines VM-level isolation with the deployment flexibility of containers.

---

# 3. Virtual Machines by Operating System

Cloud providers and virtualization platforms offer different guest operating systems. Availability depends on the provider, region, hardware, licensing, and platform restrictions.

## 3.1 Windows Virtual Machines

Windows VMs run a supported version of Microsoft Windows as their guest operating system.

### Common uses

- Hosting Windows-specific applications.
- Running Windows Server services.
- Testing software compatibility.
- Providing remote desktop environments.
- Supporting virtual desktop infrastructure (VDI).
- Hosting applications that require Windows-based dependencies.

### Security considerations

- Apply Windows security updates.
- Enable appropriate endpoint protection.
- Restrict Remote Desktop access.
- Use strong authentication and MFA where supported.
- Avoid exposing management services directly to the internet.
- Configure backups and recovery procedures.

## 3.2 Linux Virtual Machines

Linux VMs run distributions such as:

- Ubuntu.
- Debian.
- Red Hat Enterprise Linux (RHEL).
- SUSE Linux Enterprise.
- Supported CentOS-family distributions.
- Kali Linux for authorized security testing.

### Common uses

- Web servers.
- Database servers.
- Application hosting.
- Cloud-native applications.
- Containers.
- Firewalls and VPN services.
- Network monitoring.
- Security research and testing.

### Security considerations

- Keep packages and the kernel updated.
- Use SSH keys or other secure authentication methods.
- Disable unnecessary services.
- Apply least privilege.
- Configure firewall rules.
- Monitor system logs.
- Back up essential data and configurations.

**Note:** Kali Linux is primarily intended for penetration testing and security auditing. It is not generally the preferred default operating system for a routine production web server.

## 3.3 macOS Virtual Machines

macOS virtualization supports development and testing for Apple's software ecosystem, subject to Apple's licensing terms and the virtualization platform's technical requirements.

### Common uses

- macOS application testing.
- Development for Apple platforms.
- Compatibility testing.
- Controlled build environments.

macOS virtualization availability is more restricted than typical Windows and Linux virtualization. Not every cloud provider offers general-purpose macOS VMs, and licensing and hardware requirements must be checked.

---

# 4. Virtual Machines by Resource Allocation

Cloud providers offer VM families designed for different workload characteristics.

The best choice depends on CPU demand, memory requirements, storage performance, network throughput, and budget.

## 4.1 General-Purpose VMs

General-purpose VMs provide a balanced combination of CPU and memory.

### Suitable workloads

- Small and medium web applications.
- Application servers.
- Development environments.
- Small databases.
- Internal business applications.
- Lightweight services.

Choose this category when a workload does not have a strong CPU, memory, or storage bottleneck.

## 4.2 Compute-Optimized VMs

Compute-optimized VMs emphasize CPU performance relative to memory.

### Suitable workloads

- Scientific calculations.
- Batch processing.
- High-performance computing.
- CPU-based data processing.
- Video encoding.
- Simulation workloads.

These VMs are useful when the main bottleneck is CPU processing rather than memory capacity or storage throughput.

## 4.3 Memory-Optimized VMs

Memory-optimized VMs provide comparatively large amounts of RAM.

### Suitable workloads

- In-memory databases.
- Memory-intensive analytics.
- Large relational databases.
- Real-time data processing.
- Graph processing.
- Applications that maintain large working sets in memory.

These VMs are appropriate when workloads are constrained by available RAM.

## 4.4 Storage-Optimized VMs

Storage-optimized VMs emphasize storage throughput, input/output operations per second (IOPS), or access to high-performance local storage.

### Suitable workloads

- Large databases.
- NoSQL databases.
- Log processing.
- Data warehousing.
- Distributed storage.
- High-throughput data processing.

**Important:** Storage performance depends on the VM family, disk type, provisioned performance, caching, and platform limits.

## 4.5 GPU-Optimized VMs

GPU-optimized VMs provide access to graphics processing units (GPUs).

GPUs can execute many operations in parallel and are useful for supported workloads.

### Suitable workloads

- Machine learning.
- Deep learning.
- Large-scale numerical computation.
- 3D rendering.
- Scientific computing.
- Graphics-intensive applications.

### Considerations

- GPU instances are often more expensive than general-purpose VMs.
- Not every ML task benefits from a GPU.
- CPU, RAM, storage, and data transfer can still become bottlenecks.
- GPU memory capacity can limit the size of models and batches.

For small learning projects, a CPU-based environment may be sufficient. GPU resources become more useful as model size and computational demand increase.

## 4.6 Burstable VMs

Burstable VMs provide a baseline level of CPU performance and can temporarily use additional CPU capacity under the provider's bursting rules.

Some models use CPU credits or a similar mechanism.

### Suitable workloads

- Development and testing.
- Small web servers.
- Low-traffic applications.
- Lightweight APIs.
- Intermittent workloads.

**Important correction:** Burstable performance is not necessarily free beyond the baseline. Pricing, credit accumulation, throttling, and surplus usage depend on the specific VM family and provider.

## 4.7 Resource-Category Comparison

| VM Category | Main Resource Focus | Example Workload |
|---|---|---|
| General-purpose | Balanced CPU and RAM | Small web application |
| Compute-optimized | CPU performance | Scientific computation |
| Memory-optimized | RAM capacity | In-memory database |
| Storage-optimized | Storage throughput and IOPS | High-volume data processing |
| GPU-optimized | Parallel GPU computation | Deep learning |
| Burstable | Variable CPU demand | Low-traffic service |

---

# 5. Cloud Virtual Machine Pricing Models

Cloud providers offer several purchasing and pricing options. Names, discounts, and eligibility requirements change over time, so always check the current provider documentation before purchasing resources.

## 5.1 Pay-As-You-Go

Pay-as-you-go billing charges for resources according to the provider's billing rules and the time resources are allocated or consumed.

### Advantages

- No long-term commitment for standard on-demand usage.
- Suitable for experiments and short-term projects.
- Flexible capacity.
- Easy to increase or decrease resources.

### Disadvantages

- Costs can be unpredictable.
- Continuous workloads may be more expensive than eligible committed-use options.
- Supporting resources may create additional charges.

A VM's total cost may include:

- Compute.
- Managed or attached disks.
- Public IP addresses.
- Network egress.
- Backups and snapshots.
- Monitoring and logging.
- Licensing.

Stopping a VM may stop some compute charges but does not necessarily stop disk, IP, backup, or other associated charges. Verify the provider's billing behavior.

## 5.2 Reserved or Committed-Use Pricing

Cloud providers may offer reduced prices in exchange for a commitment to a defined amount of usage or spending over a specified period.

Azure offers commitment-based options, including reservations for eligible resources and other savings mechanisms.

### Advantages

- Potential savings for predictable workloads.
- Useful for long-running production systems.
- Can reduce the cost of stable compute usage.

### Disadvantages

- Commitment can limit flexibility.
- Actual savings depend on the eligible VM, region, usage, and offer.
- Unused commitments may not deliver expected savings.

**Note:** Azure reservation products and savings plans have different rules. Do not assume all reservation discounts apply identically to every VM or supporting resource.

## 5.3 Azure Spot Virtual Machines

Spot VMs use Azure's available spare compute capacity and can be discounted compared with standard on-demand pricing.

However, Azure can evict Spot VMs when capacity is needed or when configured eviction conditions are met.

### Suitable workloads

- Interruptible batch jobs.
- Development and testing.
- Distributed computations that can retry.
- Noncritical processing.
- Fault-tolerant workloads.

### Avoid relying on Spot VMs for

- Critical services requiring uninterrupted operation.
- Workloads that cannot recover from interruptions.
- Single instances hosting essential data without appropriate persistence and backup.

Design Spot workloads to checkpoint progress and tolerate interruption.

## 5.4 Azure Hybrid Benefit

Azure Hybrid Benefit can reduce eligible Azure costs when an organization applies qualifying existing Windows Server or SQL Server licenses under the program's terms.

### Considerations

- Eligibility depends on the license and subscription.
- Licensing conditions must be followed.
- Not every operating system or software license qualifies.
- Potential savings depend on the workload and licensing position.

## 5.5 Azure Dev/Test Offers

Microsoft provides development and testing benefits through eligible programs and subscriptions.

Depending on the current offer, users may qualify for specific pricing, credits, or development benefits.

Do not assume that every Visual Studio subscription includes the same credits or that all VM charges are covered. Verify current eligibility, subscription terms, and credit limits.

## 5.6 Understanding Azure VM Families

Azure VM family names indicate broad workload categories, but exact availability and specifications vary.

Examples include:

| Family Example | General Purpose |
|---|---|
| B-series | Burstable workloads |
| D-series | General-purpose workloads |
| F-series | Compute-optimized workloads |
| E-series | Memory-optimized workloads |
| L-series | Storage-optimized workloads |
| N-series | GPU-enabled workloads |

These are broad examples rather than a complete or permanent catalog. Check the current Azure documentation for exact specifications, processor models, regional availability, and pricing.

## 5.7 Cost-Estimation Checklist

Before deploying a cloud VM, estimate:

1. Number of VMs.
2. vCPU and RAM requirements.
3. Expected runtime.
4. Operating system licensing.
5. Storage capacity and performance.
6. Backup requirements.
7. Expected outbound network traffic.
8. Public IP and load-balancing requirements.
9. Monitoring and security services.
10. Recovery and availability requirements.

For learning projects, delete unused resources when they are no longer needed, after confirming that important data has been backed up.

---

# 6. Virtual Network Architecture

A virtual network connects VMs, containers, virtual firewalls, and other virtualized components.

A well-designed network should permit required communication while restricting unnecessary access.

## 6.1 Components of a Virtualized Network

### Physical server

The physical server provides the CPU, RAM, storage, and physical network interfaces.

### Hypervisor

The hypervisor creates and manages the virtual machines.

### Virtual machine

A VM provides an isolated operating system environment for a workload, such as a web server or database server.

### Virtual switch

A **virtual switch** connects virtual network interfaces to each other and, when configured, to physical network interfaces.

It may support:

- VM-to-VM communication.
- VLANs.
- Network segmentation.
- Connections to physical networks.
- Traffic filtering, depending on the platform.

A virtual switch does not necessarily assign IP addresses by itself. IP addresses are usually configured inside guest operating systems or assigned through DHCP services.

### Virtual firewall

A virtual firewall applies security rules to traffic flowing through the interfaces or zones it protects.

Its placement matters: a firewall can only inspect the traffic that actually passes through it.

### Containers

Containers run applications in isolated environments. They may connect through virtual interfaces, bridges, or overlay networks, depending on the container platform.

## 6.2 Example: Web Application Architecture

Suppose a company wants to host:

- A public-facing web server.
- A database server.
- A VM running two containers for additional services.
- A firewall to protect the environment.
- A virtual switch to connect the virtual components.

A possible logical design is:

```text
                         INTERNET
                            |
                            v
                  +-------------------+
                  | External Firewall |
                  | or Security Edge  |
                  +-------------------+
                            |
                            v
+--------------------------------------------------+
|                 PHYSICAL SERVER                  |
|                                                  |
|  +--------------------------------------------+  |
|  | Hypervisor / Virtualization Platform       |  |
|  |                                            |  |
|  |   +----------------+                       |  |
|  |   | Web Server VM  |                       |  |
|  |   +----------------+                       |  |
|  |                                            |  |
|  |   +----------------+                       |  |
|  |   | Database VM    |                       |  |
|  |   +----------------+                       |  |
|  |                                            |  |
|  |   +----------------+                       |  |
|  |   | Container Host |                       |  |
|  |   | VM             |                       |  |
|  |   | +------------+ |                       |  |
|  |   | | Container 1| |                       |  |
|  |   | +------------+ |                       |  |
|  |   | +------------+ |                       |  |
|  |   | | Container 2| |                       |  |
|  |   | +------------+ |                       |  |
|  |   +----------------+                       |  |
|  |                                            |  |
|  |   +------------------------------------+   |  |
|  |   | Virtual Switch / Virtual Networks  |   |  |
|  |   +------------------------------------+   |  |
|  +--------------------------------------------+  |
+--------------------------------------------------+
```

This is a conceptual illustration, not a complete deployment diagram. The virtual switch and firewall placement must reflect the actual traffic paths.

## 6.3 Recommended Security-Zone Design

For a stronger design, separate public-facing services from sensitive internal systems.

```text
                          INTERNET
                              |
                              v
                    +-------------------+
                    | Perimeter Firewall|
                    +-------------------+
                              |
                              v
                     +----------------+
                     |   Web Server   |
                     |   DMZ / Zone   |
                     +----------------+
                              |
                    Only required application
                    connections are permitted
                              |
                              v
                     +----------------+
                     | Database Server|
                     | Private Zone  |
                     +----------------+

                    +----------------+
                    | Container Host |
                    | Service Zone   |
                    +----------------+
```

The container host's exact placement depends on what the containers do and which systems they must reach. It should not automatically receive unrestricted access to the database or public internet.

## 6.4 Important Design Principles

- Public users should access only the services intended to be public.
- The database should not normally be exposed directly to the internet.
- The web server should connect to the database only on required ports.
- Container services should have only the network access they require.
- Management interfaces should be isolated from public traffic.
- Security controls should be applied to both inbound and outbound traffic where appropriate.
- Logs should be collected and reviewed.
- Backups should be protected against unauthorized modification.

---

# 7. Exercise: Virtual Network Simulation

## 7.1 Scenario

A company plans to host a web application on a physical server connected to the internet.

The server will contain:

1. A VM for the web server.
2. A VM for the database server.
3. A VM hosting two containers for additional application services.
4. A firewall to protect network traffic.
5. A virtual switch to connect the virtual network components.

## 7.2 Objective

Create a network diagram showing the physical server, hypervisor, virtual machines, containers, virtual switch, firewall, and internet connection.

The diagram should demonstrate the expected communication paths and the location of security controls.

## 7.3 Tools

You can use:

- Figma.
- diagrams.net (draw.io).
- LibreOffice Draw.
- PowerPoint.
- Other diagramming tools.

Figma is optional; the exercise is about network design rather than proficiency with one particular design application.

## 7.4 Step-by-Step Instructions

### Step 1: Draw the physical server

Create a large rectangle labelled:

`Physical Server`

This rectangle represents the hardware that hosts the virtualized components.

### Step 2: Add the hypervisor

Inside the physical server, draw a section labelled:

`Hypervisor / Virtualization Platform`

Place the virtual machines inside this section.

### Step 3: Add the web server VM

Create a VM icon labelled:

`Web Server VM`

This VM hosts the web application or web server software.

### Step 4: Add the database VM

Create a second VM icon labelled:

`Database Server VM`

The database should be placed in a private security zone or otherwise protected by appropriate access controls.

### Step 5: Add the container-host VM

Create a third VM labelled:

`Container Host VM`

Inside it, add two smaller components:

- `Container 1: Application Service`
- `Container 2: Background Worker`

Connect the container icons to the container host to show that they run within that VM.

### Step 6: Add the virtual switch

Add a component labelled:

`Virtual Switch / Virtual Networks`

Connect the VM network interfaces to the appropriate virtual network or networks.

For a more secure design, represent separate logical networks or VLANs for public-facing services, internal databases, and management traffic.

### Step 7: Add the firewall

Place the firewall at the point where the traffic it protects must pass through it.

For example, a perimeter firewall can control traffic entering from the internet. Internal segmentation controls can separately restrict communication between the web, database, and container-service zones.

**Important:** A firewall does not automatically protect every VM simply because its icon appears inside the physical server. The network must be configured so that the relevant traffic traverses the firewall or is subject to an equivalent enforcement mechanism.

### Step 8: Add the internet

Place an internet icon outside the physical server boundary.

Connect the internet to the perimeter firewall or the relevant external network interface.

### Step 9: Add communication labels

Label important traffic flows, for example:

- Internet to web server: HTTPS, TCP 443.
- Web server to database: only the required database protocol and port.
- Web server to application service: only the required service port.
- Administrator to management interface: authorized management access.

These are examples. Actual ports and protocols must match the application's design.

### Step 10: Add security-zone labels

Label the relevant zones:

- Public / DMZ.
- Private database network.
- Application services network.
- Management network.

### Step 11: Review the diagram

Check that:

- The web server can receive the intended public traffic.
- The database is not directly exposed to the internet.
- Only required connections are permitted.
- The container host is connected to the correct network.
- The firewall is placed on the traffic paths it is intended to protect.
- Administrative access is restricted.

## 7.5 Expected Diagram Checklist

Your diagram should contain all of the following:

- [ ] Internet.
- [ ] Physical server.
- [ ] Hypervisor or virtualization platform.
- [ ] Web server VM.
- [ ] Database server VM.
- [ ] Container-host VM.
- [ ] Two containers inside the container host.
- [ ] Virtual switch or virtual networks.
- [ ] Firewall.
- [ ] Connections between relevant components.
- [ ] Network or security-zone labels.
- [ ] Labels for important communication paths.

## 7.6 Validation Questions

After completing the diagram, answer these questions:

1. Can an internet user connect directly to the database?
2. Which device or control filters incoming internet traffic?
3. Which component hosts the two containers?
4. What connects the VM network interfaces?
5. Which systems need access to the database?
6. How would you restrict access to the management interface?
7. What would happen if the physical server failed?
8. How could you monitor unauthorized connection attempts?

---

# 8. Security Considerations

## 8.1 Isolation

VMs provide separate guest operating system environments, but isolation is not absolute. Hypervisor vulnerabilities, insecure virtual networking, excessive permissions, or shared-resource weaknesses can create risks.

## 8.2 Network Security

- Restrict unnecessary inbound and outbound traffic.
- Use separate networks for different trust levels.
- Prevent direct public access to private databases.
- Protect management interfaces.
- Monitor firewall and system logs.
- Review network rules periodically.

## 8.3 VM Security

- Apply operating system updates.
- Disable unnecessary services.
- Use secure authentication.
- Configure host-based firewalls.
- Protect VM images and snapshots.
- Encrypt sensitive storage where appropriate.
- Maintain tested backups.

## 8.4 Container Security

- Use trusted container images.
- Keep images and runtimes updated.
- Avoid running containers as root unless required and appropriately constrained.
- Restrict container capabilities and privileges.
- Protect secrets.
- Limit container network access.
- Scan images for known vulnerabilities.
- Avoid mounting sensitive host filesystems unnecessarily.

## 8.5 Availability and Recovery

If several VMs run on a single physical server, the physical host can become a single point of failure.

Depending on the required availability, consider:

- Reliable backups.
- Recovery procedures.
- Replication.
- Redundant hosts.
- Tested restoration.
- Monitoring of host resources.
- Capacity planning.

A snapshot is not a substitute for an independent backup.

---

# 9. Practical Lab

## Lab: Build a Small Virtualized Network

### Objective

Build and validate a small virtualized environment using available hardware and software.

### Suggested components

- One host computer.
- A hypervisor such as VirtualBox or an available native virtualization platform.
- One Linux VM for a web server.
- A second Linux VM for a database or test service, if hardware permits.
- An isolated virtual network.
- Optional containers hosted inside one VM.

### Procedure

1. Install or configure the virtualization platform.
2. Create the web server VM.
3. Configure its virtual network interface.
4. Create a second VM or a lightweight test service.
5. Connect the systems through the intended virtual network.
6. Configure host and network firewall rules.
7. Permit only the communication required for the lab.
8. Test connectivity between the intended endpoints.
9. Verify that prohibited traffic is blocked.
10. Inspect relevant system and firewall logs.
11. Record the configuration and test results.
12. Shut down the VMs and verify that no unintended background services remain running.

### Validation table

| Test | Expected Result |
|---|---|
| Intended host-to-web-service connection | Successful |
| Unauthorized connection to a blocked service | Denied or rejected according to policy |
| Web server access to an unneeded database port | Blocked |
| Firewall logging for a configured test rule | Evidence available |
| Restart of a VM | Configuration persists as expected |
| Recovery from a saved backup | Verified using a controlled test, when included in scope |

Use only systems and networks you own or have permission to administer.

---

# 10. Knowledge Check

## Questions

### Q1. What is a virtual machine?

A. A physical network cable  
B. A software-defined computer with virtualized resources  
C. A type of firewall rule  
D. A database query

### Q2. What does a hypervisor do?

A. Creates and manages virtual machines  
B. Automatically removes every malware infection  
C. Replaces all physical network equipment  
D. Assigns public IP addresses in every configuration

### Q3. Which statement best describes a typical container?

A. It always includes a complete independent guest kernel  
B. It is a physical server  
C. It packages an application and dependencies while typically sharing the host kernel  
D. It cannot communicate over a network

### Q4. Which VM category is generally suitable for a memory-intensive database?

A. Memory-optimized  
B. Compute-optimized in every case  
C. Burstable in every case  
D. GPU-optimized in every case

### Q5. What is an important characteristic of a Spot VM?

A. It is guaranteed to run continuously  
B. It may be interrupted or evicted  
C. It cannot use storage  
D. It does not require security controls

### Q6. What does a virtual switch do?

A. Connects virtual network interfaces  
B. Replaces the guest operating system  
C. Encrypts every file on a VM  
D. Automatically prevents all lateral movement

### Q7. Where should a database server normally be placed in a secure web application architecture?

A. Directly on the public internet with unrestricted access  
B. In a protected private network or security zone  
C. In the same unrestricted public zone as every user  
D. Outside all access controls

### Q8. Why might a cloud VM continue to incur costs after it is stopped?

A. Attached storage or other resources may still be billable  
B. Stopping a VM always doubles its compute price  
C. Every stopped VM automatically becomes a GPU instance  
D. The operating system always continues using full CPU capacity

### Q9. What is a potential risk of hosting several VMs on one physical server?

A. All VMs must use the same IP address  
B. A physical host failure may affect every VM on that host  
C. The VMs cannot run different applications  
D. The virtual switch always disables networking

### Q10. Why is firewall placement important in a virtual network?

A. A firewall can only enforce controls on traffic that passes through it or is otherwise subject to its enforcement  
B. A firewall protects all systems regardless of network design  
C. A firewall replaces the hypervisor  
D. A firewall automatically assigns every VM its IP address

## Answers and Explanations

1. **B — A software-defined computer.** A VM runs an operating system and applications using virtualized resources.
2. **A — Creates and manages virtual machines.** The hypervisor manages virtual hardware and access to physical resources.
3. **C — Packages an application and dependencies.** Containers typically share the host kernel.
4. **A — Memory-optimized.** These VMs provide relatively large memory capacity for suitable workloads.
5. **B — It may be interrupted.** Spot workloads should tolerate interruption.
6. **A — Connects virtual network interfaces.** A virtual switch forwards traffic within configured virtual networks.
7. **B — A protected private network.** Database access should be limited to authorized systems.
8. **A — Supporting resources may remain billable.** Billing depends on the provider and resource type.
9. **B — A host failure may affect multiple VMs.** Recovery and availability need to be planned.
10. **A — Traffic must traverse an enforcement point.** Diagram placement alone does not enforce security.

---

# 11. Key Takeaways

- Virtualization allows multiple isolated computing environments to share physical hardware.
- Hypervisors create and manage virtual machines.
- Containers generally share a host kernel and are commonly used for application deployment.
- VM categories are designed around workload requirements such as CPU, RAM, storage, or GPU performance.
- Cloud pricing may include compute, disks, networking, licensing, backups, and other services.
- Spot VMs can be interrupted and are best suited to workloads designed for interruption.
- Virtual switches connect virtual network interfaces, but IP addressing is usually handled by guest configurations or DHCP.
- Firewalls must be placed and configured so that the relevant traffic is actually controlled.
- Public-facing web services should be separated from sensitive internal databases.
- Backups, monitoring, patching, least privilege, and network segmentation are essential security controls.

---

# 12. Glossary

| Term | Definition |
|---|---|
| Virtualization | Software-based abstraction of computing resources |
| Virtual machine (VM) | A virtualized computer that runs an operating system and applications |
| Hypervisor | Software that creates and manages virtual machines |
| Host | The physical or underlying system that provides resources |
| Guest OS | The operating system running inside a VM |
| vCPU | A virtual CPU allocated to a VM |
| Container | An isolated application environment that typically shares the host kernel |
| Docker | A platform used to build and run containers |
| Kubernetes | A platform for orchestrating containerized applications |
| Virtual switch | A software-based network switch for virtual network interfaces |
| Virtual firewall | A firewall implemented as software or a virtual appliance |
| General-purpose VM | A VM with a balanced CPU and memory allocation |
| Compute-optimized VM | A VM designed for CPU-intensive workloads |
| Memory-optimized VM | A VM designed for memory-intensive workloads |
| Storage-optimized VM | A VM designed for storage-intensive workloads |
| GPU | Graphics Processing Unit, also used for parallel computation |
| Spot VM | A discounted VM using spare capacity that may be interrupted |
| DMZ | A network zone used to separate exposed services from internal systems |
| Network segmentation | Dividing a network into zones with controlled communication |
| Snapshot | A point-in-time representation of a VM or storage state |
| Backup | A separate recoverable copy of data or system configuration |
| IOPS | Input/output operations per second |
| vCPU | A virtual processing unit allocated to a virtual machine |

---

**End of Notes**

*These notes are intended for educational use and authorized system administration. Verify current cloud pricing, licensing, and platform-specific networking behavior before deploying production workloads.*