**How To Guide: GitHub Organization Rulesets and Role Management**

**1\. Objective**

Based on ticket [TP4A-51](https://factory-x.atlassian.net/browse/TP4A-51), there is uncertainty in several repositories about how Branch Protection Rules, team roles, and personal roles interact.  
The goal is to introduce a unified, centralized ruleset at the organization level to ensure that developers can only perform permitted actions.

**2\. Branch Protection Rules vs. Rulesets**

- **Branch Protection Rules**
  - Configured per repository
  - Apply only to specific branches
- **Rulesets**
  - Can be created at the organization level (central management)
  - Can be applied to all repositories or a subset of them
  - Apply to branches, tags, or default branches
  - Enable centralized management and standardization
  - Multiple rulesets (up to 75) can be active at the same time (the strictest rule applies)

Rulesets are the newer “fancier” way of handling protection rules and are the recommend way of doing it. See [here](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets#about-rulesets-and-protected-branches).

**3\. Role Overview (Copied from** [**Github Docs**](https://docs.github.com/en/organizations/managing-user-access-to-your-organizations-repositories/managing-repository-roles/repository-roles-for-an-organization)**)**

From least access to most access, the roles for an organization repository are:

- **Read:** Recommended for non-code contributors who want to view or discuss your project
- **Triage:** Recommended for contributors who need to proactively manage issues, discussions, and pull requests without write access
- **Write:** Recommended for contributors who actively push to your project
- **Maintain:** Recommended for project managers who need to manage the repository without access to sensitive or destructive actions
- **Admin:** Recommended for people who need full access to the project, including sensitive and destructive actions like managing security or deleting a repository

Thus, in our case, we’d need a team for regular contributors, which can open PRs from downstream forked repositories, and a maintainer team, which is able to review and merge those PRs.

**4\. Mapping: Acceptance Criteria 🡪 Ruleset**

| Acceptance Criterion | Ruleset |
| --- | --- |
| No new branches in the repo (except dependabot, private repos) | Restrict creation + Bypass list |
| No direct push to main | Restrict updates |
| All changes must go through PRs | Require a pull request before merging |
| Only repo team members can review PRs | CODEOWNERS-Datei pro Repo |
| At least one approval, “change requested” blocks merge | Required approval = 1  <br>“Dismiss stale approvals” + “Require approval of most recent push” |
| CI/CD must succeed | Require status checks to pass |
| Block force pushed | Block force pushes |
| Only maintainer can add members | Organization setting (not configurable in ruleset) |

**5\. Concrete Ruleset Configuration**

- **Restrict creation** (branch creation only with bypass)
- **Restrict updates** (no push to main)
- **Restrict** **deletion** (prevent deletion of protected branches)
- **Require a pull request before merging**
  - Required approvals ≥ 1
  - Dismiss stale pull request approvals when new commits are pushed
  - Require approval of the most recent reviewable push
- **Require status checks to pass**
- **Block force pushes**
- **Bypass list**
  - Dependabot
  - (Maintainer)

**8\. References**

<https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets>

<https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/managing-a-branch-protection-rule>

<https://github.blog/changelog/2025-06-16-organization-rulesets-now-available-for-github-team-plans/>

<https://stackoverflow.com/questions/79292201/what-are-the-practical-differences-between-github-rulesets-and-branch-protection>
