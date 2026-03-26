# Juniper — Labs vJunos & Références
# Juniper — vJunos Labs & References

> 🇫🇷 [Français](#fr) | 🇬🇧 [English](#en)

---

<a name="fr"></a>
## 🇫🇷 Français

### Présentation

Ce dossier documente les labs **Juniper vJunos** sur EVE-NG — complément aux labs AOS-CX, permettant de comparer les approches d'automatisation entre les deux écosystèmes.

Les labs Juniper utilisent la collection Ansible `juniper.device` via **PyEZ + NETCONF**.

> 🔜 Les playbooks Juniper sont en cours de développement — Phase 2 du repo `netdevops`.

---

### Stack technique Juniper

| Composant | Solution |
|-----------|----------|
| VM Juniper | vJunos (EVE-NG sur Proxmox) |
| Protocole | NETCONF over SSH |
| Collection Ansible | `juniper.device` |
| Librairies Python | PyEZ (`jnpr.junos`), ncclient |
| Virtualenv | `/opt/ansible-juniper/venv/` |

---

### Prérequis

```bash
# Créer un virtualenv dédié
python3 -m venv /opt/ansible-juniper/venv
source /opt/ansible-juniper/venv/bin/activate

# Installer les dépendances
pip install junos-eznc ncclient

# Installer la collection Ansible
ansible-galaxy collection install juniper.device

# Vérifier
python3 -c "import jnpr.junos; print('PyEZ OK')"
```

> ⚠️ PyEZ et ncclient doivent être dans le même virtualenv que celui utilisé par Ansible.

---

### Labs prévus

| Lab | Topologie | Protocole | Statut |
|-----|-----------|-----------|--------|
| 01 — Facts gathering | vJunos single node | NETCONF | 🔜 |
| 02 — VLANs & interfaces | vJunos single node | NETCONF | 🔜 |
| 03 — OSPF + BGP | vJunos 2 nodes | NETCONF | 🔜 |

---

### Structure prévue

```
ansible/labs/
├── juniper/
│   ├── ansible.cfg
│   ├── hosts.yml
│   └── labs/
│       ├── 01-facts/
│       │   └── site.yml
│       ├── 02-vlans-interfaces/
│       │   └── site.yml
│       └── 03-ospf-bgp/
│           └── site.yml
└── aruba/
    └── (labs 01→06 AOS-CX existants)
```

---

### Comparatif AOS-CX vs vJunos

| Aspect | AOS-CX (Aruba) | vJunos (Juniper) |
|--------|----------------|------------------|
| Collection Ansible | `arubanetworks.aoscx` | `juniper.device` |
| Protocole | REST API (HTTPS) | NETCONF (SSH) |
| Format config | YAML déclaratif | Junos config (set/hierarchical) |
| Rollback natif | Limité | ✅ `rollback 0` |
| Virtualenv requis | Non | ✅ PyEZ + ncclient |

---

### Ressources Juniper

| Ressource | URL |
|-----------|-----|
| Collection juniper.device | [galaxy.ansible.com/juniper/device](https://galaxy.ansible.com/juniper/device) |
| Junos PyEZ Documentation | [junos-pyez.readthedocs.io](https://junos-pyez.readthedocs.io) |
| Juniper vJunos | [juniper.net/vjunos](https://www.juniper.net/documentation/us/en/software/vjunos/) |
| NETCONF RFC | [RFC 6241](https://tools.ietf.org/html/rfc6241) |

---

### Liens croisés

- 🔗 [`../aruba/`](../aruba/) — Ressources Aruba AOS-CX
- 🔗 [`../../ansible/`](../../ansible/) — Labs AOS-CX Ansible
- 🔗 [`../gitlab-cicd/`](../gitlab-cicd/) — Pipeline CI/CD

---
---

<a name="en"></a>
## 🇬🇧 English

### Overview

This folder documents **Juniper vJunos** labs on EVE-NG — a complement to AOS-CX labs, allowing comparison of automation approaches between both ecosystems.

Juniper labs use the `juniper.device` Ansible collection via **PyEZ + NETCONF**.

> 🔜 Juniper playbooks are under development — Phase 2 of the `netdevops` repo.

### Juniper stack

| Component | Solution |
|-----------|----------|
| Juniper VM | vJunos (EVE-NG on Proxmox) |
| Protocol | NETCONF over SSH |
| Ansible collection | `juniper.device` |
| Python libraries | PyEZ (`jnpr.junos`), ncclient |
| Virtualenv | `/opt/ansible-juniper/venv/` |

### Prerequisites

```bash
python3 -m venv /opt/ansible-juniper/venv
source /opt/ansible-juniper/venv/bin/activate
pip install junos-eznc ncclient
ansible-galaxy collection install juniper.device
```

### Planned labs

| Lab | Topology | Protocol | Status |
|-----|----------|----------|--------|
| 01 — Facts gathering | vJunos single node | NETCONF | 🔜 |
| 02 — VLANs & interfaces | vJunos single node | NETCONF | 🔜 |
| 03 — OSPF + BGP | vJunos 2 nodes | NETCONF | 🔜 |

### AOS-CX vs vJunos comparison

| Aspect | AOS-CX (Aruba) | vJunos (Juniper) |
|--------|----------------|------------------|
| Ansible collection | `arubanetworks.aoscx` | `juniper.device` |
| Protocol | REST API (HTTPS) | NETCONF (SSH) |
| Config format | Declarative YAML | Junos config (set/hierarchical) |
| Native rollback | Limited | ✅ `rollback 0` |
| Virtualenv required | No | ✅ PyEZ + ncclient |

### References

- [juniper.device collection](https://galaxy.ansible.com/juniper/device)
- [Junos PyEZ Docs](https://junos-pyez.readthedocs.io)
- [`../aruba/`](../aruba/) — Aruba AOS-CX resources

---

*Last updated: March 2026 — [@Luconik](https://github.com/Luconik)*
