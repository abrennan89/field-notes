---
name: create-release-notes
description: Create a placeholder release notes file for OpenShift Virtualization
triggers: 
  - new release
  - release notes
  - create release note
  - release notes placeholder
---

# Create Release Notes Placeholder

This skill creates a placeholder release notes file for OpenShift Virtualization releases.

## Process Overview

This skill automates the creation of a placeholder release notes file by:

1. Locating the openshift-docs repository on the local machine
2. Reading current version numbers from `_attributes/common-attributes.adoc`
3. Incrementing the version numbers for the new release
4. Creating a PR against the `upstream/enterprise-X.XX` branch with:
   - Updated version attributes
   - New placeholder release notes file

## Step-by-Step Process

**Step 1: Locate openshift-docs repository**
- Search for the repository in common locations or prompt user for path

**Step 2: Read and increment version numbers**
- Read from `_attributes/common-attributes.adoc`:
  - `:VirtVersion:` (e.g., 4.21 → 4.22)
  - `:HCOVersion:` (e.g., 4.21.0 → 4.22.0)
  - `:HCOVersionPrev:` (e.g., 4.20.0 → 4.21.0)

**Step 3: Create feature branch**
- Checkout `upstream/enterprise-X.XX` (using new version number)
- Create branch: `virt-X-XX-release-notes-placeholder`

**Step 4: Update attributes in enterprise branch**
- Update `_attributes/common-attributes.adoc` with new version numbers

**Step 5: Create placeholder file**
- Create file: `virt/release_notes/virt-X-XX-release-notes.adoc`
- Add template content:
```
:_mod-docs-content-type: ASSEMBLY
[id="virt-X-XX-release-notes"]
= {VirtProductName} release notes
include::_attributes/common-attributes.adoc[]
:context: virt-X-XX-release-notes

toc::[]

Do not add or edit release notes here. Edit release notes directly in the branch
that they are relevant for.

This file is here to allow builds to work.
```

**Step 6: Commit and create PR**
- Commit changes
- Push to fork
- Create PR against `upstream/enterprise-X.XX`

## Notes

- This skill creates the placeholder PR (PR #2 from the release notes process)
- A separate PR to update attributes on main branch (PR #1) can be created manually or added later
- The enterprise branch PR can be rebased later as needed

## Usage

Invoke this skill by running:
```
/create-release-notes
```

Or by asking Claude to "create a release notes placeholder"

## Reference

Based on the process documented in `/docs/cnv/release-notes/rn-process.md`
