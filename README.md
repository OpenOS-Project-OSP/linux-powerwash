[update-readmes]   Mode: rewrite — migrating to template structure...
# linux-powerwash

[![Built with Ona](https://ona.com/build-with-ona.svg)](https://app.ona.com/#https://github.com/OpenOS-Project-OSP/linux-powerwash) [![KDE Eco](https://img.shields.io/badge/KDE%20Eco-certified-brightgreen?logo=kde&logoColor=white&style=flat-square)](https://eco.kde.org/) [![Blue Angel](https://img.shields.io/badge/Blue%20Angel-DE--UZ%20215-0055a4?style=flat-square)](https://www.blauer-engel.de/en/certification/criteria) [![Energy](https://api.green-coding.io/v1/ci/badge/get?repo=OpenOS-Project-OSP%2Flinux-powerwash&branch=main&workflow=eco-audit.yml)](https://metrics.green-coding.io/ci-index.html)


<!-- AI:start:what-it-does -->
This project provides a distro-agnostic and filesystem-agnostic tool for performing factory resets on Linux systems. It supports multiple reset modes, such as soft, medium, and hard resets, and includes plugins for handling specific distributions, filesystems, and hardware configurations. System administrators and developers use it to restore Linux environments to a clean state while preserving flexibility across different setups.
<!-- AI:end:what-it-does -->

## Architecture

<!-- AI:start:architecture -->
Linux Powerwash consists of modular shell scripts organized into directories based on functionality. The main components include:

- **Binary**: The `bin/powerwash` script serves as the entry point for executing the tool.
- **Libraries**: Core functionality is implemented in reusable shell scripts located in `lib/`. These include utilities for handling distributions, filesystems, backups, and plugins.
- **Modes**: Reset modes are defined in `modes/`, each implementing a specific level of reset (e.g., soft, medium, hard).
- **Plugins**: Optional extensions are stored in `plugins/`, categorized by functionality (e.g., `distro`, `filesystem`, `hardware`).
- **Systemd Integration**: Systemd service files and helpers are in `systemd/` for managing system-level tasks.
- **Configuration**: Default configuration files are stored in `/etc/powerwash`.

The directory structure is as follows:

```plaintext
.
├── bin
│   └── powerwash
├── lib
│   ├── common.sh
│   ├── distro.sh
│   ├── filesystem.sh
│   ├── backup.sh
│   └── plugin.sh
├── modes
│   ├── soft.sh
│   ├── medium.sh
│   ├── hard.sh
│   ├── sysprep.sh
│   └── hardware.sh
├── plugins
│   ├── distro
│   │   └── ubuntu-ppa.sh
│   ├── filesystem
│   │   └── btrfs-snapshot.sh
│   └── hardware
│       └── amd-gpu.sh
├── systemd
│   ├── powerwash-rebind.service
│   ├── powerwash-rebind.conf
│   └── rebind-helper
├── docs
├── dep-graph
└── README.md
```
<!-- AI:end:architecture -->

## Install

<!-- Add installation instructions here. This section is yours — the AI will not modify it. -->

```bash
git clone https://github.com/Interested-Deving-1896/linux-powerwash.git
cd linux-powerwash
```

## Usage


```
powerwash [--dry-run] <command> [options]
```

### Reset modes

```bash
# Clear user dotfiles only — safe, non-destructive
sudo powerwash soft

# Remove user-installed packages and reset dotfiles
sudo powerwash medium

# Full factory reset: packages, home dirs, system config
sudo powerwash hard

# Generalize for disk imaging (clears machine-id, SSH keys, network state)
sudo powerwash sysprep --shutdown
```

### Preview before committing

Every command supports `--dry-run`. Nothing is modified:

```bash
sudo powerwash --dry-run hard
```

### Backup

```bash
# Create a backup before resetting manually
sudo powerwash backup create

# Create an encrypted backup
sudo powerwash backup create --encrypt

# List backups
sudo powerwash backup list

# Restore packages from a backup
sudo powerwash backup restore-packages /var/lib/powerwash/backups/backup_20240101_120000
```

### Filesystem snapshots

```bash
# Create a snapshot (btrfs or ZFS only)
sudo powerwash snapshot create my-label

# List snapshots
sudo powerwash snapshot list

# Roll back
sudo powerwash snapshot rollback my-label
```

### Hardware device reset

```bash
# List devices and their sysfs BUS IDs
sudo powerwash hardware list

# Rebind a USB controller after resume
sudo powerwash hardware rebind 0000:00:14.0

# Trigger AMD GPU vendor-reset (requires gnif/vendor-reset module)
sudo powerwash hardware vendor-reset 0000:01:00.0
```

### System info

```bash
powerwash info
```

### Interactive menu

```bash
sudo powerwash menu
```

---

## Configuration

<!-- Document configuration options here. This section is yours — the AI will not modify it. -->

## CI

<!-- AI:start:ci -->
The repository uses GitHub Actions for continuous integration. The following workflows are defined:

1. **`mirror-osp-to-ooc.yaml`**  
   - Syncs the repository to an external mirror.  
   - Trigger: Push events to the default branch.  
   - Required secrets: `MIRROR_REPO_URL`, `MIRROR_REPO_TOKEN`.

2. **`trigger-artifact-mirror.yml`**  
   - Triggers artifact mirroring to external storage.  
   - Trigger: Successful completion of the `mirror-osp-to-ooc.yaml` workflow.  
   - Required secrets: `ARTIFACT_STORAGE_URL`, `ARTIFACT_STORAGE_TOKEN`.  

Both workflows ensure repository synchronization and artifact availability.
<!-- AI:end:ci -->

## Mirror chain

<!-- AI:start:mirror-chain -->
This repo is maintained in [`Interested-Deving-1896/linux-powerwash`](https://github.com/Interested-Deving-1896/linux-powerwash) and mirrored through:

```
Interested-Deving-1896/linux-powerwash  ──►  OpenOS-Project-OSP/linux-powerwash  ──►  OpenOS-Project-Ecosystem-OOC/linux-powerwash
```

Changes flow downstream automatically via the hourly mirror chain in
[`fork-sync-all`](https://github.com/Interested-Deving-1896/fork-sync-all).
Direct commits to OSP or OOC are detected and opened as PRs back to `Interested-Deving-1896`.
<!-- AI:end:mirror-chain -->

## Contributors

<!-- AI:start:contributors -->
[@Interested-Deving-1896](https://github.com/Interested-Deving-1896): 8 commits

Note: This repository is a mirror. Please refer to the upstream source for additional details.
<!-- AI:end:contributors -->

## Origins

<!-- AI:start:origins -->

Original project — distro-agnostic, filesystem-agnostic factory reset tool for Linux.
<!-- AI:end:origins -->

## Resources

<!-- AI:start:resources -->
| File | Description |
|---|---|
| [dep-graph/origins.md](https://github.com/Interested-Deving-1896/linux-powerwash/blob/main/dep-graph/origins.md) | Dependency graph (Markdown table) |
<!-- AI:end:resources -->

## License

<!-- AI:start:license -->
<!-- License not detected — add a LICENSE file to this repo. -->
<!-- AI:end:license -->
