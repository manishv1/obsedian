
Main processor typically allocates a specific region of memory with in main system RAM, which is isolated from the rest of memory to ensure security and prevent unauthorised access.

The processor's hardware enforces this separation, ensuring that only trusted applications running inside the TEE can access this memory.


Here's how memory for TEE works:

### 1. **Isolated Memory Regions:**

- The TEE is allocated a protected region of memory within the main system’s RAM, which is isolated from the memory used by the **Rich Execution Environment (REE)** (the regular operating system and applications).
- The processor's hardware enforces this separation, ensuring that only trusted applications running inside the TEE can access this memory.

### 2. **Memory Management:**

- The main processor uses features such as **Memory Protection Units (MPU)** or **Memory Management Units (MMU)** to enforce access controls.
- These units ensure that only the TEE can access its allocated memory space, while the regular operating system and applications are restricted from accessing it.
### 3. **Dynamic Memory Allocation:**

- In some architectures like ARM TrustZone, memory is dynamically allocated between the TEE and the REE. When the system transitions into the TEE, the processor switches to a separate, secure memory address space, and when switching back to the REE, it returns to the regular memory space.
### 4. **Encryption and Integrity Protection:**

- The memory used by the TEE may also be encrypted to provide an additional layer of security. This ensures that sensitive data stored in the TEE memory is protected, even if someone tries to physically access the device's RAM.
### Examples of Memory Isolation Mechanisms:

- **ARM TrustZone**: TrustZone-based systems partition the memory into secure and non-secure worlds, with the TEE using the secure world.
- **Intel SGX (Software Guard Extensions)**: Intel SGX isolates memory using encrypted memory regions called enclaves, which are accessible only by trusted applications running inside the SGX environment.

In short, while the TEE shares the physical memory of the system, it operates in a tightly controlled and isolated region to protect sensitive data and processes.


## Trusted Execution Environment

A Trusted Execution Environment (TEE) is a secure area inside a main processor. It runs in parallel of the operating system, in an isolated environment. It guarantees that the code and data loaded in the TEE are protected with respect to confidentiality and integrity. This alongside-system is intended to be more secure than the classic system (called REE or Rich Execution Environment) by using both hardware and software to protect data and code.

Trusted applications running in a TEE have access to the full power of a device's main processor and memory, whereas hardware isolation protects these components from user installed applications running in the main operating system. Software and cryptographic isolations inside the TEE protect the different contained trusted applications from each other.

This starts a series of two blogposts discussing hardware technologies that can be used to support TEE implementations:

- TrustZone from ARM
    
- SGX from Intel

ARM processors with TrustZone implement architectural Security Extensions in which each of the physical processor cores provides two virtual cores, one being considered non-secure, and called _Non Secure World_, the other being considered Secure and called _Secure World_, and a mechanism to context switch between the two, known as the _monitor mode_.

A schema from ARM:

![[Pasted image 20240919210146.png]]As illustrated by this figure, TrustZone consist in a monitor, an optional OS and optional applications, all running in _Secure World_. A Trustzone implementation could be all those components like on the Qualcomm or Trustonic implementations, or only a Monitor as the Nintendo Switch implementation does.

Implementing a TrustZone OS provides a more flexible model for adding trusted functionality which are meant to provide additional secure service to the _Normal World_.

These features are available as signed third-party applications (called _trustlet_), and are securely loaded and executed in Secure World by the operating system running in TrustZone (the SecureOS).

One of these secure loading features (namely the Qualcomm one) was fully [retro engineered by Gal Beniamini](http://bits-please.blogspot.com/2016/04/exploring-qualcomms-secure-execution.html), before Qualcomm [made it public](https://www.qualcomm.com/media/documents/files/secure-boot-and-image-authentication-technical-overview.pdf).

The trustlet integrity checking process is relatively standard and consists of a hash table, each hash represents the hash of a segment of the ELF binary. This hash table is then signed by the trustlet issuer, and this signature can be verified through a certificate chain placed directly after the signature. This chain of trust can be validated thanks to the root certificate of which a SHA256 is placed in a fuse (QFuse), ensuring its integrity.

![Trustlet integrity checking schema](https://blog.quarkslab.com/resources/2018-06-18-trustzone/images/schema.png)

Source: Qualcomm

TrustZone is used for many purposes, including DRM, accessing platform hardware features such as stored RSA public key hash in eFuse, Hardware Credential Storage, Secure Boot, Secure Element Emulation, etc.