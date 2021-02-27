# Trunk Based development in GIT

- Whole Development Team share a common branch called TRUNK (or main/master)
- Each committer (developer) in this trunk based development way of working generally steams a small commits --directly--> Into the trunk branch (main or master branch)
- Trunk based development is best for short lived feature branches (one person over a couple of days max) and flowing PR style code-review and build automation before merging into the trunk branch
- Trunk-Based Development is a key enabler of Continuous Integration and by extension Continuous Delivery.
- When individuals on a team are committing their changes to the trunk multiple times a day (all team members commit to trunk at least once every 24 hours) ==> This ensures the codebase is always releasable on demand and helps to make Continuous Delivery a reality.
- **Trunk Based Development**
  - ![Trunk Based](./assets/trunk.png)
- **Scaled Trunk-Based Development**:
  - ![Scaled Trunk Based](./assets/scaled-trunk.png)
- Claims
  - You can either do a direct to trunk commit/push (v small teams) or a Pull-Request workflow as long as those feature branches are short-lived and the product of a single person
  - You should do Trunk-Based Development instead of GitFlow
- Caveats or Warning
  - Depending on the team size, and the rate of commits, short-lived feature branches are used for code-review and build checking (CI), but not artifact creation or publication,
  - Very small teams may commit direct to the trunk.
  - Depending on the intended release cadence, there may be release branches that are cut from the trunk on a just-in-time basis, are ‘hardened’ before a release
  - there may also be no release branches if the team is releasing from Trunk, and choosing a “fix forward” strategy for bug fixes. (Releasing from trunk is also for high-throughput teams)
  - If you have more than a couple of developers on the project, you are going to need to hook up a build server to verify that their commits have not broken the build after they land in the trunk
  - Google do Trunk-Based Development and have 35000 developers and QA automators in that single monorepo trunk
  - Also practice it at scale trunck based development
  - People who practice the GitHub-flow branching model will feel that this is quite similar, but there is one small difference around where to release from.
    - Gitflow branching Model:
      - Gitflow Workflow is a Git workflow that helps with continuous software development and implementing DevOps practices.
      - Gitflow Workflow defines a strict branching model designed for project release
      - provides a robust framework for managing larger projects.
      - This workflow doesn’t add any new concepts or commands beyond what’s required for the Feature Branch Workflow.
      - Feature branch workflow: [Release branch](./release-branch.md)
  - Many publications promote Trunk-Based Development as we describe it here. Those include the best-selling ‘Continuous Delivery’ and ‘DevOps Handbook’.

---

- Reference : https://trunkbaseddevelopment.com/
