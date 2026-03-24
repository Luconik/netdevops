<div align="center">

![Luconik Banner](assets/Logo_Luconik.png)

# netdevops

**Nicolas Culetto — Pre-Sales Systems Engineer @ HPE Aruba Networking**

*Network Automation · Infrastructure as Code · GitLab CI/CD*

[![LinkedIn](https://img.shields.io/badge/LinkedIn-nicolasculetto-0077B5?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/nicolasculetto/)
[![GitHub](https://img.shields.io/badge/GitHub-Luconik-181717?style=flat-square&logo=github)](https://github.com/Luconik)
[![Ansible](https://img.shields.io/badge/Automation-Ansible-EE0000?style=flat-square&logo=ansible)](https://www.ansible.com/)
[![Terraform](https://img.shields.io/badge/IaC-Terraform-7B42BC?style=flat-square&logo=terraform)](https://www.terraform.io/)
[![GitLab](https://img.shields.io/badge/CI/CD-GitLab-FC6D26?style=flat-square&logo=gitlab)](https://gitlab.com/)

---

> 🇫🇷 Documentation principale en français — un résumé en anglais est disponible dans chaque section.  
> 🇬🇧 Main documentation in French — an English summary is available in each section.

</div>

---

## À propos

Ce dépôt regroupe des labs d'automatisation réseau basés sur **Ansible** et **Terraform**, intégrés dans un pipeline **GitLab CI/CD** self-hosted.

Les labs couvrent trois plateformes :
- 🟠 **HPE Aruba AOS-CX** — switching enterprise (VSX, OSPF, VLAN, Campus, Datacenter)
- 🔵 **Juniper vJunos** — routing/switching virtualisé (NETCONF, Junos)
- 🟡 **Proxmox VE** — automatisation de l'hyperviseur homelab via API

> **EN** — This repository contains network automation labs using Ansible and Terraform, integrated in a self-hosted GitLab CI/CD pipeline. Labs cover three platforms: HPE Aruba AOS-CX switching, Juniper vJunos routing, and Proxmox VE hypervisor automation.

---

## Stack technique

| Composant | Technologie | Rôle |
|-----------|-------------|------|
| Automatisation | Ansible + collection `arubanetworks.aoscx` | Playbooks AOS-CX |
| IaC | Terraform + provider `arubanetworks/aoscx` | Provisioning déclaratif |
| CI/CD | GitLab CE (`gitlab.culetto.fr`) | Pipeline lint → deploy → plan → apply |
| Runner | automation.culetto.fr | Exécution des jobs GitLab |
| Émulation réseau | EVE-NG (VM Proxmox) | Switches AOS-CX OVA + vJunos |
| Hyperviseur | Proxmox VE (`pve1.culetto.fr`) | Infrastructure homelab |

> **EN** — The stack uses Ansible for AOS-CX configuration automation, Terraform for declarative provisioning, and a self-hosted GitLab instance for CI/CD. All network devices run as virtual machines inside EVE-NG on a Proxmox VE host.

---

## Structure du dépôt

```
netdevops/
├── ansible/
│   ├── ansible.cfg                  # Configuration Ansible
│   ├── hosts.yml                    # Inventaire (switches AOS-CX)
│   ├── group_vars/
│   │   └── all.yml                  # Variables globales
│   └── labs/
│       ├── 01-vlan-management/      # VLAN Access & Trunk
│       ├── 02-l2-campus/            # Campus 2-Tier L2 VSX + MCLAG
│       ├── 03-l3-ospf/              # Campus 2-Tier L3 OSPF Routed Access
│       ├── 04-vsx-l2/               # VSX L2 pur — ISL + MCLAG + MSTP
│       ├── 05-vsx-l3-campus/        # Campus 3-Tier VSX + OSPF
│       └── 06-datacenter-collapsed-core/  # DC Collapsed Core — OSPF + Jumbo MTU
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── juniper/                         # Labs vJunos (NETCONF)
│   └── labs/
├── proxmox/                         # Automatisation Proxmox API
│   └── labs/
├── docs/                            # Guides d'installation et de configuration
└── .gitlab-ci.yml                   # Pipeline CI/CD
```

> **EN** — The repository is split into three platform sections (ansible for AOS-CX, juniper for vJunos, proxmox for hypervisor automation), plus a Terraform directory and a GitLab CI/CD pipeline at the root.

---

## Labs HPE Aruba AOS-CX

| Lab | Topologie | Protocoles | Outil |
|-----|-----------|------------|-------|
| [01 — VLAN Management](ansible/labs/01-vlan-management/) | 1 switch | L2, Access, Trunk | Ansible |
| [02 — Campus 2-Tier L2](ansible/labs/02-l2-campus/) | VSX Core + 2 Access | MCLAG, Active GW | Ansible |
| [03 — Campus 2-Tier L3 OSPF](ansible/labs/03-l3-ospf/) | VSX Core + 2 Access | OSPF, Routed Access | Ansible |
| [04 — VSX L2](ansible/labs/04-vsx-l2/) | 2 switches VSX | ISL, MCLAG, MSTP | Ansible |
| [05 — Campus 3-Tier VSX OSPF](ansible/labs/05-vsx-l3-campus/) | Core + VSX Agg + Access | OSPF, Active GW, MCLAG | Ansible |
| [06 — DC Collapsed Core](ansible/labs/06-datacenter-collapsed-core/) | VSX DC-Core + CORE | OSPF, Active GW, Jumbo MTU | Ansible |

> **EN** — Six progressive AOS-CX labs from basic VLAN management to full datacenter collapsed core topology with VSX, OSPF, and jumbo MTU. Each lab includes an Ansible playbook, a topology diagram, and verification commands.

---

## Labs Juniper vJunos

| Lab | Description | Outil |
|-----|-------------|-------|
| 01 — Déploiement vJunos | Installation et configuration initiale sur EVE-NG | Manuel |
| 02 — NETCONF basics | Premiers appels NETCONF via Python/Ansible | Ansible |
| 03 — BGP vJunos ↔ AOS-CX | Peering BGP entre Juniper et Aruba | Ansible |

> **EN** — Juniper vJunos labs running on EVE-NG. Covers initial deployment, NETCONF automation basics, and interoperability scenarios with HPE Aruba AOS-CX.

📋 Labs en cours de rédaction

---

## Automatisation Proxmox

| Lab | Description | Outil |
|-----|-------------|-------|
| 01 — API Proxmox | Interaction avec l'API REST Proxmox | Ansible |
| 02 — Scheduler VMs | Démarrage/arrêt automatique de VMs | Ansible + n8n |

> **EN** — Proxmox automation labs using the REST API. Covers VM lifecycle management and integration with n8n workflows for scheduled operations.

📋 Labs en cours de rédaction

---

## Pipeline CI/CD

```
┌─────────┐    ┌────────────────┐    ┌──────────────────┐    ┌──────────────────┐
│  lint   │───►│ ansible-deploy │───►│  terraform-plan  │───►│ terraform-apply  │
│         │    │  (manuel/auto) │    │   (sur main)     │    │    (manuel)      │
└─────────┘    └────────────────┘    └──────────────────┘    └──────────────────┘
```

- **lint** : ansible-lint + yamllint sur tous les playbooks
- **ansible-deploy** : automatique sur branches `lab/*`, manuel sur `main`
- **terraform-plan** : plan affiché dans la MR pour review
- **terraform-apply** : déclenchement manuel uniquement

> **EN** — The GitLab CI/CD pipeline runs on every push: linting first, then Ansible deployment (auto on lab branches, manual on main), followed by Terraform plan and apply stages.

---

## Architecture homelab

```
pve1.culetto.fr (Proxmox VE — Intel NUC)
│
├── VM : gitlab.culetto.fr       ← Dépôt Git + Orchestrateur CI/CD
├── VM : automation.culetto.fr   ← Runner GitLab + Ansible + Terraform
└── VM : EVE-NG
         ├── Switches AOS-CX (OVA 10.16)
         └── Routeurs vJunos
```

> **EN** — All lab infrastructure runs on a single Intel NUC with Proxmox VE. GitLab, the automation VM (Ansible + Terraform runner), and EVE-NG are separate VMs. AOS-CX and vJunos devices run inside EVE-NG as virtual appliances.

---

## Démarrage rapide

### Prérequis

- VM `automation.culetto.fr` configurée (voir [homelab-setup](https://github.com/Luconik/homelab-setup))
- Switch AOS-CX dans EVE-NG avec REST API activée :

```bash
switch(config)# https-server rest access-mode read-write
switch(config)# https-server vrf mgmt
```

### Lancer un lab Ansible

```bash
# Sur automation.culetto.fr
source /opt/ansible-aruba/venv/bin/activate
cd ~/netdevops/ansible

# Vérifier la connectivité
ansible-playbook -i hosts.yml check_facts.yml

# Lancer le lab 01
ansible-playbook -i hosts.yml labs/01-vlan-management/site.yml
```

---

## Repo associé

| Repo | Description |
|------|-------------|
| [homelab-setup](https://github.com/Luconik/homelab-setup) | Infrastructure homelab — Proxmox, EVE-NG, Docker, n8n, Security |

---

## Contact

- 🔗 **LinkedIn** : [linkedin.com/in/nicolasculetto](https://www.linkedin.com/in/nicolasculetto/)
- 📧 **Email** : nicolas@culetto.fr
- 🐙 **GitHub** : [github.com/Luconik](https://github.com/Luconik)

---

> Les guides sont librement réutilisables avec mention de l'auteur (CC BY 4.0).

<div align="center">

*Made with ❤️ and too much coffee — Nicolas Culetto / Luconik*

[![LinkedIn](https://img.shields.io/badge/LinkedIn-nicolasculetto-0077B5?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/nicolasculetto/)

</div>
