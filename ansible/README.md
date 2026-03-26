# Ansible — Labs AOS-CX Aruba
# Ansible — AOS-CX Aruba Labs

> 🇫🇷 [Français](#fr) | 🇬🇧 [English](#en)

---

<a name="fr"></a>
## 🇫🇷 Français

### Présentation

Ce dossier contient les playbooks Ansible pour l'automatisation de switches **HPE Aruba AOS-CX**, organisés en 6 labs progressifs couvrant le catalogue Airheads HPE Aruba — du VLAN management basique jusqu'aux architectures Datacenter Collapsed Core.

### Prérequis

- VM `automation.culetto.fr` avec :
  - `ansible-core` >= 2.15
  - Collection `arubanetworks.aoscx` installée :
    ```bash
    ansible-galaxy collection install arubanetworks.aoscx
    ```
  - Python >= 3.10
- Switch AOS-CX cible avec REST API activée (port 443)
- Credentials switch dans `group_vars/all.yml` ou variables CI/CD GitLab

---

### Installation

```bash
# Cloner le repo
git clone https://github.com/Luconik/netdevops.git
cd netdevops

# Installer la collection AOS-CX
ansible-galaxy collection install arubanetworks.aoscx

# Vérifier l'inventaire
ansible -i ansible/hosts.yml all -m ping
```

---

### Structure

```
ansible/
├── ansible.cfg
├── hosts.yml
├── group_vars/
│   └── all.yml          ← variables globales (IP switch, credentials)
└── labs/
    ├── 01-vlan-management/
    ├── 02-l2-campus/
    ├── 03-l3-ospf/
    ├── 04-vsx-l2/
    ├── 05-vsx-l3-campus/
    └── 06-datacenter-collapsed-core/
```

---

### Labs disponibles

| Lab | Topologie | Modules principaux | Complexité |
|-----|-----------|-------------------|------------|
| [01 — VLAN Management](#lab-01) | Access & Trunk basique | `aoscx_vlan`, `aoscx_l2_interface` | ⭐ |
| [02 — Campus 2-Tier L2](#lab-02) | VSX + ISL + MCLAG + Active Gateway | `aoscx_vsx`, `aoscx_lag_interface` | ⭐⭐ |
| [03 — Campus 2-Tier L3](#lab-03) | OSPF Routed Access + SVIs | `aoscx_ospf_router`, `aoscx_l3_interface` | ⭐⭐⭐ |
| [04 — VSX L2 pur](#lab-04) | VSX + MSTP + MCLAG | `aoscx_vsx`, `aoscx_stp` | ⭐⭐ |
| [05 — Campus 3-Tier](#lab-05) | VSX Aggregation + Core + OSPF | `aoscx_vsx`, `aoscx_ospf_router` | ⭐⭐⭐⭐ |
| [06 — Datacenter Collapsed Core](#lab-06) | MCLAG + Jumbo MTU + OSPF upstream | `aoscx_lag_interface`, `aoscx_ospf_router` | ⭐⭐⭐⭐ |

---

### Lancer un lab manuellement

```bash
cd ansible/

# Lab 01 — VLAN Management
ansible-playbook -i hosts.yml labs/01-vlan-management/site.yml

# Avec surcharge de variables
ansible-playbook -i hosts.yml labs/01-vlan-management/site.yml \
  -e "switch_ip=10.224.100.180"

# Mode dry-run (check)
ansible-playbook -i hosts.yml labs/01-vlan-management/site.yml --check --diff

# Verbose
ansible-playbook -i hosts.yml labs/01-vlan-management/site.yml -vvv
```

---

### Détail des labs

<a name="lab-01"></a>
#### Lab 01 — VLAN Management

Création de VLANs, configuration de ports Access et Trunk. Point de départ de tout lab réseau — configuration déclarative via variables YAML.

**Concepts couverts :** `aoscx_vlan`, `aoscx_l2_interface`, ports access/trunk, mode déclaratif

```bash
ansible-playbook -i hosts.yml labs/01-vlan-management/site.yml
```

---

<a name="lab-02"></a>
#### Lab 02 — Campus 2-Tier L2 avec VSX

Introduction au **VSX (Virtual Switching Extension)** — équivalent HPE du MC-LAG Cisco. Configuration de l'ISL, des MCLAGs vers les switches d'accès, et des SVIs avec **Active Gateway** (anycast gateway).

**Concepts couverts :** VSX, ISL, MCLAG, Active Gateway, `aoscx_vsx`, `aoscx_lag_interface`

```bash
ansible-playbook -i hosts.yml labs/02-l2-campus/site.yml
```

---

<a name="lab-03"></a>
#### Lab 03 — Campus 2-Tier L3 OSPF Routed Access

Architecture Routed Access avec OSPF sur les SVIs et uplinks routés, loopbacks comme Router-ID stables.

**Concepts couverts :** OSPF, SVIs, loopbacks, `aoscx_ospf_router`, `aoscx_ospf_interface`, `aoscx_l3_interface`

```bash
ansible-playbook -i hosts.yml labs/03-l3-ospf/site.yml
```

---

<a name="lab-04"></a>
#### Lab 04 — VSX L2 pur avec MSTP

Focus sur MSTP avec plusieurs instances par groupe de VLANs. Interaction VSX + Spanning Tree.

**Concepts couverts :** VSX, MSTP, ISL, MCLAG, `aoscx_stp`, `aoscx_stp_instance`

```bash
ansible-playbook -i hosts.yml labs/04-vsx-l2/site.yml
```

---

<a name="lab-05"></a>
#### Lab 05 — Campus 3-Tier VSX OSPF

Architecture 3-Tier avec couche Aggregation VSX, couche Core et redistribution OSPF entre niveaux.

**Concepts couverts :** 3-Tier, VSX Aggregation, OSPF inter-couches, Active Gateway, `aoscx_vsx`, `aoscx_ospf_router`

```bash
ansible-playbook -i hosts.yml labs/05-vsx-l3-campus/site.yml
```

---

<a name="lab-06"></a>
#### Lab 06 — Datacenter Collapsed Core

Architecture Datacenter avec MCLAG vers les serveurs, Jumbo MTU 9000, et OSPF vers l'upstream.

**Concepts couverts :** Collapsed Core, MCLAG, Jumbo MTU, `aoscx_lag_interface`, `aoscx_interface`, `aoscx_ospf_router`

```bash
ansible-playbook -i hosts.yml labs/06-datacenter-collapsed-core/site.yml
```

---

### Références

- [Collection arubanetworks.aoscx — Ansible Galaxy](https://galaxy.ansible.com/arubanetworks/aoscx)
- [HPE Aruba AOS-CX Ansible Documentation](https://developer.arubanetworks.com/aruba-aoscx/docs/ansible-getting-started)
- [HPE Airheads Community](https://community.arubanetworks.com)
- [`../terraform/`](../terraform/) — Infrastructure as Code AOS-CX
- [`../docs/gitlab-cicd/`](../docs/gitlab-cicd/) — Pipeline CI/CD

---
---

<a name="en"></a>
## 🇬🇧 English

### Overview

This folder contains Ansible playbooks for automating **HPE Aruba AOS-CX** switches, organized in 6 progressive labs covering the HPE Aruba Airheads catalogue — from basic VLAN management to Datacenter Collapsed Core architectures.

### Prerequisites

- VM `automation.your-domain.com` with:
  - `ansible-core` >= 2.15
  - `arubanetworks.aoscx` collection:
    ```bash
    ansible-galaxy collection install arubanetworks.aoscx
    ```
  - Python >= 3.10
- AOS-CX switch with REST API enabled (port 443)
- Switch credentials in `group_vars/all.yml` or GitLab CI/CD variables

### Labs

| Lab | Topology | Key modules | Complexity |
|-----|----------|-------------|------------|
| 01 — VLAN Management | Access & Trunk | `aoscx_vlan`, `aoscx_l2_interface` | ⭐ |
| 02 — Campus 2-Tier L2 | VSX + ISL + MCLAG + Active GW | `aoscx_vsx`, `aoscx_lag_interface` | ⭐⭐ |
| 03 — Campus 2-Tier L3 | OSPF Routed Access + SVIs | `aoscx_ospf_router`, `aoscx_l3_interface` | ⭐⭐⭐ |
| 04 — VSX L2 | VSX + MSTP + MCLAG | `aoscx_vsx`, `aoscx_stp` | ⭐⭐ |
| 05 — Campus 3-Tier | VSX Agg + Core + OSPF | `aoscx_vsx`, `aoscx_ospf_router` | ⭐⭐⭐⭐ |
| 06 — DC Collapsed Core | MCLAG + Jumbo MTU + OSPF | `aoscx_lag_interface`, `aoscx_ospf_router` | ⭐⭐⭐⭐ |

### Run a lab

```bash
cd ansible/

# Run lab 01
ansible-playbook -i hosts.yml labs/01-vlan-management/site.yml

# Dry-run
ansible-playbook -i hosts.yml labs/01-vlan-management/site.yml --check --diff
```

### References

- [arubanetworks.aoscx — Ansible Galaxy](https://galaxy.ansible.com/arubanetworks/aoscx)
- [HPE Aruba AOS-CX Ansible Docs](https://developer.arubanetworks.com/aruba-aoscx/docs/ansible-getting-started)
- [`../terraform/`](../terraform/) — IaC AOS-CX
- [`../docs/gitlab-cicd/`](../docs/gitlab-cicd/) — CI/CD Pipeline

---

*Last updated: March 2026 — [@Luconik](https://github.com/Luconik)*
