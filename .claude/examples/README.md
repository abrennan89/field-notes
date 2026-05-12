# Claude Skills Examples

This directory contains example configurations and workflows for Claude skills.

## virt-release-notes-trigger.yml

GitHub Actions workflow for automatically creating PR#2 after PR#1 is merged in the OpenShift Virtualization release notes process.

### Setup Instructions

1. **Copy the workflow file** to the openshift-docs repository:
   ```bash
   cp virt-release-notes-trigger.yml /path/to/openshift-docs/.github/workflows/
   ```

2. **Ensure GitHub Actions is enabled** in the repository settings

3. **Grant workflow permissions**:
   - Go to Repository Settings → Actions → General
   - Under "Workflow permissions", select "Read and write permissions"
   - Enable "Allow GitHub Actions to create and approve pull requests"

4. **Test the workflow**:
   - Create a test PR with branch name matching `virt-X-XX-Z-attributes-update`
   - Merge the PR
   - Verify that PR#2 is automatically created against the enterprise branch

### How It Works

1. **Trigger**: Workflow runs when a PR is merged to `main`
2. **Filter**: Checks if the merged PR's branch name matches `virt-X-XX-Z-attributes-update`
3. **Extract version**: Parses version numbers from branch name (e.g., `virt-4-22-1-attributes-update` → 4.22.1)
4. **Create branch**: Checks out the appropriate enterprise branch (e.g., `enterprise-4.22`)
5. **Copy file**: Copies the release notes file from main (merged PR#1)
6. **Create PR**: Opens PR#2 to the enterprise branch with the placeholder file

### Important Notes

- **Manual merge required**: The workflow only creates the PR, it does NOT automatically merge it
- **Human review**: A writer must always review and manually merge both PR#1 and PR#2
- **No attribute changes in PR#2**: Only the release notes file is copied, attribute changes stay on main
- **Version detection**: Relies on specific branch naming convention

### Customization

You can modify the workflow to:
- Change the branch name pattern matching
- Customize commit messages
- Add additional validation steps
- Notify team members via Slack/email
- Add labels to created PRs

### Troubleshooting

**Workflow doesn't trigger:**
- Check that PR was merged (not just closed)
- Verify branch name matches pattern `virt-X-XX-Z-attributes-update`
- Check workflow logs in Actions tab

**PR creation fails:**
- Ensure workflow has proper permissions
- Verify the enterprise branch exists
- Check that the release notes file exists on main

**Wrong enterprise branch:**
- Workflow extracts branch from version (e.g., 4.22.1 → enterprise-4.22)
- If naming differs, update the regex pattern in the workflow
