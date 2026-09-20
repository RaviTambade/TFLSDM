# Understanding Virtualization

## From Physical Data Centers to Virtual Machines, Hypervisors, Linux and System Administration

Mentor: Ravi Tambade, Chief Mentor – Transflower Learning Audience: TAP Students | Final-Year Engineering Students | Aspiring Software Developers Session Type: Conceptual Learning + Classroom Role Play + Live Demonstration + Hands-on Lab Core Theme: Understand the infrastructure on which your applications run.

## Session Learning Outcomes

By the end of this session, students should be able to:

1. Explain the purpose of a physical data center.

2. Describe virtualization using a real-world analogy.

3. Explain the role of a hypervisor.

4. Differentiate Type 1 and Type 2 hypervisors.

5. Create a basic Ubuntu virtual machine using VirtualBox.

6. Inspect CPU, memory, storage, network and processes in Linux.

7. Distinguish the responsibilities of developers, system administrators and DevOps/platform engineers.

8. Explain the relationship between virtual machines and containers.

9. Perform basic resource calculations for a virtual lab.

# Part 1: Before Writing Code, Understand the Machine

## 1. Session Opening — Where Does Your Application Run?

Ravi Sir:

“Students, you are learning C#, Java, Python, JavaScript, React, Angular and backend development.

You are writing code. You are building applications.

But let me ask you one simple question.”

Ravi Sir: “Where does your application actually run?”

Student: “Sir, on a computer.”

Ravi Sir: “Correct! But what kind of computer?”

Students:

* My laptop, Sir.

* A server.

* A cloud machine.

* A virtual machine.

* A container.

Ravi Sir: “Excellent. Today, we are going to understand the machine behind the application.”

“Before we discuss virtualization, let us begin with the physical infrastructure that supports software.”

# Part 2: Understanding the Physical Data Center

## 2. What Is a Data Center?

A data center is a facility that houses computing infrastructure used to run applications, databases, websites, enterprise systems and cloud services.

Imagine an organization operating:

* Banking applications

* Insurance policy management systems

* E-commerce websites

* Hospital management systems

* Learning management platforms

* Artificial intelligence services

These applications need computing resources that are available, secure, connected and maintainable.

A data center provides the physical environment for those resources.

