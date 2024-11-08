TrustZone is a system-wide hardware isolation achieved by separating the CPU into the _Normal World_ and the _Secure World_. The _Normal World_ contains and executes the main operating system, also called the _Rich OS_ (e.g. Android, GNU/Linux, etc.), which the user primarily interacts with and which performs all the non-sensitive tasks. This operating system is distrusted by design, therefore all data communicated from the _Normal World_ should be thoroughly checked before being used. In parallel exists the _Secure World_, which runs trusted code and stores/processes sensitive data.

In the following sections _Normal World_ and _Secure World_ will be abbreviated NWd and SWd respectively.

In order for the CPU to know whether it runs in secure or non-secure state, the ARM architecture uses the least significant bit of the _Secure Configuration Register_, or SCR, given in the following figure.

![[Pasted image 20240919212942.png]]

The separation being effective, the system now needs a secure mean of communication between the two worlds. To meet this requirement, ARM introduced the _Monitor Mode_, responsible for switching between the SWd and the NWd. It runs in secure state at the highest ARM execution level.

There are different ways to enter the _Monitor Mode_. From the NWd, it can be entered using a _Secure Monitor Call_, or _SMC_, instruction, through an interrupt or by raising an _External Abort_ exception, as shown in the next figure. The same mechanisms can be used from the SWd in addition to writing directly to the _Current Program Status Register_, or _CPSR_, which can only be performed by privileged processes in the SWd.


![[Pasted image 20240919213109.png]]

ARM provides all the necessary tools to vendors for them to build their own TrustZone implementation. There are few limitations and the SWd implementation can range from a simple library acting as an API, like in the Nintendo Switch, to a full-fledged operating system, such as the ones implemented by Samsung and Qualcomm, passing through intermediate solutions that we have never encountered yet:

![[Pasted image 20240919213224.png]]

![[Pasted image 20240919213443.png]]