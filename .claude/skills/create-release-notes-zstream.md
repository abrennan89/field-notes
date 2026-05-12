---
name: create-release-notes-zstream
description: Create a placeholder release notes file for OpenShift Virtualization z-stream releases
triggers: 
  - z-stream release
  - zstream release notes
  - create zstream release note
  - z-stream placeholder
---

# Create Z-Stream Release Notes Placeholder

This skill creates a placeholder release notes file for OpenShift Virtualization z-stream releases (e.g., 4.22.1, 4.22.2).

## Process Overview

This skill automates the creation of a z-stream placeholder release notes file by:

1. Locating the openshift-docs repository on the local machine
2. Reading current version numbers from `_attributes/common-attributes.adoc`
3. Incrementing the z-stream version number (e.g., 4.22 → 4.22.1)
4. Creating a PR against the `upstream/enterprise-X.XX` branch with:
   - Updated version attributes for z-stream
   - New placeholder release notes file

## Step-by-Step Process

**Step 1: Locate openshift-docs repository**
- Search for the repository in common locations or prompt user for path

**Step 2: Read and increment version numbers for z-stream**
- Read from `_attributes/common-attributes.adoc`:
  - `:VirtVersion:` (e.g., 4.22 → 4.22.1 or 4.22.1 → 4.22.2)
  - `:HCOVersion:` (e.g., 4.22.0 → 4.22.1 or 4.22.1 → 4.22.2)
  - `:HCOVersionPrev:` (e.g., 4.21.0 → 4.22.0 or 4.22.0 → 4.22.1)

**Step 3: Create feature branch**
- Checkout `upstream/enterprise-X.XX` (using major.minor version number)
- Create branch: `virt-X-XX-Z-release-notes-placeholder` (e.g., `virt-4-22-1-release-notes-placeholder`)

**Step 4: Update attributes in enterprise branch**
- Update `_attributes/common-attributes.adoc` with new z-stream version numbers

**Step 5: Create placeholder file**
- Create file: `virt/release_notes/virt-X-XX-Z-release-notes.adoc` (e.g., `virt-4-22-1-release-notes.adoc`)
- Add template content:
```
:_mod-docs-content-type: ASSEMBLY
include::_attributes/common-attributes.adoc[]
[id="virt-X-XX-Z-release-notes"]
= {VirtProductName} release notes
:context: virt-X-XX-Z-release-notes

[role="_abstract"]
These release notes describe new features and enhancements, Technology Preview features, deprecated and removed features, fixed issues, and known issues for {VirtProductName} {VirtVersion}.

toc::[]

Do not add or edit release notes here. Edit release notes directly in the branch
that they are relevant for.

This file is here to allow builds to work.
```

**Step 6: Commit and create PR**
- Commit changes
- Push to fork
- Create PR against `upstream/enterprise-X.XX`

## Differences from Major/Minor Release Placeholder

- Version increments are z-stream (patch level): 4.22 → 4.22.1, not 4.21 → 4.22
- HCOVersion loses the trailing .0: 4.22.0 → 4.22.1 (not 4.22.1)
- File names use dashes for all version components: `virt-4-22-1-release-notes.adoc`
- Branch follows same pattern: `virt-4-22-1-release-notes-placeholder`

## Notes

- This skill creates the z-stream placeholder PR
- Z-stream releases are patch releases within an existing major.minor version
- The enterprise branch number stays the same (e.g., enterprise-4.22 for both 4.22.0 and 4.22.1)

## Usage

Invoke this skill by running:
```
/create-release-notes-zstream
```

Or by asking Claude to "create a z-stream release notes placeholder"

## Reference

Based on the process documented in `/docs/cnv/release-notes/rn-process.md`
