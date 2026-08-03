# Learning progress dashboard

The repository uses a deliberately simple split:

| Component | Responsibility |
|---|---|
| GitHub repository | Curriculum, code, lab documentation, runbooks, and evidence |
| GitHub Issues | Live task checkboxes and phase completion |
| GitHub Pages | Read-only HTML dashboard built from the issue state |
| IDE or local shell | Terraform, Ansible, Kubernetes, application, and documentation work |
| ChatGPT GitHub connection | Repository inspection and authorized file, branch, pull-request, and issue updates |

## Why the dashboard is read-only

A static GitHub Pages site cannot safely write back to GitHub without an OAuth flow,
backend service, or personal access token. Embedding a token in browser JavaScript would
expose repository write access and is not acceptable.

The dashboard therefore performs a single unauthenticated read against the public GitHub
Issues API. Progress remains cross-device because the checkboxes live in GitHub rather
than browser local storage.

## Daily workflow

1. Open the next roadmap issue.
2. Work on one or more unchecked tasks.
3. Commit the lab code or documentation.
4. Add evidence to the required evidence document.
5. Check only tasks that are implemented and evidenced.
6. Refresh the dashboard.

Reading documentation or watching training alone does not mark an implementation task complete.

## Publishing with GitHub Pages

After the dashboard pull request is merged:

1. Open the repository **Settings**.
2. Select **Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**.
4. Select branch **main** and folder **/docs**.
5. Save the configuration.

The expected site address is:

`https://en4ble1337.github.io/terra-kube-ansible/`

The repository must remain public for the token-free issue API design. If the repository
is made private, replace the unauthenticated browser request with an authenticated backend
or abandon the live dashboard in favor of local-only progress.

## Completion standard

A phase is complete only when:

- the implementation works;
- the phase completion gate passes;
- code and configuration are committed;
- the runbook explains deployment, validation, troubleshooting, and teardown;
- evidence is linked or recorded;
- all issue tasks, including deliverables, are checked.

## Time model

The roadmap is estimated at approximately 214 hours. At a sustained 18–24 hours per week,
the work fits roughly into a 9–12 week window. The schedule is milestone-driven: do not
advance merely because a calendar week ended.
