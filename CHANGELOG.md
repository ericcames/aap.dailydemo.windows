# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Fixed
- Repointed `vm_image` (`roles/vm/vars/main.yml`) to the current Windows Server 2025
  Base AMI for us-west-1: `ami-0a2d99cd134c33377`
  (`Windows_Server-2025-English-Full-Base-2026.05.13`). The previous AMI
  `ami-072fcf26b3b4a134a` was deregistered by AWS, causing `RunInstances` to fail
  with `AuthFailure — Not authorized for images` (ServiceNow INC0011346, AAP job 178).

### Added
- `CHANGELOG.md` to track notable changes.
- `docs/update-ami.md` runbook: how to refresh an existing AAP with a new AMI.
