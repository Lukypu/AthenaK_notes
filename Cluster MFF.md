---
modified: 2026-07-28
created: 2026-07-28
---
## Overview
Compromises of different clusters glued together –> possible communication though not recommended to use more than necessary.

Main cluster for physics – Chimera
- ffa – free for all partition –> used by anyone, jobs in queue per priority
- mff-pejcha partition – 'owned' by Pejcha

Manuals:
- code of conduct
- GitLab

### Access
Two options:
- JupyterHUB – good for interactivity
- SSH – old n good, uses CAS credentials

### Queue
Managed by 'SLURM' system

### Workflow
Basic:
- Have an executable
- Write a bash script with SLURM commands to reserve resources and shell commands
- run and pray (might be improved into future)