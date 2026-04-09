# netdevops — HPE Networking Lab Repository

> 🇫🇷 Français | 🇬🇧 [English](#netdevops--hpe-networking-lab-repository-en)

---

## 🇫🇷 Français

Ce repo regroupe des labs réseau reproductibles et des guides NetDevOps pour les deux gammes **HPE Networking** — **Aruba AOS-CX** et **Juniper** — avec un focus sur Containerlab, Ansible et Terraform.

Les labs sont conçus pour les ingénieurs réseau, presales et partenaires intégrateurs qui souhaitent pratiquer sans matériel physique.

> 📖 **Guide de création de compte HPE Networking** (requis pour AOS-CX Simulator) :
> [github.com/Luconik/hpe-aruba-guides/tree/main/nsp-account](https://github.com/Luconik/hpe-aruba-guides/tree/main/nsp-account)

---

### 📦 Containerlab

Labs réseau virtualisés avec [Containerlab](https://containerlab.dev).

| Lab | Technologie | Switches | Statut |
|-----|------------|---------|--------|
| [aoscx-vsx](containerlab/aoscx-vsx/) | HPE Aruba AOS-CX VSX | 2x AOS-CX 10.16 | ✅ Validé |
| [vjunos-mclag](containerlab/vjunos-mclag/) | Juniper vJunos-switch MC-LAG | 2x EX9214 virt. + 2x Ubuntu | ✅ Validé |

#### Prérequis communs

- Linux bare metal recommandé (Fedora 43 / Ubuntu 24.04)
- [Containerlab](https://containerlab.dev/install/) ≥ 0.74
- Docker
- Extension VS Code **Containerlab** by srl-labs (optionnel)

#### Images requises

| Image | Source | Compte requis |
|-------|--------|--------------|
| `vrnetlab/vr-aoscx:10.16.1020` | [networking.hpe.com](https://networking.hpe.com) — AOS-CX Switch Simulator | ✅ Oui ([guide](https://github.com/Luconik/hpe-aruba-guides/tree/main/nsp-account)) |
| `vrnetlab/juniper_vjunos-switch:25.4R1.12` | [juniper.net/us/en/dm/vjunos-labs.html](https://www.juniper.net/us/en/dm/vjunos-labs.html) | ❌ Non |

---

### 🔧 À venir

| Lab | Technologie | Statut |
|-----|------------|--------|
| Ansible — AOS-CX VSX | Automatisation config VSX | 🔜 En cours |
| Ansible — Juniper MC-LAG | Automatisation config MC-LAG | 🔜 Planifié |
| Terraform — AOS-CX | Infrastructure as Code | 🔜 Planifié |

---

### 👤 Auteur

**Nicolas Culetto** — Channel Presales Consultant, HPE Aruba Networking

- 🔗 [linkedin.com/in/nicolasculetto](https://linkedin.com/in/nicolasculetto)
- 🐙 [github.com/Luconik](https://github.com/Luconik)
- 📖 [hpe-aruba-guides](https://github.com/Luconik/hpe-aruba-guides)

---

## 🇬🇧 English {#netdevops--hpe-networking-lab-repository-en}

This repo contains reproducible network labs and NetDevOps guides for both **HPE Networking** product lines — **Aruba AOS-CX** and **Juniper** — with a focus on Containerlab, Ansible and Terraform.

Labs are designed for network engineers, presales and integrator partners who want to practice without physical hardware.

> 📖 **HPE Networking account creation guide** (required for AOS-CX Simulator):
> [github.com/Luconik/hpe-aruba-guides/tree/main/nsp-account](https://github.com/Luconik/hpe-aruba-guides/tree/main/nsp-account)

---

### 📦 Containerlab

Virtualized network labs using [Containerlab](https://containerlab.dev).

| Lab | Technology | Switches | Status |
|-----|-----------|---------|--------|
| [aoscx-vsx](containerlab/aoscx-vsx/) | HPE Aruba AOS-CX VSX | 2x AOS-CX 10.16 | ✅ Validated |
| [vjunos-mclag](containerlab/vjunos-mclag/) | Juniper vJunos-switch MC-LAG | 2x EX9214 virt. + 2x Ubuntu | ✅ Validated |

#### Common Prerequisites

- Bare metal Linux recommended (Fedora 43 / Ubuntu 24.04)
- [Containerlab](https://containerlab.dev/install/) ≥ 0.74
- Docker
- VS Code **Containerlab** extension by srl-labs (optional)

#### Required Images

| Image | Source | Account required |
|-------|--------|-----------------|
| `vrnetlab/vr-aoscx:10.16.1020` | [networking.hpe.com](https://networking.hpe.com) — AOS-CX Switch Simulator | ✅ Yes ([guide](https://github.com/Luconik/hpe-aruba-guides/tree/main/nsp-account)) |
| `vrnetlab/juniper_vjunos-switch:25.4R1.12` | [juniper.net/us/en/dm/vjunos-labs.html](https://www.juniper.net/us/en/dm/vjunos-labs.html) | ❌ No |

---

### 🔧 Coming Soon

| Lab | Technology | Status |
|-----|-----------|--------|
| Ansible — AOS-CX VSX | VSX config automation | 🔜 In progress |
| Ansible — Juniper MC-LAG | MC-LAG config automation | 🔜 Planned |
| Terraform — AOS-CX | Infrastructure as Code | 🔜 Planned |

---

### 👤 Author

**Nicolas Culetto** — Channel Presales Consultant, HPE Aruba Networking

- 🔗 [linkedin.com/in/nicolasculetto](https://linkedin.com/in/nicolasculetto)
- 🐙 [github.com/Luconik](https://github.com/Luconik)
- 📖 [hpe-aruba-guides](https://github.com/Luconik/hpe-aruba-guides)
