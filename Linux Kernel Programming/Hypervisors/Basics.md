
Check linked in of Yuvraj Sakshith for Hypervisors 
https://github.com/yuvraj1803?tab=repositories

One of the key functions a hypervisor provides is isolation, meaning that a guest cannot affect the operation of the host or any other guest, even if it crashes. As such, the hypervisor must carefully emulate the hardware of a physical machine, and (except under carefully controlled circumstances), prevent access by a guest to the real hardware.

Buying separate servers for different applications is expensive, time-consuming, and takes up space. Type 1 hypervisors allow IT to better utilize server hardware, thus lowering capital expenditures, freeing up real estate, and minimizing energy usage. Most of them also automate resource allocation as needed, which results in dynamic and efficient resource allocation in the virtualized environment.

Hypervisors do not simply virtualize machines, they also protect high-availability with native durability and redundancy. For example, a failover cluster supports virtualized node environments for continued availability if a node goes down.

### What are embedded hypervisors

Embedded hypervisor refers to a type 1 hypervisor deployed within an embedded system. They are used when security and system reliability is crucial. Like i.e.

- transportation
- avionics
- military
- robotics
- any life critical systems

https://tandasat.github.io/Hypervisor_Development_for_Security_Researchers.html
https://revers.engineering/7-days-to-virtualization-a-series-on-hypervisor-development/

#### 7 thoughts on “5 Days to Virtualization: A Series on Hypervisor Development”

- Pingback: [Day 0: Virtual Environment Setup, Scripts, and WinDbg - Reverse Engineering](https://revers.engineering/day-0-virtual-environment-setup-scripts-and-windbg/)
    
- Pingback: [Applied Reverse Engineering Series - Reverse Engineering](https://revers.engineering/applied-reverse-engineering-series/)
    
- Pingback: [MMU Virtualization via Intel EPT - Index - Reverse Engineering](https://revers.engineering/mmu-virtualization-via-intel-ept-index/)
    
- Pingback: [5 Days to Virtualization: A Series on Hypervisor Development – Reverse Engineering – Library 7: Mad Tea Party Edition](https://aeternusmalus.wordpress.com/2020/04/15/5-days-to-virtualization-a-series-on-hypervisor-development-reverse-engineering/)
    
- Pingback: [Bystolic](https://online21rxon.com/)
    
- Pingback: [Hypervisor Development in Rust Part 1 – My Blog](https://kiranblog.me/2023/04/15/hypervisor-development-in-rust-part-1/)
    
- Pingback: [Hypervisor Style in Rust – TOP Show HN](https://show-hn.com/2023/04/15/hypervisor-style-in-rust/)
    

### Virtual machines and virtual CPUs

It is important to understand the difference between a Virtual Machine (VM) and a Virtual CPU (vCPU). A VM will contain one or more vCPUs, as shown in the following diagram: