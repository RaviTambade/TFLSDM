# Understanding Virtualization

## From Physical Data Centers to Virtual Machines, Hypervisors, Linux and System Administration

Mentor: Ravi Tambade, Chief Mentor – Transflower Learning Audience: TAP Students | Final-Year Engineering Students | Aspiring Software Developers Session Type: Conceptual Learning + Classroom Role Play + Live Demonstration Core Theme: Understand the infrastructure on which your applications run.

## 1. Session Opening: Before Writing Code, Understand the Machine

Ravi Sir:

"Students, you are learning C#, Java, Python, JavaScript, React, Angular and backend development.

You are building applications. But let me ask you one question.

Where does your application actually run?"

Student: "Sir, on a computer."

Ravi Sir: "Correct! But what kind of computer? Your laptop? A server? A virtual machine? A cloud server? A container?"

"Before we discuss virtualization, let us understand the physical infrastructure that supports software applications."

![DCIM x AIOps: The Next Big Trend Reshaping AI Software - GIGABYTE Global](https://images.openai.com/static-rsc-4/sW41XWsilnCPvfjLwlZyE9Q5NZS-pLVj2O0uY4iodntAIY1kK9Ub0aT99qB3c80KNGRUMIFE8soVKGTlKX8oXcm4crw9L9yRMOnIvRjvA2y_D7DpbmuswVlk_7IaiZPqCSkg1gR2YWdXd9C84UNgKm8atDxvnwDs85uwLQUtQA0?purpose=inline)

![ICT Interconnection and Systems Integration Inc.](https://images.openai.com/static-rsc-4/ZnRoI-h3FkwY14Cov2YOwEg1mNeLF-u0SwOX0aczJr4mWqOPDk9RR29I73JW1GKux07QPfllilaOzIaK-T1OaxNf1F2I3iuHTb4Jpa511qwxdcNE8EBWxgtprj79vECJntzRYmg__Clj0LY0bPVpKU15YOJrXRDDcGwVJsswZaM?purpose=inline)

![Google invests \$40B in Texas data centers, boosting mechanical construction | Liam Quail posted on the topic | LinkedIn](https://images.openai.com/static-rsc-4/APB_JnE4Tf0Y3Axn23I0H02ePJim_dtd0irwYpj_QcwZKt93LxIA0e1fTU_v0H2IFqsPUQg2dP1PAtllAiqixf2pJDxesXrvRYtW6QoHOQETe6yzfmHe-KD-3kOmC71FkDnVNE3-An3FK_3eG59oAuMxcBCgD03ZZmbeY-XpWqg?purpose=inline)

6

### The physical data center

A data center is a facility containing computing infrastructure used to host applications, databases, websites, enterprise systems and cloud services.

Imagine a company running:

* Banking applications

* Insurance policy management systems

* E-commerce websites

* Hospital management systems

* Learning management platforms

* Artificial Intelligence services

These applications require computing resources that must be available, secure and maintainable.

A data center provides the physical environment for those resources.

### What do we find inside a data center?

|
Component

|

Purpose

|
| --- | --- |
|

Rack cabinet

|

Holds and organizes servers

|
|

Rack servers

|

Provide CPU, RAM and computing power

|
|

SSD / HDD storage

|

Stores operating systems, databases and files

|
|

Network switches

|

Connect servers within a network

|
|

Routers

|

Connect networks

|
|

Fiber-optic cabling

|

High-speed network communication

|
|

Cooling systems

|

Maintain operating temperature

|
|

UPS

|

Provides temporary backup power

|
|

Monitoring systems

|

Track hardware health and availability

|
|

Physical security

|

Protects infrastructure

|

Ravi Sir:

"Students, a data center is not just a room containing computers. It is an engineered environment involving electricity, cooling, networking, storage, security and operations."


## 2. The Classroom Analogy: Physical Infrastructure as an Apartment Building

Ravi Sir:

"Let us imagine a large apartment building."

![13期重劃區▪台中室｜捷運宅專賣](https://images.openai.com/static-rsc-4/pqQ4ts9BiH871BkMHBhZ2-luAsT_pgGt6Ytn6KWSXM952Y0hDheEvrjX2Wj8vDjDGKO6wJS8ffm9vO27KckdconM4A0jZ2pBczJW5qxU3KSFX5t2r9Ie8Gna3Ea7mSvQA8YqOtSRHdpaQM_PsD5TBZIj6XlQkpEhTspsh5x7vi0?purpose=inline)

The building has:

* A physical structure

* Electricity and water supply

* Security

* Maintenance staff

* Separate apartments

* Different families living independently

Now imagine that the building is our physical server.

Each apartment represents an isolated virtual environment.

The building's infrastructure is shared, but each apartment has its own occupants and private space.

Similarly, virtualization allows multiple virtual machines to operate on one physical computer, with allocated computing resources and logical isolation.

Student: "Sir, does that mean one physical server can behave like multiple computers?"

Ravi Sir: "Exactly! That is the fundamental idea behind virtualization."

## 3. What Is Virtualization?

Definition:

Virtualization is the process of creating software-based representations of computing resources, such as computers, operating systems, storage and networks.

In server virtualization, a single physical machine can host multiple virtual machines.

Each virtual machine can have its own:

* Virtual CPU allocation

* Virtual RAM

* Virtual disk

* Virtual network adapter

* Operating system

* Installed applications

![Understanding Virtualization - PCSP](https://images.openai.com/static-rsc-4/iOJa4PlQ0QYF5_VfLiCZfi6abA4qnPzZSEI_hyiz2MR89WatF2LX_1tUNU1U4SC2OY5zi4NOaGyR6viKechxbtTbEOwezSn2NN1NyHsCSjijoquCewFv2b4JeyMHirxcFGtgbvevhCHWmNAGhe4vIs5U7xoPUv5feg4BBfS1SI8?purpose=inline)

### Traditional infrastructure vs virtualized infrastructure

Traditional approach

Physical Server 1

Windows Server + Application A

Physical Server 2

Linux + Application B

Physical Server 3

Database Server

Each workload has a separate physical machine.

Virtualized approach

One Physical Server

Hypervisor

VM 1

Windows

VM 2

Ubuntu

VM 3

Linux DB

Multiple virtual environments share the host's physical resources.

### Why do organizations use virtualization?

1. Resource utilization: Use available CPU, RAM and storage more efficiently.

2. Isolation: Separate operating systems and workloads.

3. Flexibility: Create or remove virtual machines as requirements change.

4. Testing: Experiment with operating systems without replacing the host OS.

5. Consolidation: Run several workloads on fewer physical servers.

6. Administration: Manage virtual machines through centralized tools.

Important: Virtualization does not magically create unlimited hardware. Every VM consumes real physical resources.

## 4. Hypervisor: The Heart of Virtualization

Ravi Sir:

"Now, who manages these virtual machines? Who allocates CPU, memory and virtual storage?"

Students: "Hypervisor, sir!"

Correct.

A hypervisor, also called a Virtual Machine Monitor (VMM), is the software layer that creates and manages virtual machines and controls their access to underlying hardware resources.

### Two types of hypervisors

## Type 1 – Bare-metal hypervisor

Runs directly on physical hardware

![\[Tản mạn\] Ảo hóa - Ai cũng biết nhưng cụ thể nó là gì ?](https://images.openai.com/static-rsc-4/X09xgGsJbLEHPC84siqHdlGfXhRZY1Uv2_VGDicvxJQBn6geMwmnJtzLLgJ-SWZg66Bu1GfbtkWmB40lgxrfYffXfbGrXGvWpdtvzltJxpA2KoK2yyb7SRImKLNM89lhsFRovrebFC-lqCppjX0CPQW3czaneWeQHLCLcvk3WQI?purpose=inline)

Physical Hardware

Hypervisor

VM 1

VM 2

VM 3

Examples: Microsoft Hyper-V in its bare-metal deployment, VMware ESXi, and Xen.

## Type 2 – Hosted hypervisor

Runs as an application on a host OS

![Virtualization](https://images.openai.com/static-rsc-4/rWWYAeiMHySJ6XW38zGAzi9iTzTMOqzCQl9AOOZdcDEye8_uG3g2HRv0Ci5QiwvZW5L5n5WqfFLUHZIMG5oPtizmyuDeK4xlVC8P6BA05LIBphDgVn0CxrduhAjfqzYARdBBUxr5kt6w5dfaQQN0FVg0mhw6x4foURgpljUVU5U?purpose=inline)

Physical Hardware

Host Operating System – Windows

VirtualBox

Ubuntu VM

Linux VM

Examples: Oracle VirtualBox and VMware Workstation.

### Classroom question

Ravi Sir: "Students, when you install Oracle VirtualBox on your Windows laptop, is VirtualBox directly replacing Windows?"

Student: "No, sir. It runs on top of Windows."

Ravi Sir: "Exactly. That is a Type 2 hosted hypervisor."

## 5. Live Demonstration: Creating an Ubuntu Virtual Machine

The session moves from theory to hands-on infrastructure practice.

Ravi Sir:

"Today, we are not just going to talk about Linux. We are going to create a Linux machine inside our existing computer."

### Demonstration architecture

Your Windows Laptop

Physical host machine

Oracle VirtualBox

Hosted hypervisor

Ubuntu Virtual Machine

Guest operating system

Linux + Terminal + Development Tools

### Step 1: Understand the installation files

Ravi Sir:

"Before installing Ubuntu, let us understand the difference between an operating system and an ISO file."

An ISO file is a disk-image file that can contain installation media for an operating system.

For example:

* Ubuntu Desktop ISO

* Ubuntu Server ISO

* Windows installation ISO

The ISO is not itself a running virtual machine. It is installation media that the VM can boot from.

### Step 2: Configure the virtual machine

For a classroom demonstration, a possible starting configuration is:

|
Resource

|

Example allocation

|
| --- | --- |
|

Virtual CPUs

|

2

|
|

RAM

|

4 GB

|
|

Virtual disk

|

25 GB

|
|

Operating system

|

Ubuntu Desktop

|
|

Network

|

NAT

|
|

Installation media

|

Ubuntu ISO

|

These are demonstration settings, not universal requirements. Actual resource needs depend on the Ubuntu release, workload and host capacity.

Ravi Sir:

"Students, if your laptop has 8 GB RAM and you allocate 7 GB to a VM, what happens?"

Student: "Sir, Windows may become slow because it also needs memory."

"Correct! Capacity planning is an important responsibility of infrastructure engineers."

### Step 3: Install Ubuntu

![Install Ubuntu 22.04 LTS Desktop \[Step By Step\] - OSTechNix](https://images.openai.com/static-rsc-4/AL8uUAgKfUxGxfnPTRhPDuaI-M3GkOsJJ9Mcs4MQjbFJT5vAck65imLgImuVvxzywzwARuSjTdEnknjC1sidtMP7o3BYtV9gOh_X_QYwbiKuT9XHuHe2wfi4rY8r6QPU7VRGH1bhW7lxsP1dgPoUBuPU4KEgfR43cFOwTkL4fG0?purpose=inline)

![How to Dual Boot Windows 11 and Linux on Separate Hard Drives](https://images.openai.com/static-rsc-4/N3gTUPdoPLwU80hi-9yORSUuHB1dd4ZSfkIt_5bKiITkECERj8GDaFKqhPptnIrFTQv_OpEdjTLdcSU_iwy7nak1M1o5JSNxCl76I21ay65wMiRAWz_Nmmd7rBRL5TD4dx_C075C29Zxy9ikIdo-Zu8WiUPemUgVoKzEHabhk-M?purpose=inline)

![Como instalar o Ubuntu 22.04 no VirtualBox?](https://images.openai.com/static-rsc-4/Lqa27HHXgxilTPyscd0OVmrzTIsIS1mPsaaUgBpCSK3Mi2x6m4tsfl4bdWH828q022qkRnp3V_UIvpTAGUa-br4GO3lP_LiTMCZJPaVnJNw_-yPX11j9sWF1W8lNmwa_wzL4l9W-d7xYxCyrSmbiza2AiRAKdwjVTD8dBwl5Y9M?purpose=inline)

7

Typical installation flow:

1. Create a new VM.

2. Select the Ubuntu ISO as installation media.

3. Configure virtual CPU, RAM and disk.

4. Start the VM.

5. Follow the Ubuntu installer.

6. Configure the username and password.

7. Complete installation and restart.

8. Remove or detach the ISO when appropriate so the VM boots from its virtual disk.

Important classroom safety note: When installing an operating system, carefully check which disk is selected. The virtual disk should be used for this exercise, not the host computer's physical Windows disk.


## 6. Software Developer vs System Administrator vs DevOps Engineer

One of the important learning moments in the session is understanding that building software and operating software are related but distinct responsibilities.

Ravi Sir:

"Imagine you have developed an insurance application using ASP.NET Core, React and MySQL.

You have written the code. Your application works on your laptop.

Now the company asks you to deploy it on a Linux server.

Who will configure the server? Who will install the runtime? Who will configure networking? Who will monitor the application?"

### Understanding the roles

Software Developer

* Develops application functionality.

* Writes business logic and APIs.

* Designs database interactions.

* Writes unit and integration tests.

* Fixes application defects.

System Administrator

* Installs and configures operating systems.

* Manages users, permissions and storage.

* Configures network services.

* Applies patches and monitors system health.

* Troubleshoots infrastructure problems.

DevOps / Platform Engineer

* Automates build and deployment pipelines.

* Creates infrastructure using code.

* Manages containers and orchestration.

* Implements monitoring and observability.

* Improves deployment reliability and operational workflows.

### End-to-end application lifecycle

Developer

Builds and tests application

System / Platform Engineer

Prepares infrastructure and runtime

CI/CD Pipeline

Builds, tests and deploys software

Production Application

Monitoring, maintenance and support

Ravi Sir:

"Students, as software engineers, you should understand the environment in which your application runs. You may not be responsible for every infrastructure task, but you should be able to collaborate with the people who are."

## 7. Virtualization, Multiprocessing and Multitasking

The transcript also connects virtualization with CPU resources and operating-system concepts.

Let us distinguish these terms carefully.

|
Concept

|

Meaning

|
| --- | --- |
|

Multitasking

|

Operating system manages multiple executing tasks

|
|

Multiprocessing

|

Uses multiple processing units or CPU cores to execute work

|
|

Virtualization

|

Creates logical computing environments using physical resources

|
|

Virtual machine

|

A software-defined computer with virtualized hardware

|
|

Hypervisor

|

Manages virtual machines and their hardware access

|

Ravi Sir:

"One physical server can have multiple CPU cores. A hypervisor can allocate virtual CPUs to different VMs. Inside each VM, the guest operating system schedules its own processes and threads."

### A practical example

Suppose a physical host has:

* 8 physical CPU cores

* 32 GB RAM

* 500 GB storage

We configure:

|
VM

|

vCPU allocation

|

RAM

|
| --- | --- | --- |
|

Ubuntu Development

|

2

|

4 GB

|
|

Windows Testing

|

2

|

8 GB

|
|

Linux Database

|

2

|

8 GB

|
|

Linux Web Server

|

2

|

4 GB

|
|

Host and overhead

|

Shared resources

|

Remaining memory

|

The assigned virtual CPUs are not necessarily dedicated physical cores. Actual scheduling and resource usage depend on the hypervisor and configuration.

Classroom question: Can we allocate more virtual CPUs across VMs than the host has physical cores?

Yes. This is called CPU overcommitment. It can work for workloads that do not constantly need all allocated CPU capacity, but excessive contention can affect performance.

## 8. Virtual Machines and Containers: What Comes Next?

Once students understand virtualization, introduce containers as the next step in the infrastructure learning journey.

![Part 21: Virtualization & Containers | Computer Architecture & OS Mastery - Wasil Zafar](https://images.openai.com/static-rsc-4/7ME8V5msWkHVqk4QBDeu5FfOHq_zn3Ms4ZFik8q7WDjBs5RM92XugCjuErU9pCkJQm-mOMVSwpeKUW5zwrnR1CE_Wdu6RwDKx_K3mxymBjx9pXHxtSo2Mz2ylMeHYqll_fuo51GGFnpt62aK-S5Bj6AEA0WxjVpavsWz8NI7Vbg?purpose=inline)

|
Virtual Machine

|

Container

|
| --- | --- |
|

Virtualizes hardware

|

Isolates applications using OS-level mechanisms

|
|

Runs a guest operating system

|

Shares the host kernel

|
|

Usually includes a complete guest OS

|

Packages application and dependencies

|
|

Typically uses more resources

|

Often has lower overhead

|
|

Managed by a hypervisor

|

Managed by a container runtime

|

Ravi Sir:

"Suppose your ASP.NET Core application needs to run on Linux.

You can install the application directly on a Linux VM. Alternatively, you can package the application and its dependencies into a Docker container.

But remember: Docker containers and virtual machines solve related but different problems."

# 9. TAP Hands-on Lab: Your First Virtual Linux Server

Practical Assignment

# Lab 01 – Create and Explore an Ubuntu VM

Objective: Understand the relationship between physical hardware, hypervisor, guest operating system and application runtime.

Prerequisites

* Windows or Linux laptop

* VirtualBox

* Ubuntu ISO

* At least 8 GB RAM recommended for this classroom configuration

* Sufficient free disk space

Tasks

0 of 9 tasks completed

Install and launch VirtualBox

Create a new Ubuntu virtual machine

Configure virtual CPU, RAM and disk

Install Ubuntu using the ISO

Log in and open the terminal

Inspect operating-system information

Inspect CPU and memory allocation

Install a development tool or runtime

Document the VM configuration

### Linux commands for the lab

Run these commands inside the Ubuntu terminal.

Bash

```
# Operating system information
cat /etc/os-release

# Kernel information
uname -a

# CPU information
lscpu

# Memory usage
free -h

# Disk usage
df -h

# Current user
whoami

# Network configuration
ip addr

# Current processes
ps aux
```

Mentor's explanation:

"These commands are not just Linux commands to memorize. Each one helps you inspect a different aspect of the computing environment."

|
Command

|

What you learn

|
| --- | --- |
|

`uname -a`

|

Kernel and system information

|
|

`lscpu`

|

CPU architecture and processors visible to the VM

|
|

`free -h`

|

Memory usage

|
|

`df -h`

|

Filesystem capacity

|
|

`ip addr`

|

Network interfaces and addresses

|
|

`ps aux`

|

Running processes

|

## 10. TAP Capacity Planning Exercise

Ravi Sir:

"Now imagine Transflower wants to conduct a Linux and backend development lab for 20 students.

Each student needs one Ubuntu VM configured with 2 virtual CPUs and 4 GB RAM.

What resources should we plan for?"

## Virtual Lab Resource Calculator

Number of students: 20

RAM per VM: 4 GB

Virtual CPUs per VM: 2

Total allocated VM RAM

# 80 GB

Total configured vCPUs

# 40

These figures represent configured VM resources, not guaranteed physical capacity. Add host OS, hypervisor overhead and workload headroom when planning infrastructure.

Discussion questions:

1. Should all student VMs run on one physical machine?

2. What happens if physical RAM is insufficient?

3. How can we distribute VMs across multiple hosts?

4. What happens if a physical host fails?

5. How can we back up VM disks?

6. What is the role of monitoring in a production environment?

## 11. Classroom Assessment: Check Your Understanding

TAP Knowledge Check

0/5 answered

1. What is the primary purpose of a hypervisor?

To write application code

To create and manage virtual machines

To design database tables

To compile JavaScript

2. Which is an example of a Type 2 hypervisor?

Oracle VirtualBox

A physical network switch

A Linux shell

A database server

3. What is Ubuntu ISO used for in this lab?

It is the physical CPU

It is installation media for Ubuntu

It is a network router

It is a database backup

4. What does free -h display?

Memory usage

CPU temperature

Installed packages

Network routes

5. What is the key difference between a VM and a typical container?

Containers always need a separate guest kernel

VMs virtualize hardware and run guest OSs; containers share the host kernel

VMs cannot run Linux

Containers require a physical server per application

Submit assessment

## 12. Closing Message – Ravi Sir's Mentor Perspective

Ravi Sir:

"Students, today we started with a physical data center.

We explored server racks, CPU, RAM, storage, cooling, networking and power.

Then we understood virtualization.

We learned that a hypervisor allows multiple virtual machines to share physical infrastructure.

We created a Linux virtual machine using VirtualBox and explored the operating system.

And finally, we connected infrastructure knowledge with software development, system administration and DevOps."

"Remember one thing. A software developer should not think only about the code written inside Visual Studio or VS Code.

Think about the complete journey."

## From Code to Production

Application Code

C# / Java / Python / JavaScript

Runtime and Dependencies

.NET / JVM / Python Runtime / Node.js

Operating System

Windows / Linux

Virtualization / Infrastructure

VMs / Hypervisor / Cloud / Containers

Physical Computing Resources

CPU / RAM / Storage / Network / Power

Final mentor takeaway:

> "Don't become a developer who only knows how to run an application. Become an engineer who understands how an application is built, deployed, operated, monitored and maintained."

TAP Learning Outcome: Students should be able to explain virtualization, distinguish Type 1 and Type 2 hypervisors, create a basic Ubuntu VM, inspect its resources and understand how infrastructure supports application development.
