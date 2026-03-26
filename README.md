<div align="center">

![Luconik Banner](assets/Logo_Luconik.png)

# netdevops

**Automatisation réseau HPE Aruba AOS-CX — Ansible · Terraform · GitLab CI/CD**

[![GitHub](https://img.shields.io/badge/GitHub-Luconik-181717?style=flat-square&logo=github)](https://github.com/Luconik)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-nicolasculetto-0077B5?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/nicolasculetto/)
[![Ansible](https://img.shields.io/badge/Ansible-AOS--CX-EE0000?style=flat-square&logo=ansible)](https://galaxy.ansible.com/arubanetworks/aoscx)
[![Terraform](https://img.shields.io/badge/Terraform-aruba%2Faoscx-7B42BC?style=flat-square&logo=terraform)](https://registry.terraform.io/providers/aruba/aoscx/latest)

> 🇫🇷 [Français](#fr) | 🇬🇧 [English](#en)

</div>

---

<a name="fr"></a>
## 🇫🇷 Français

### Présentation

Ce repo documente la mise en place d'un pipeline **NetDevOps complet** dans un homelab, ciblant des switches **HPE Aruba AOS-CX** et des routeurs virtuels **Juniper vJunos**.

Il couvre 6 labs Ansible progressifs, une configuration Terraform, et un pipeline GitLab CI/CD en 4 stages — le tout hébergé sur une instance GitLab self-hosted et mirroré sur GitHub.

---

### Stack technique

| Composant | Solution |
|-----------|----------|
| Hyperviseur | Proxmox VE (`pve1.your-domain.com`) |
| CI/CD | GitLab CE (`gitlab.your-domain.com`) |
| Automatisation | Ansible + collection `arubanetworks.aoscx` |
| Infrastructure as Code | Terraform + provider `aruba/aoscx` |
| Switch cible | HPE Aruba AOS-CX (REST API port 443) |
| Routeurs virtuels | Juniper vJunos (EVE-NG sur Proxmox) — Phase 2 |
| VM d'automation | Ubuntu Server (`automation.your-domain.com`) |

---

### Structure du repo

```
netdevops/
├── ansible/                     ← Playbooks AOS-CX (Labs 01→06)
│   ├── README.md
│   ├── ansible.cfg
│   ├── hosts.yml
│   ├── group_vars/
│   └── labs/
│       ├── 01-vlan-management/
│       ├── 02-l2-campus/
│       ├── 03-l3-ospf/
│       ├── 04-vsx-l2/
│       ├── 05-vsx-l3-campus/
│       └── 06-datacenter-collapsed-core/
├── terraform/                   ← Infrastructure as Code AOS-CX
│   └── README.md
├── docs/
│   ├── gitlab-cicd/             ← Pipeline CI/CD (4 stages)
│   │   └── README.md
│   ├── aruba/                   ← Ressources & références Aruba
│   │   └── README.md
│   └── juniper/                 ← Labs vJunos (Phase 2)
│       └── README.md
└── assets/
    └── Logo_Luconik.png
```

---

### Labs AOS-CX

| Lab | Topologie | Complexité |
|-----|-----------|------------|
| [01 — VLAN Management](ansible/labs/01-vlan-management/) | Access & Trunk | ⭐ |
| [02 — Campus 2-Tier L2](ansible/labs/02-l2-campus/) | VSX + ISL + MCLAG + Active Gateway | ⭐⭐ |
| [03 — Campus 2-Tier L3](ansible/labs/03-l3-ospf/) | OSPF Routed Access + SVIs | ⭐⭐⭐ |
| [04 — VSX L2 pur](ansible/labs/04-vsx-l2/) | VSX + MSTP + MCLAG | ⭐⭐ |
| [05 — Campus 3-Tier](ansible/labs/05-vsx-l3-campus/) | VSX Aggregation + Core + OSPF | ⭐⭐⭐⭐ |
| [06 — DC Collapsed Core](ansible/labs/06-datacenter-collapsed-core/) | MCLAG + Jumbo MTU + OSPF upstream | ⭐⭐⭐⭐ |

---

### Pipeline CI/CD

```
[lint] → [ansible-deploy] → [terraform-plan] → [terraform-apply]
```

| Stage | Déclencheur |
|-------|-------------|
| **lint** | Automatique sur push |
| **ansible-deploy** | Auto sur `lab/*`, manuel sur `main` |
| **terraform-plan** | Manuel sur `main` |
| **terraform-apply** | Manuel sur `main` après review du plan |

→ Voir [`docs/gitlab-cicd/`](docs/gitlab-cicd/)

---

### Démarrage rapide

```bash
# Cloner le repo
git clone https://github.com/Luconik/netdevops.git
cd netdevops

# Installer la collection Ansible AOS-CX
ansible-galaxy collection install arubanetworks.aoscx

# Lancer le lab 01
cd ansible/
ansible-playbook -i hosts.yml labs/01-vlan-management/site.yml
```

---

### Documentation

| Section | Description |
|---------|-------------|
| [`ansible/`](ansible/) | 6 labs AOS-CX — commandes, modules, topologies |
| [`terraform/`](terraform/) | Provider aruba/aoscx, state GitLab, exemples HCL |
| [`docs/gitlab-cicd/`](docs/gitlab-cicd/) | Pipeline 4 stages, variables CI/CD, workflow Git |
| [`docs/aruba/`](docs/aruba/) | Portails HPE, modules Ansible, liens croisés |
| [`docs/juniper/`](docs/juniper/) | Labs vJunos Phase 2 — NETCONF, PyEZ |

---

### Repos liés

| Repo | Description |
|------|-------------|
| [`homelab-setup`](https://github.com/Luconik/homelab-setup) | Infrastructure homelab (Proxmox, EVE-NG, Docker, GitLab) |
| [`hpe-aruba-guides`](https://github.com/Luconik/hpe-aruba-guides) | GreenLake SSO, NAC + Intune, Central workspace |

---
---

<a name="en"></a>
## 🇬🇧 English

### Overview

This repo documents a complete **NetDevOps pipeline** built in a homelab, targeting **HPE Aruba AOS-CX** switches and **Juniper vJunos** virtual routers.

It covers 6 progressive Ansible labs, Terraform configuration, and a 4-stage GitLab CI/CD pipeline — hosted on a self-managed GitLab instance and mirrored to GitHub.

---

### Tech stack

| Component | Solution |
|-----------|----------|
| Hypervisor | Proxmox VE |
| CI/CD | GitLab CE (self-hosted) |
| Automation | Ansible + `arubanetworks.aoscx` collection |
| IaC | Terraform + `aruba/aoscx` provider |
| Target switch | HPE Aruba AOS-CX (REST API port 443) |
| Virtual routers | Juniper vJunos (EVE-NG on Proxmox) — Phase 2 |
| Automation VM | Ubuntu Server |

---

### AOS-CX Labs

| Lab | Topology | Complexity |
|-----|----------|------------|
| 01 — VLAN Management | Access & Trunk | ⭐ |
| 02 — Campus 2-Tier L2 | VSX + ISL + MCLAG + Active GW | ⭐⭐ |
| 03 — Campus 2-Tier L3 | OSPF Routed Access + SVIs | ⭐⭐⭐ |
| 04 — VSX L2 | VSX + MSTP + MCLAG | ⭐⭐ |
| 05 — Campus 3-Tier | VSX Agg + Core + OSPF | ⭐⭐⭐⭐ |
| 06 — DC Collapsed Core | MCLAG + Jumbo MTU + OSPF | ⭐⭐⭐⭐ |

---

### Quick start

```bash
git clone https://github.com/Luconik/netdevops.git
cd netdevops
ansible-galaxy collection install arubanetworks.aoscx
cd ansible/
ansible-playbook -i hosts.yml labs/01-vlan-management/site.yml
```

---

### Related repos

| Repo | Description |
|------|-------------|
| [`homelab-setup`](https://github.com/Luconik/homelab-setup) | Homelab infrastructure (Proxmox, EVE-NG, Docker, GitLab) |
| [`hpe-aruba-guides`](https://github.com/Luconik/hpe-aruba-guides) | GreenLake SSO, NAC + Intune, Central workspace |

---

*Last updated: March 2026 — [@Luconik](https://github.com/Luconik)*
