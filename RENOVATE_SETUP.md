# Renovate Bot Setup - Next Steps

## ✅ Completed Implementation

The following files have been created/updated to enable automated dependency management with Renovate bot:

1. **`.github/workflows/test.yml`** (NEW)
   - Build validation workflow
   - Content validation checks
   - Runs on all PRs and pushes to main

2. **`renovate.json`** (UPDATED)
   - Auto-merge rules for patch/minor updates
   - Manual review requirements for major updates
   - Scheduling and rate limiting
   - Grouped updates for related dependencies

3. **`.github/workflows/.github-pages.yml`** (UPDATED)
   - Added comments clarifying test workflow integration
   - Existing deployment logic unchanged

4. **`README.md`** (UPDATED)
   - Added documentation about automated dependency management
   - Explained auto-merge vs manual review rules

5. **`CLAUDE.md`** (UPDATED)
   - Added Renovate and test workflow documentation
   - Updated deployment section

## 🔧 Manual Steps Required

### Step 1: Enable Renovate Bot (if not already enabled)

1. Go to https://github.com/apps/renovate
2. Click "Configure"
3. Select your repository: `marcopaga/marcopaga.github.io`
4. Grant access

### Step 2: Configure Branch Protection Rules

**CRITICAL:** Configure branch protection on the `main` branch to enable auto-merge:

1. Go to repository Settings → Branches → Branch protection rules
2. Add rule for `main` branch:
   - ✅ **Require status checks to pass before merging**
     - Search and add: `build-test`
     - Search and add: `content-validation`
   - ✅ **Require branches to be up to date before merging**
   - ✅ **Allow auto-merge**
   - ⚠️ Optional but recommended:
     - Add `run-sitespeed` as optional check (won't block merge if it fails)

### Step 3: Enable Auto-Merge in Repository Settings

1. Go to Settings → General → Pull Requests
2. ✅ Enable "Allow auto-merge"

### Step 4: Test the Setup

#### Option A: Wait for next Renovate run (Monday before 6am CET)
Renovate will automatically create PRs for pending updates.

#### Option B: Trigger Renovate manually
1. Create a dummy commit to trigger Renovate:
   ```bash
   git commit --allow-empty -m "trigger: renovate"
   git push origin main
   ```
2. Check the Renovate bot dashboard for PR creation

#### Option C: Test with a local PR
1. Create a test branch and modify a GitHub Action version
2. Open a PR and verify:
   - `build-test` job runs and passes
   - `content-validation` job runs and passes
   - Status checks appear in the PR

### Step 5: Manage Existing Renovate PRs

You currently have 6 pending Renovate PRs. After completing Steps 1-3:

1. **Option A: Close and recreate** (Recommended)
   - Close all existing Renovate PRs
   - Renovate will recreate them with new configuration
   - New PRs will have auto-merge enabled

2. **Option B: Rebase existing PRs**
   - Comment `@renovate rebase` on each PR
   - Renovate will update them with new config

## 📊 Expected Behavior

### For Patch/Minor Updates (Auto-merge enabled)
1. Renovate creates PR
2. Test workflow runs automatically
3. If tests pass:
   - PR auto-merges (no manual action needed)
   - Deployment to GitHub Pages happens
4. If tests fail:
   - PR stays open for manual review
   - You'll be notified

### For Major Updates (Manual review required)
1. Renovate creates PR with label `major-update`
2. Test workflow runs automatically
3. PR waits for manual review (no auto-merge)
4. You review changes and merge manually

### For Hugo Updates (Manual review required)
1. Renovate creates PR with label `hugo-update`
2. Test workflow runs automatically
3. PR waits for manual review
4. Test locally before merging (breaking changes common)

## 🔍 Verification Checklist

After completing manual steps, verify:

- [ ] Branch protection rules are active on `main`
- [ ] `build-test` and `content-validation` are required checks
- [ ] Auto-merge is enabled in repository settings
- [ ] Renovate bot has access to the repository
- [ ] Test workflow runs successfully on a test PR
- [ ] Existing Renovate PRs are closed or rebased

## 🛠️ Troubleshooting

### Auto-merge not working
- Verify branch protection rules include required status checks
- Check "Allow auto-merge" is enabled in repository settings
- Ensure Renovate has write permissions

### Tests failing
- Check test workflow logs in Actions tab
- Verify Hugo builds locally: `hugo --minify --baseURL="https://marco-paga.eu"`
- Ensure critical pages exist after build

### Renovate not creating PRs
- Check Renovate dashboard for errors
- Verify `renovate.json` syntax is valid
- Check schedule configuration (runs Mondays before 6am CET)

## 📚 Additional Resources

- [Renovate Documentation](https://docs.renovatebot.com/)
- [GitHub Branch Protection](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches)
- [GitHub Auto-merge](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/automatically-merging-a-pull-request)

## 🎯 Success Criteria

You'll know the setup is working correctly when:
1. Renovate creates a patch/minor update PR
2. Test workflow runs and passes
3. PR auto-merges without manual intervention
4. Site deploys successfully to https://marco-paga.eu
5. Live site remains functional
