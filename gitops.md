# TIL: GitOps in 2 Minutes

**The core idea:** Git is the single source of truth for your running infrastructure. Instead of engineers running manual deployment commands or giving CI pipelines direct cluster credentials, you declare the desired state in a Git repo, and an agent inside the cluster pulls and applies it automatically.

### How It Works

1. **You describe state, not steps:** You write manifests (e.g., "Run 3 replicas of `v1.2`"), not script commands.
2. **Deploying is just a Pull Request:** Merging to `main` triggers a release; rolling back is a simple `git revert`.
3. **Pushed vs. Pulled:** CI builds the app and updates configuration files in Git. An in-cluster agent detects changes, pulls them, and corrects configuration drift automatically.

---

### The GitOps Toolchain

GitOps spans everything from where code lives to how it gets verified, packaged, and synced:

* **SaaS Git Repos & Collaboration:** GitHub, GitLab, and Bitbucket (where branches, reviews, and pull requests live).
* **Branching Models & Workflows:** Patterns like trunk-based development or Vincent Driessen's classic [A successful Git branching model (nvie.com)](https://nvie.com/posts/a-successful-git-branching-model/) (Git Flow), defining how code transitions from feature branches to release-ready state.
* **Packaging & Templating:** Helm, Kustomize (templates manifests so environments only differ by small values files).
* **GitOps Controllers (Sync Engines):** ArgoCD, Flux (run inside Kubernetes, watch Git, and reconcile live state).
* **Secrets Management:** Sealed Secrets, External Secrets Operator, HashiCorp Vault (keeps raw credentials out of plain text Git commits).

---

### Why It Matters

* **Zero cluster credentials in CI:** CI only needs push access to Git, not `cluster-admin` rights.
* **Instant audit trail:** `git log` shows exactly who deployed what, when, and why.
* **No configuration drift:** If someone edits a pod or service manually, the in-cluster controller detects the mismatch and reverts it.
