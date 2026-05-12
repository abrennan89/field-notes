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

This skill creates TWO pull requests for z-stream releases:

**PR#1 (main branch)**:
- Dummy release notes file for builds
- Updated version attributes

**PR#2 (enterprise branch)**:
- Placeholder release notes file (copies dummy from PR#1)
- No attribute changes

**Automatic Trigger**: PR#2 is automatically created once PR#1 is merged (manual merge required)

## Step-by-Step Process

### Part 1: Create PR#1 (main branch)

**Step 1: Locate openshift-docs repository**
- Search for the repository in common locations or prompt user for path

**Step 2: Read and increment version numbers for z-stream**
- Read from `_attributes/common-attributes.adoc` on main:
  - `:VirtVersion:` (e.g., 4.22 → 4.22.1 or 4.22.1 → 4.22.2)
  - `:HCOVersion:` (e.g., 4.22.0 → 4.22.1 or 4.22.1 → 4.22.2)
  - `:HCOVersionPrev:` (e.g., 4.21.0 → 4.22.0 or 4.22.0 → 4.22.1)

**Step 3: Create PR#1 branch off main**
- Checkout `upstream/main`
- Create branch: `virt-X-XX-Z-attributes-update` (e.g., `virt-4-22-1-attributes-update`)

**Step 4: Create dummy file and update attributes**
- Create `virt/release_notes/virt-X-XX-Z-release-notes.adoc` with dummy content:
```
:_mod-docs-content-type: ASSEMBLY
[id="virt-X-XX-Z-release-notes"]
= {VirtProductName} release notes
include::_attributes/common-attributes.adoc[]
:context: virt-X-XX-Z-release-notes

toc::[]

Do not add or edit release notes here. Edit release notes directly in the branch
that they are relevant for.

This file is here to allow builds to work.
```
- Update `_attributes/common-attributes.adoc` with new version numbers

**Step 5: Commit and create PR#1**
- Commit both files
- Push to fork
- Create PR against `upstream/main`
- PR description notes that PR#2 will follow after merge

### Part 2: Automatic PR#2 Creation (triggered on PR#1 merge)

**Trigger Setup**: A GitHub Actions workflow monitors PR#1 merge

**Step 6: Auto-create PR#2 branch off enterprise branch**
- On PR#1 merge event, checkout `upstream/enterprise-X.XX`
- Create branch: `virt-X-XX-Z-release-notes-placeholder` (e.g., `virt-4-22-1-release-notes-placeholder`)

**Step 7: Copy dummy file from merged PR#1**
- Copy `virt/release_notes/virt-X-XX-Z-release-notes.adoc` from main
- Do NOT copy attribute changes

**Step 8: Commit and auto-create PR#2**
- Commit the release notes file only
- Push to fork
- Auto-create PR against `upstream/enterprise-X.XX`
- PR references PR#1 in description

## Automatic Trigger Configuration

The skill sets up a trigger to automatically create PR#2 once PR#1 is merged. This can be implemented via:

### Option 1: GitHub Actions Workflow (Recommended)
Create a workflow in the openshift-docs repo that:
- Triggers on `pull_request` events with `closed` action
- Checks if merged PR matches pattern `virt-*-attributes-update`
- Extracts version from PR branch name
- Creates PR#2 to enterprise branch with copied release notes file

See `examples/virt-release-notes-trigger.yml` for workflow template.

### Option 2: Claude Code Hook
Configure a hook in `.claude/settings.json`:
```json
{
  "hooks": {
    "on-pr-merge": {
      "pattern": "virt-*-attributes-update",
      "action": "create-release-notes-pr2"
    }
  }
}
```

**Important**: PR merges must always be done manually by a human writer. The trigger only automates PR#2 creation, not the merge itself.

## Differences from Major/Minor Release Placeholder

- Creates TWO PRs instead of one
- PR#1 (main): dummy file + attributes
- PR#2 (enterprise): placeholder file only (no attributes)
- Version increments are z-stream (patch level): 4.22 → 4.22.1, not 4.21 → 4.22
- HCOVersion format: 4.22.0 → 4.22.1
- File names use dashes for all version components: `virt-4-22-1-release-notes.adoc`
- Branch naming:
  - PR#1: `virt-4-22-1-attributes-update`
  - PR#2: `virt-4-22-1-release-notes-placeholder`

## Notes

- This skill creates TWO PRs: one for main (with attributes), one for enterprise (placeholder only)
- Z-stream releases are patch releases within an existing major.minor version
- The enterprise branch number stays the same (e.g., enterprise-4.22 for both 4.22.0 and 4.22.1)
- PR#2 is auto-created after PR#1 is manually merged by a writer
- **Never automate the PR merge** - always requires human review and manual merge

## Usage

Invoke this skill by running:
```
/create-release-notes-zstream
```

Or by asking Claude to "create a z-stream release notes placeholder"

## Reference

Based on the process documented in `/docs/cnv/release-notes/rn-process.md`
