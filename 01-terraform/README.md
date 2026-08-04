# Phase 01 — Terraform

## Start here

- **Progress checklist:** [CURRENT PHASE — Terraform](https://github.com/en4ble1337/terra-kube-ansible/issues/1)
- **Target credential:** HashiCorp Certified Terraform Associate (004)
- **Primary platform:** Proxmox
- **Lab outcome:** Terraform creates one Ubuntu control-plane VM and two Ubuntu worker VMs, exposes their addresses, detects changes, destroys them, and recreates them cleanly.
- **Estimated effort:** 10 sessions × approximately 3 hours

This file is the Terraform learning guide. Follow the sessions in order.

## Course strategy

Use your Udemy courses in this order:

1. **Primary course:** *HashiCorp Certified Terraform Associate 004 Exam Prep Course* — Bryan Krausen.
2. **Hands-on supplement:** *Terraform for the Absolute Beginners with Labs* — KodeKloud. Use it when a concept is not yet intuitive or you need another guided exercise.
3. **Final assessment:** *HashiCorp Certified Terraform Associate 004 — Practice Exams* — Bryan Krausen.
4. **Reference only:** *Terraform: The Complete Guide from Beginner to Expert*. Do not complete it in parallel unless a specific topic requires deeper treatment.

Also use HashiCorp's official Associate 004 learning path and exam-content list during the final review.

## What not to do

- Do not begin with the old Proxmox LXC guide.
- Do not copy a complete Terraform project and call the phase finished.
- Do not commit credentials, live `.tfvars`, or state files.
- Do not build remote state, CI/CD, or complex module hierarchies before the local workflow works.
- Do not advance to Ansible until the rebuild completion gate passes.

## Planned working layout

Create these paths as you reach them:

```text
01-terraform/
├── README.md
├── labs/
│   ├── 01-basics/
│   └── proxmox-kubernetes-vms/
│       ├── versions.tf
│       ├── providers.tf
│       ├── variables.tf
│       ├── locals.tf
│       ├── main.tf
│       ├── outputs.tf
│       ├── terraform.tfvars.example
│       └── modules/
│           └── proxmox-vm/
├── notes/
│   ├── lab-environment.md
│   ├── provider-decision.md
│   └── state-and-lifecycle.md
└── evidence/
    └── completion.md
```

---

## Session 1 — Orientation, installation, and core workflow

### Learn

Complete the matching introductory material in the Bryan Krausen course:

- Infrastructure as Code
- Terraform architecture and workflow
- Terraform CLI
- HCL configuration basics
- Providers and resources

Understand these commands before moving on:

```text
terraform fmt
terraform init
terraform validate
terraform plan
terraform apply
terraform destroy
```

### Build

On an Ubuntu 22.04/24.04 or Debian management machine, install Terraform from HashiCorp's package repository. Then validate the tools:

```bash
terraform version && git --version
```

Create `01-terraform/labs/01-basics/` and build a harmless local exercise that demonstrates init, plan, apply, output, and destroy without touching Proxmox.

### Validate

```bash
cd 01-terraform/labs/01-basics
terraform fmt -check -recursive
terraform init
terraform validate
terraform plan
```

### Evidence

Record the installed Terraform version and a short explanation of each core command in `01-terraform/evidence/completion.md`.

### Exit criteria

You can explain what Terraform changes during init, plan, apply, and destroy without reading the documentation.

---

## Session 2 — Variables, types, outputs, locals, and expressions

### Learn

Study:

- Input variables and validation
- Primitive and collection types
- Lists, maps, objects, and sets
- Local values
- Outputs
- Resource references
- `for_each`, conditionals, and functions
- Variable precedence and sensitive values

### Build

Expand the basics lab so configuration is driven by variables and a map or object rather than duplicated resource blocks. Add useful outputs and at least one variable validation rule.

### Validate

```bash
terraform fmt -check -recursive
terraform validate
terraform plan -var-file=terraform.tfvars.example
terraform output
```

### Exit criteria

You can explain when to use a variable, local, output, and resource reference, and you understand why marking a value sensitive does not encrypt it in state.

---

## Session 3 — State, lifecycle, dependencies, and troubleshooting

### Learn

Study:

- Terraform state purpose and risks
- Desired configuration versus recorded state versus real infrastructure
- Dependency graph
- Implicit and explicit dependencies
- Resource addressing
- Lifecycle operations
- Refresh behavior
- Import and state commands
- Local versus remote state

### Build

Create `01-terraform/notes/state-and-lifecycle.md`. Explain:

- why state exists;
- why state can contain secrets;
- why state is ignored in Git;
- what drift means;
- when import is appropriate;
- why `terraform state` commands require caution.

Practice inspecting state only in the local basics lab:

```bash
terraform state list
terraform show
terraform output -json
```

### Exit criteria

You can draw the relationship among configuration, state, provider APIs, and infrastructure.

---

## Session 4 — Proxmox discovery and provider decision

### Learn

Study provider source addressing, version constraints, authentication, provider configuration, and lock files.

### Build

Create `01-terraform/notes/lab-environment.md` and record:

- Proxmox version and node name
- management endpoint
- target storage pool
- snippets or ISO storage
- bridge and VLAN
- gateway and DNS
- three reserved IP addresses
- proposed VM IDs and hostnames
- CPU, RAM, and disk sizing

Create `01-terraform/notes/provider-decision.md` comparing the current BPG and Telmate Proxmox providers for your installed Proxmox version. Select one based on maintained functionality, VM and cloud-init support, documentation, and compatibility—not because an older guide already used it.

Create a least-privilege Proxmox API token and expose credentials to Terraform through environment variables or an ignored local file.

### Security rule

Never commit the API token, password, secret ID, or a populated `.tfvars` file.

### Exit criteria

The provider initializes successfully, the dependency lock file is committed, and the decision document explains why the provider was chosen.

---

## Session 5 — Ubuntu cloud-init template

### Learn

Understand cloning, cloud images, cloud-init, SSH keys, guest agents, VM templates, and the division of responsibility between Terraform and Ansible.

### Build

Create an Ubuntu 24.04 cloud-init template in Proxmox. Document every manual prerequisite that Terraform will consume, including:

- source cloud image;
- template VM ID and name;
- storage and disk configuration;
- cloud-init drive;
- network adapter and bridge;
- QEMU guest agent;
- serial console if required;
- default user and SSH-key behavior.

Terraform should clone the template. It should not repeatedly rebuild the base image during this phase.

### Validate

Clone one test VM manually or through a minimal Terraform resource and confirm:

```bash
ssh <user>@<test-vm-ip> 'hostnamectl && cloud-init status --wait && ip addr'
```

### Exit criteria

A clone boots, receives the intended network configuration, accepts SSH-key authentication, and reports cloud-init complete.

---

## Session 6 — First Proxmox VM as code

### Learn

Study resource arguments, computed attributes, timeouts, provider errors, and plan interpretation.

### Build

Create `01-terraform/labs/proxmox-kubernetes-vms/` with:

```text
versions.tf
providers.tf
variables.tf
main.tf
outputs.tf
terraform.tfvars.example
```

Provision one control-plane VM first. Keep the configuration explicit until it works; do not abstract it into a module prematurely.

### Validate

```bash
terraform fmt -check -recursive
terraform init
terraform validate
terraform plan
terraform apply
terraform plan
```

The second plan should show no unintended changes.

Confirm from the guest:

```bash
ssh <user>@<control-plane-ip> 'hostname && cloud-init status --wait && systemctl is-active qemu-guest-agent'
```

### Exit criteria

One VM is reproducibly created and a second plan is clean.

---

## Session 7 — Reusable VM module

### Learn

Study module structure, module inputs and outputs, module composition, versioning principles, and when not to create a module.

### Build

Move the working VM implementation into:

```text
01-terraform/labs/proxmox-kubernetes-vms/modules/proxmox-vm/
```

The module should accept only useful inputs, such as:

- VM name and ID
- clone/template ID
- CPU and memory
- disk and storage
- bridge, VLAN, address, gateway, and DNS
- cloud-init user and SSH public key
- tags

Avoid building a universal module with dozens of unused switches.

### Validate

Recreate the control-plane VM through the module and confirm the plan remains understandable.

### Exit criteria

The root module describes intent while the child module contains the repeatable VM implementation.

---

## Session 8 — Three-node Kubernetes VM foundation

### Build

Use a typed map of objects and `for_each` to provision:

- one control-plane VM;
- two worker VMs.

Add outputs for:

- hostname-to-IP mapping;
- SSH commands;
- control-plane address;
- worker addresses;
- data that can later generate an Ansible inventory.

### Validate

```bash
terraform fmt -check -recursive
terraform validate
terraform plan
terraform apply
terraform output
terraform output -json
terraform plan -detailed-exitcode
```

Expected `terraform plan -detailed-exitcode` result after apply:

```text
exit code 0 = no changes
exit code 2 = changes are present
exit code 1 = error
```

Confirm all three guests are reachable and uniquely named.

### Exit criteria

All three VMs are created from one data-driven configuration with predictable names and addresses.

---

## Session 9 — Change, drift, import, destroy, and rebuild

### Learn and test

Perform controlled lifecycle exercises:

1. Change one safe variable such as worker memory.
2. Review the plan and predict exactly what will change.
3. Apply it and verify only the intended resource changed.
4. Make one safe out-of-band change in Proxmox.
5. Run a plan and document the detected drift.
6. Restore or accept the intended state using a documented decision.
7. Practice `terraform import` only with a disposable test resource if the provider supports it cleanly.
8. Destroy the managed VMs.
9. Recreate them from the committed configuration.

### Evidence

Record commands, plan summaries, and your reasoning in `01-terraform/evidence/completion.md`.

### Exit criteria

You are comfortable reading a destructive plan and do not apply changes you cannot explain.

---

## Session 10 — Documentation, exam review, and completion gate

### Build the final runbook

The evidence file must contain:

- architecture summary;
- prerequisites;
- provider and version decisions;
- credential-loading method without secrets;
- initialization and deployment commands;
- expected outputs;
- validation commands;
- change and drift exercise results;
- destroy and rebuild results;
- known limitations;
- troubleshooting notes.

### Certification review

1. Review HashiCorp's Associate 004 learning path.
2. Review every item in the Associate 004 exam-content list.
3. Complete the Bryan Krausen practice exams.
4. For every missed question, write why the correct answer is correct and reproduce the concept in a small lab when possible.
5. Do not schedule the exam based on one memorized high score. Require repeated passing scores with understood reasoning.

Note that the Associate 004 exam targets Terraform 1.12 concepts. Your lab may use a newer supported CLI, but exam review should avoid assuming features added after the tested version.

## Final completion gate

From a clean checkout and an existing documented cloud-init template, you must be able to:

1. initialize Terraform;
2. validate the configuration;
3. produce an understandable plan;
4. create all three VMs;
5. show useful outputs;
6. produce a clean second plan;
7. destroy all managed VMs;
8. recreate them without undocumented manual changes.

When this passes, complete Terraform issue #1 and move to Phase 02.

## Official references

- [Terraform installation](https://developer.hashicorp.com/terraform/install)
- [Terraform documentation](https://developer.hashicorp.com/terraform/docs)
- [Terraform Associate 004 learning path](https://developer.hashicorp.com/terraform/tutorials/certification-004/associate-study-004)
- [Terraform Associate 004 exam content](https://developer.hashicorp.com/terraform/tutorials/certification-004/associate-review-004)
- [Terraform certifications](https://developer.hashicorp.com/certifications/infrastructure-automation)