![发展历程 - 弘信电子集团](https://images.openai.com/static-rsc-4/4GOLDS3MGOv0xIdsOnTOIEh1StLbxcnxDCnnH8xSNo9mIkVmU_zUieyMKvAgBrPNKNbSmhNp_LHd--SbWYr-05_b0XrxgXKspqTlYbOKvJdBhj2Ecu-cDOonDhaevWc1q_KyQnvgKOulGh27nWtFs2Fqros6OlT7Xm3c_UOCJIo?purpose=inline)

![Thermal Coating for UAE Data Centers: Cutting Cooling Costs in Extreme Heat | Seal Coatings News](https://images.openai.com/static-rsc-4/xuRcqheqH-Tj5ukJqAe0SNRMPIbl_Eb7CAiUKj9CkJQlMB8P7FXbAkGbjOMNejOUySSgeAIZLhsvTK1O4ljlax2ntoGczaLZQ91LZmlVdjGiABpJKU0MNYOvAqEa40pSzlj_Dhjc0f6jl_njwWveMOPJwh47SW-4vuO4zUIaNOc?purpose=inline)

![CyberPower
– vnetwork](https://images.openai.com/static-rsc-4/TP1DTz_j3C-xCRrYLvDUUkwOZVb81AFc6xRVU1ZcRlhjlhwxw5MKWWOt9MYqPeItHHitX5yiN6ps4wQSDqNSQBAWicPGhBzhQ8GDne1HsauDafsfWZI-112gX2uWK2ZFq3QM1Cmi96nVot9h3sc7C7iZaNrJ9mPvHxi7MGkYOKk?purpose=inline)

7

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

Connect devices within a network

|
|

Routers

|

Connect different networks

|
|

Fiber-optic cabling

|

Supports high-speed network communication

|
|

Cooling systems

|

Maintain suitable operating temperatures

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

Protects infrastructure and equipment

|

Ravi Sir:

“Students, a data center is not just a room containing computers. It is an engineered environment involving electricity, cooling, networking, storage, security and operations.”

### Classroom question

Ravi Sir: “If a server has a powerful CPU but no electricity, can it run our application?”

Student: “No, Sir.”

Ravi Sir: “If it has electricity but no network connection, can remote users access the application?”

Student: “Not normally, Sir.”

Ravi Sir: “Exactly. Infrastructure is a system of connected components. A server is only one part of it.”

# Part 3: The Apartment Building Analogy

## 3. Understanding Virtualization Through a Familiar Example

Ravi Sir:

“Let us imagine a large apartment building.”

![Top 10 Most Expensive Cities for New Apartments in Germany](https://images.openai.com/static-rsc-4/yrpoSyqV6qlKd4dHEG3nnlvOMa3bTuAQ8CIQtp9tak-9bdgc_nsryBA_1v_M2R0cG8uE3dJGm1zTYAn4drkAFUSno9AHgBgQSbweCxtYWQzFLXznICHA2fQkht4rQ8QOLoO-59VrN_omSMNlc3jwVP-sOQP-b43M28JiOhYUx-g?purpose=inline)

The building has:

* A physical structure

* Electricity and water supply

* Security

* Maintenance staff

* Separate apartments

* Different families living independently

Now imagine that the building represents our physical server.

Each apartment represents an isolated virtual environment.

The building's physical infrastructure is shared, but each apartment has its own private space and occupants.

Similarly, virtualization allows multiple virtual machines to operate on one physical computer, using allocated computing resources and logical isolation.

Student: “Sir, does that mean one physical server can behave like multiple computers?”

Ravi Sir: “Exactly! That is the fundamental idea behind server virtualization.”

# Part 4: What Is Virtualization?

## 4. Definition

Virtualization is the creation of software-based representations of computing resources, such as computers, operating systems, storage and networks.

In server virtualization, one physical machine can host multiple virtual machines.

Each virtual machine can be configured with its own:

* Virtual CPU allocation

* Virtual RAM

* Virtual disk

* Virtual network adapter

* Guest operating system

* Installed applications

Traditional infrastructure

Physical Server 1 Windows Server + Application A

Physical Server 2 Linux + Application B

Physical Server 3 Database Server

Virtualized infrastructure

One Physical Server

Hypervisor

VM 1 Windows

VM 2 Ubuntu

VM 3 Linux DB

CPU • RAM • Storage • Network

### Why do organizations use virtualization?

1. Resource utilization: Use available CPU, RAM and storage more efficiently.

2. Isolation: Separate operating systems and workloads.

3. Flexibility: Create, configure or remove virtual machines as requirements change.

4. Testing: Experiment with operating systems without replacing the host OS.

5. Consolidation: Run multiple workloads on fewer physical servers.

6. Administration: Manage virtual machines through centralized tools.

Important: Virtualization does not create unlimited hardware. Every VM consumes real physical resources, and too many demanding VMs can compete for CPU, RAM, storage and network capacity.

# Part 5: Hypervisor — The Manager of Virtual Machines

## 5. What Is a Hypervisor?

Ravi Sir:

“Now, who manages these virtual machines? Who provides their virtual hardware and controls their access to the physical resources?”

Students: “The hypervisor, Sir!”

Correct.

A hypervisor, also called a Virtual Machine Monitor (VMM), is the software layer that creates and manages virtual machines and controls their access to underlying hardware resources.

## 5.1 Type 1 — Bare-Metal Hypervisor

A Type 1 hypervisor runs directly on physical hardware.

Virtual Machines

VM 1

VM 2

VM 3

Type 1 Hypervisor

Physical Hardware CPU • RAM • Storage • Network

Examples include:

* VMware ESXi

* Microsoft Hyper-V in its bare-metal deployment

* Xen

## 5.2 Type 2 — Hosted Hypervisor

A Type 2 hypervisor runs as an application on a host operating system.

Guest Virtual Machines

Ubuntu VM

Linux VM

VirtualBox — Hosted Hypervisor

Host Operating System — Windows

Physical Hardware

Examples include:

* Oracle VirtualBox

* VMware Workstation

### Classroom dialogue

Ravi Sir: “When you install Oracle VirtualBox on your Windows laptop, does VirtualBox replace Windows?”

Student: “No, Sir. It runs on top of Windows.”

Ravi Sir: “Exactly. Windows is the host operating system. Ubuntu inside VirtualBox is the guest operating system.”

# Part 6: Live Demonstration — Creating an Ubuntu Virtual Machine

## 6. Our Demonstration Architecture

Windows Laptop Physical host machine

Oracle VirtualBox Hosted hypervisor

Ubuntu Virtual Machine Guest operating system

Linux Terminal • Development Tools • Runtime

Ravi Sir:

“Today, we are not just going to talk about Linux. We are going to create a Linux machine inside our existing computer.”

## 6.1 Understand the ISO File

An ISO file is a disk-image file that can contain installation media for an operating system.

Examples:

* Ubuntu Desktop ISO

* Ubuntu Server ISO

* Windows installation ISO

The ISO is not itself a running virtual machine. It is installation media that the VM can boot from.

## 6.2 Example VM Configuration

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

These are demonstration settings, not universal requirements. Actual needs depend on the Ubuntu release, workload and host capacity.

Ravi Sir: “Suppose your laptop has 8 GB RAM and you allocate 7 GB to a virtual machine. What might happen?”

Student: “Windows may become slow because it also needs memory.”

Ravi Sir: “Correct. Capacity planning is an important infrastructure skill.”

## 6.3 Installation Steps

1. Install and launch VirtualBox.

2. Create a new virtual machine.

3. Select the Ubuntu ISO as installation media.

4. Configure virtual CPU, RAM and disk.

5. Start the VM.

6. Follow the Ubuntu installer.

7. Configure the username and password.

8. Complete installation and restart.

9. Detach the ISO when appropriate so the VM boots from its virtual disk.

![Macchine virtuali (VM): come crearne una - IONOS](https://images.openai.com/static-rsc-4/YctwHzlfxBrOsg_aiM3gUA5geYY5MLgqYE4U2Gl5umi87byi_PAza7RzzQ9YeJJpWnAjiOzq3Aj3OwFrr2N6muI85gXzF-t6hfgyg0xHEng8E_uZYxzg4z-4o2NHRTDkHZv-ReKwC2qDtQgfyCUyVf8TSwcFJwstLuVRaTSpkB0?purpose=inline)

![Install Ubuntu on VirtualBox and Configure it Properly | WxGuy](https://images.openai.com/static-rsc-4/x7euDWIeZAatXgUCiWxZ8C8FTAVRUf27g2kUABv1r9MH0YczSlB7Akytc030ys7_L9mz5LVEX9iUsHPUfdxj--XZkfqomC1nskaiSpLHM_79QrEu5uq4UIAq8xbV0UyUXeYa_nIC47iPMmCrIiA6p6lanJLAEIt-rHZN1wu3u-g?purpose=inline)

![VirtualBox – How To Install Ubuntu as Virtual Machine on Windows 10 Host - TehnoBlog.org](https://images.openai.com/static-rsc-4/o4r_ny6wjULo2BxpTJ2o0x7fwcszVInr31uyNWK-Fu7DDlSpDtKryWCqIfy_79JlFD0hgnO2e8MvdfrObgwxuf1w8nBJn1x8nPvi0zOiKVBGYLzKkAz4HShsa1CZoORrJDf0jwsGqQmm7TANb3WpEITjz_ctVXoXvpwhfge4GW4?purpose=inline)

5

Safety reminder: Carefully verify that the installer is using the VM’s virtual disk—not the host computer’s physical Windows disk.

# Part 7: Developer, System Administrator and DevOps Engineer

## 7. Who Is Responsible for What?

Ravi Sir:

“Imagine you have developed an insurance application using ASP.NET Core, React and MySQL.

It works on your laptop. Now the company wants to deploy it on a Linux server.

Who prepares the server? Who installs the runtime? Who configures networking? Who monitors the application?”

### Understanding the roles

|
Role

|

Typical responsibilities

|
| --- | --- |
|

Software Developer

|

Builds application functionality, APIs, business logic, database interactions and tests

|
|

System Administrator

|

Configures operating systems, users, permissions, storage, network services, patching and system health

|
|

DevOps / Platform Engineer

|

Automates build and deployment, manages infrastructure as code, containers, monitoring and delivery workflows

|

These responsibilities can overlap, particularly in smaller teams.

### End-to-end application lifecycle

Developer Builds and tests application

System / Platform Engineer Prepares infrastructure and runtime

CI/CD Pipeline Builds, tests and deploys software

Production Application Monitoring • Maintenance • Support

Ravi Sir:

“As software engineers, you should understand the environment in which your application runs. You may not be responsible for every infrastructure task, but you should be able to collaborate with the people who are.”

# Part 8: Virtualization, Multitasking and Multiprocessing

## 8. Do Not Confuse These Concepts

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

Software-defined computer with virtualized hardware

|
|

Hypervisor

|

Creates and manages VMs and their access to hardware

|

Ravi Sir:

“One physical server can have multiple CPU cores. A hypervisor can assign virtual CPUs to different VMs. Inside each VM, the guest operating system schedules its own processes and threads.”

### CPU overcommitment

Suppose a physical host has:

* 8 physical CPU cores

* 32 GB RAM

* 500 GB storage

A hypervisor may configure more total virtual CPUs across its VMs than the number of physical CPU cores. This is called CPU overcommitment.

It can work when workloads do not all require their full CPU allocation simultaneously. But excessive contention can reduce performance.

# Part 9: Virtual Machines and Containers

## 9. What Comes Next?

Ravi Sir:

“Suppose your ASP.NET Core application needs to run on Linux.

You can install the application and its dependencies directly on a Linux VM. Alternatively, you can package the application and its dependencies into a Docker container.

But remember: containers and virtual machines solve related, but different, problems.”

Virtual Machine

Application + Dependencies

Guest Operating System

Virtual Hardware

Container

Application + Dependencies

Container Runtime

Host Operating System Kernel

A typical container shares the host OS kernel, while a VM runs a guest operating system on virtualized hardware.

This is why containers are often lighter to start and deploy, while VMs provide a separate guest OS environment.

# Part 10: TAP Hands-on Lab

## Lab 01 — Create and Explore an Ubuntu VM

Objective: Understand the relationship between physical hardware, hypervisor, guest operating system and application runtime.

### Prerequisites

* Windows or Linux laptop

* VirtualBox

* Ubuntu ISO

* 8 GB RAM recommended for this example configuration

* Sufficient free disk space

Lab checklist

0 / 9 completed

Install and launch VirtualBox

Create a new Ubuntu virtual machine

Configure virtual CPU, RAM and disk

Install Ubuntu using the ISO

Log in and open the terminal

Inspect operating-system information

Inspect CPU and memory allocation

Install a development tool or runtime

Document the VM configuration

## 10.1 Linux Commands for the Lab

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

### What are we learning from these commands?

|
Command

|

What you learn

|
| --- | --- |
|

`cat /etc/os-release`

|

Distribution and OS release information

|
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

`whoami`

|

Current user

|
|

`ip addr`

|

Network interfaces and IP addresses

|
|

`ps aux`

|

Running processes

|

Ravi Sir:

“These are not just Linux commands to memorize. Each command helps you inspect a different aspect of your computing environment.”

# Part 11: TAP Capacity Planning Exercise

## 11. Planning a Virtual Lab for 20 Students

Ravi Sir:

“Imagine Transflower wants to conduct a Linux and backend development lab for 20 students. Each student needs one Ubuntu VM configured with 2 virtual CPUs and 4 GB RAM.

What resources should we plan for?”

### Resource calculator

## Virtual Lab Resource Calculator

Number of students

−

20

*

RAM per VM (GB)

2 GB4 GB8 GB

Virtual CPUs per VM

124

Total configured VM RAM

# 80 GB

Total configured vCPUs

# 40

These figures represent configured VM resources, not guaranteed physical capacity. Include host OS, hypervisor overhead, storage, network and workload headroom when planning infrastructure.

### Discussion questions

1. Should all student VMs run on one physical machine?

2. What happens if physical RAM is insufficient?

3. How can we distribute VMs across multiple hosts?

4. What happens if a physical host fails?

5. How can we back up VM disks?

6. What is the role of monitoring in a production environment?

# Part 12: Classroom Assessment

## TAP Knowledge Check

Knowledge check

0 / 5 answered

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

3. What is an Ubuntu ISO used for in this lab?

It is the physical CPU

It is installation media for Ubuntu

It is a network router

It is a database backup

4. What does free -h display?

Memory usage

CPU temperature

Installed packages

Network routes

5. What is a key difference between a VM and a typical container?

Containers always need a separate guest kernel

VMs virtualize hardware and run guest OSs; containers share the host kernel

VMs cannot run Linux

Containers require a physical server per application

Submit assessment

# Part 13: Closing Message — Ravi Sir’s Mentor Perspective

Ravi Sir:

“Students, today we started with a physical data center.

We explored server racks, CPU, RAM, storage, cooling, networking and power.

Then we understood virtualization. We learned that a hypervisor allows multiple virtual machines to share physical infrastructure.

We created a Linux virtual machine using VirtualBox and explored the operating system through terminal commands.

Finally, we connected infrastructure knowledge with software development, system administration and DevOps.”

## From Code to Production

Application Code C# / Java / Python / JavaScript

Runtime and Dependencies .NET / JVM / Python Runtime / Node.js

Operating System Windows / Linux

Virtualization / Infrastructure VMs / Hypervisor / Cloud / Containers

Physical Computing Resources CPU / RAM / Storage / Network / Power

> “Don’t become a developer who only knows how to run an application. Become an engineer who understands how an application is built, deployed, operated, monitored and maintained.”

### TAP Learning Outcome

Students should be able to explain virtualization, distinguish Type 1 and Type 2 hypervisors, create a basic Ubuntu VM, inspect its resources, and understand how infrastructure supports application development.
