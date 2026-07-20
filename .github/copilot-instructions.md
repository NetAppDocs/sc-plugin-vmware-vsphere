## Copilot instructions for SnapCenter Plug-in for VMware vSphere documentation

### Repository overview
Product: SnapCenter Plug-in for VMware vSphere (SCV)

*SCV* is a Linux-based virtual appliance, deployed from an OVA, that protects VMware *VMs*, *datastores*, and *VMDKs* with backup, restore, mount/unmount, and guest file restore operations. It also works with *SnapCenter Server* to support application-consistent data protection for applications running in VMware environments.

### Repository structure
- `./` – Main AsciiDoc topics and site config files for SCV concepts, deployment, quick start, monitoring, storage management, data protection, restore, guest file restore, REST APIs, upgrade, and legal notices.
- `media/` – Shared screenshots, diagrams, and workflow images referenced by the root-level AsciiDoc topics.

### Product-specific context
**Architecture and components:**
- *SCV* is a standalone Linux virtual appliance; deploy a separate SCV instance for each *vCenter Server*, including linked-mode environments.
- The *VMware vSphere client* in vCenter is used for VM, datastore, and VMDK protection workflows, while the *SnapCenter* user interface is used for application-consistent backup and restore workflows on VMs.
- SCV stores its own metadata in an internal *MySQL* database, and this repository includes procedures to back up and restore that database from the appliance maintenance console.
- Add *storage VMs (SVMs)* or clusters to SCV before protecting resources; *ONTAP tools for VMware vSphere* must be deployed before protecting *vVol* VMs.
- Many *VMFS* restore workflows depend on *VMware Storage vMotion*, while many *NFS* restore workflows use native ONTAP restore functionality.

**Key concepts:**
- A *resource group* is the protection container for VMs, datastores, vSphere tags, or VM folders; all VM and datastore backups run against resource groups.
- A *backup policy* defines backup frequency, retention, replication, and options such as *VM consistency*; resource groups use policies to determine when and how backups run.
- *Traditional VMs*, *vVol VMs*, *FlexGroup* datastores, and *ASA r2* resources have different grouping rules and cannot always be combined in the same resource group.
- *Guest file restore* restores files from a backed-up *VMDK* by attaching the disk to a guest VM or *proxy VM*, creating a restore session, and then browsing the disk contents.
- *Secondary protection* uses *SnapMirror* and *SnapVault* relationships; for *ASA r2* resources, the docs describe consistency-group-based secondary protection.

**Naming conventions and terminology:**
- *SCV* is the repository’s standard abbreviation for *SnapCenter Plug-in for VMware vSphere*.
- *SVM* means *storage VM*; the docs use *storage VM* for ONTAP storage systems added to SCV inventory.
- *VMDK* means VMware virtual disk; the docs treat VM restore, VMDK restore, datastore mount/unmount, and guest file restore as separate workflows.
- *vVol* means VMware virtual volume; *vVol VMs* support crash-consistent backups only and require *ONTAP tools for VMware vSphere*.
- `scbr.override` is the SCV override file for configurable properties such as datastore exclusion patterns.
- `_recent` is a special backup suffix SCV can add to the latest snapshot for each policy.
- Names for VMs, datastores, policies, backups, and resource groups must avoid special characters and spaces; an underscore is allowed.

### Typical user workflows
**Initial setup:** Deploy the SCV OVA → register one SCV appliance with one vCenter Server → add storage systems → create backup policies → create resource groups

**Protect VMs and datastores:** Open the SnapCenter vSphere client → select VMs, datastores, tags, or VM folders for a resource group → attach one or more backup policies → run backups on demand or on a schedule → monitor jobs and backups

**Restore guest files:** Start guest file restore from a backed-up VMDK → attach the virtual disk to a guest VM or proxy VM → wait for the restore session → browse the VMDK contents → restore selected files or folders
