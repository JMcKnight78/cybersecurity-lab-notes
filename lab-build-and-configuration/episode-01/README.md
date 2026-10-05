# Episode 01 — Building a Hyper-V Cybersecurity Lab

> **Video:** [Watch Episode 1 on YouTube](https://www.youtube.com/watch?v=ZUj9Mx_kjxo)

This is the public technical companion to Episode 1 of my cybersecurity lab series.

The goal of this project is to build a compact, repeatable Hyper-V environment that I can use to practice systems administration, networking, defensive security, offensive security, troubleshooting, and recovery.

This write-up reflects what was actually built in the video. Internal lab documentation contains additional implementation detail that is intentionally not published here.

---

## Objective

Episode 1 focuses on the **virtual-machine build phase**.

The goal is to establish the core systems first, then configure and validate each service in later episodes.

The lab follows this lifecycle:

```text
BUILD -> CONFIGURE -> VERIFY -> BASELINE -> BREAK -> OBSERVE -> RESTORE -> RE-VERIFY
```

A system being created does **not** mean it has been validated. Validation will be performed separately and documented with observable evidence.

## Platform

The lab is hosted on:

- Windows 11 Pro
- Microsoft Hyper-V
- Local virtual storage
- Isolated/internal virtual networking with temporary build connectivity where required

The public documentation intentionally omits unnecessary host-specific details, credentials, private management paths, and internal-only configuration.

## Core Lab Architecture

| System | Role |
|---|---|
| `FW01` | Firewall / router |
| `DC01` | Primary Windows Server domain controller and DNS |
| `DC02` | Secondary Windows Server domain controller and DNS |
| `DHCP01` | Dedicated Windows Server DHCP server |
| `WIN11-01` | Windows 11 domain workstation / target system |
| `TOOL01` | Kali Linux security and validation workstation |

The environment is intentionally small. Additional systems will only be added when a specific lab or research requirement justifies them.

## What I Did in Episode 1

The episode walks through the Hyper-V VM creation process and establishes the virtual hardware needed for the lab.

The build work includes:

- Creating the core virtual machines in **Hyper-V Manager**
- Selecting VM generations
- Assigning memory
- Creating and attaching virtual hard disks
- Connecting installation media
- Attaching virtual network adapters
- Adjusting processor allocation where needed
- Booting the VMs
- Beginning or completing guest operating-system installation
- Verifying that the newly created systems appear correctly in Hyper-V Manager

The machines were built through the Hyper-V GUI rather than PowerShell. This gives a clear look at the settings involved instead of hiding the initial build behind automation.

Several VM names were initially created using lowercase naming. Naming consistency will be cleaned up as the systems move into the configuration phase.

## Why Build Before Configuring?

I am deliberately separating **construction** from **service configuration**.

During this phase I am concerned with:

- Does the VM exist?
- Does it have appropriate virtual hardware?
- Can it boot?
- Can the operating system install?
- Does it have the connectivity required to complete setup?

I am **not** treating the lab as operational yet.

Active Directory, DNS, DHCP, routing, firewall policy, domain joins, segmentation, security controls, and final validation belong to later stages.

Keeping those phases separate makes troubleshooting much easier and creates a cleaner evidence trail.

## Resource Strategy

I am starting the lab conservatively rather than assigning large amounts of CPU and memory to every VM.

The general approach is:

- Start small
- Measure actual resource use
- Increase resources only when required
- Run only the systems needed for the current exercise

This keeps the lab practical on a single Hyper-V workstation while still supporting a realistic Windows domain environment.

## Build Connectivity vs. Lab Connectivity

One important design decision is separating **temporary build connectivity** from the lab's eventual permanent network design.

During installation, a VM may need Internet access for:

- Windows activation
- Updates
- Package installation
- Repository access
- Tool installation

That does not mean the same network path will remain after the lab is configured.

Later episodes will move the environment toward controlled routing and segmentation through the lab firewall.

## Current Status

At the end of Episode 1, the project is still in the **BUILD** phase.

```text
BUILD        <- CURRENT PHASE
CONFIGURE
VERIFY
BASELINE
BREAK
OBSERVE
RESTORE
RE-VERIFY
```

The presence of a VM in Hyper-V is not being counted as proof that its intended service works.

For example:

- A Windows Server VM is not automatically a functioning domain controller.
- A machine named `DHCP01` is not automatically providing DHCP.
- A firewall VM is not automatically enforcing segmentation.
- A Kali VM having network access does not prove the final security boundary is correct.

Those claims require testing.

## Validation Philosophy

Throughout this series I am using a simple rule:

> **Configured does not mean validated.**

For each major component I want to capture:

1. **Intent** — what the component is supposed to do.
2. **Implementation** — what was installed or configured.
3. **Verification** — how I tested it.
4. **Evidence** — what output proves the result.
5. **Baseline** — what represents a known-good state.
6. **Failure** — how I deliberately break or alter it.
7. **Restoration** — how I return it to the baseline.
8. **Re-verification** — how I prove recovery actually worked.

That process turns the environment into more than a collection of VMs. It becomes a repeatable security lab.

## What Comes Next

The next stage is to configure the machines one at a time.

```text
Firewall / Networking
        |
        v
Primary Domain Controller
        |
        v
Secondary Domain Controller
        |
        v
DHCP
        |
        v
Windows Workstation
        |
        v
Kali / Security Tooling
        |
        v
Full Integration Validation
        |
        v
Known-Good Baseline
```

After the lab reaches a verified baseline, later exercises can intentionally introduce failures and security problems, observe the results, fix them, and prove that the environment has returned to its expected state.

## Repository Purpose

This repository contains the **public technical notes** that accompany my cybersecurity videos.

The YouTube videos show the hands-on work. The GitHub write-ups preserve the architecture, reasoning, methodology, lessons learned, and validation process in a format that is easier to reference later.

Sensitive or unnecessary internal information is intentionally excluded from the public version.

## Video

**Episode 1 — Hyper-V Cybersecurity Lab Build**

[Watch on YouTube](https://www.youtube.com/watch?v=ZUj9Mx_kjxo)

---

## Series Principle

> Build it. Understand it. Validate it. Break it. Fix it. Prove it.
