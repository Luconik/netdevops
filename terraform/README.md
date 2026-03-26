# Terraform — AOS-CX Infrastructure as Code
# Terraform — AOS-CX Infrastructure as Code

> 🇫🇷 [Français](#fr) | 🇬🇧 [English](#en)

---

<a name="fr"></a>
## 🇫🇷 Français

### Présentation

Ce dossier contient la configuration **Terraform** pour le provisioning d'équipements **HPE Aruba AOS-CX** via le provider officiel `aruba/aoscx`.

Terraform complète les playbooks Ansible en gérant l'**état de l'infrastructure** (state) et en permettant des plans d'exécution auditables avant tout changement.

### Prérequis

- Terraform >= 1.5
- Provider `aruba/aoscx` (installé automatiquement via `terraform init`)
- Switch AOS-CX avec REST API activée (port 443)
- Credentials dans les variables d'environnement ou `terraform.tfvars`

### Installation

```bash
cd terraform/

# Initialiser le provider
terraform init

# Vérifier la configuration
terraform validate

# Voir le plan (dry-run)
terraform plan

# Appliquer
terraform apply
```

---

### Structure

```
terraform/
├── README.md
├── main.tf          ← ressources principales (VLANs, interfaces, OSPF...)
├── variables.tf     ← déclaration des variables
├── outputs.tf       ← sorties (IPs, IDs...)
└── terraform.tfvars.example  ← exemple de valeurs (ne jamais committer tfvars)
```

---

### Variables principales

| Variable | Description | Exemple |
|----------|-------------|---------|
| `switch_ip` | IP du switch AOS-CX | `10.224.100.180` |
| `switch_username` | Utilisateur REST API | `admin` |
| `switch_password` | Mot de passe REST API | `admin` ⚠️ |
| `switch_port` | Port REST API | `443` |

Créer un fichier `terraform.tfvars` (non commité) :

```hcl
switch_ip       = "10.224.100.180"
switch_username = "admin"
switch_password = "votre_mot_de_passe"
switch_port     = 443
```

> ⚠️ Ne jamais committer `terraform.tfvars` — il est dans le `.gitignore`.

---

### Exemple de ressources

```hcl
# Créer un VLAN
resource "aoscx_vlan" "vlan_100" {
  vlan_id     = 100
  name        = "MGMT"
  description = "Management VLAN"
}

# Configurer une interface L2
resource "aoscx_interface" "port_1_1" {
  name        = "1/1/1"
  description = "Uplink vers Core"
  vlan_mode   = "trunk"
  vlan_ids    = [100, 200, 300]
}
```

---

### State backend GitLab

L'état Terraform est géré dans le **backend HTTP natif de GitLab** pour le partage d'état entre pipelines :

```hcl
terraform {
  backend "http" {
    address        = "https://gitlab.your-domain.com/api/v4/projects/<PROJECT_ID>/terraform/state/netdevops"
    lock_address   = "https://gitlab.your-domain.com/api/v4/projects/<PROJECT_ID>/terraform/state/netdevops/lock"
    unlock_address = "https://gitlab.your-domain.com/api/v4/projects/<PROJECT_ID>/terraform/state/netdevops/lock"
    username       = "gitlab-ci-token"
    password       = "${CI_JOB_TOKEN}"
    lock_method    = "POST"
    unlock_method  = "DELETE"
    retry_wait_min = 5
  }
}
```

---

### Commandes utiles

```bash
# Initialiser
terraform init

# Valider la syntaxe
terraform validate

# Voir le plan
terraform plan -out=tfplan

# Appliquer le plan
terraform apply tfplan

# Détruire les ressources
terraform destroy

# Voir l'état actuel
terraform show

# Importer une ressource existante
terraform import aoscx_vlan.vlan_100 100
```

---

### Références

- [Provider aruba/aoscx — Terraform Registry](https://registry.terraform.io/providers/aruba/aoscx/latest)
- [HPE Aruba AOS-CX Terraform Documentation](https://developer.arubanetworks.com/aruba-aoscx/docs/terraform-getting-started)
- [`../ansible/`](../ansible/) — Playbooks Ansible AOS-CX
- [`../docs/gitlab-cicd/`](../docs/gitlab-cicd/) — Pipeline CI/CD avec backend Terraform
- 🔗 [`homelab-setup/ubuntu-server/`](https://github.com/Luconik/homelab-setup/tree/main/ubuntu-server) — Installation Terraform 1.5 sur la VM automation

---
---

<a name="en"></a>
## 🇬🇧 English

### Overview

This folder contains **Terraform** configuration for provisioning **HPE Aruba AOS-CX** switches using the official `aruba/aoscx` provider.

Terraform complements Ansible playbooks by managing **infrastructure state** and providing auditable execution plans before any change.

### Prerequisites

- Terraform >= 1.5
- `aruba/aoscx` provider (auto-installed via `terraform init`)
- AOS-CX switch with REST API enabled (port 443)
- Credentials in environment variables or `terraform.tfvars`

### Quick start

```bash
cd terraform/
terraform init
terraform plan
terraform apply
```

### Key variables

| Variable | Description | Example |
|----------|-------------|---------|
| `switch_ip` | AOS-CX switch IP | `10.224.100.180` |
| `switch_username` | REST API username | `admin` |
| `switch_password` | REST API password | `admin` ⚠️ |
| `switch_port` | REST API port | `443` |

> ⚠️ Never commit `terraform.tfvars` — it's in `.gitignore`.

### GitLab state backend

Terraform state is managed in the **GitLab native HTTP backend** for pipeline state sharing. See FR section for full configuration.

### References

- [aruba/aoscx provider — Terraform Registry](https://registry.terraform.io/providers/aruba/aoscx/latest)
- [HPE Aruba AOS-CX Terraform Docs](https://developer.arubanetworks.com/aruba-aoscx/docs/terraform-getting-started)
- [`../ansible/`](../ansible/) — Ansible AOS-CX playbooks
- [`../docs/gitlab-cicd/`](../docs/gitlab-cicd/) — CI/CD pipeline
- 🔗 [`homelab-setup/ubuntu-server/`](https://github.com/Luconik/homelab-setup/tree/main/ubuntu-server) — Terraform 1.5 install on automation VM

---

*Last updated: March 2026 — [@Luconik](https://github.com/Luconik)*
