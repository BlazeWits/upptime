# Handoff

Uptime monitor and status page for **gerardwu.com**, built from the
[Upptime](https://github.com/upptime/upptime) template.

Created under `BlazeWits`, transferred to `AIkry`.

## Current state

- Monitors one endpoint: `https://gerardwu.com`, every 5 minutes.
- Status page: https://blazewits.github.io/upptime/ (changes to
  `https://aikry.github.io/upptime/` after transfer).
- Only `.upptimerc.yml` was customized. Everything in `.github/workflows/`
  is generated — do not edit it by hand, it is overwritten by the daily
  template update.

## Required steps for the new owner

Secrets do not survive a repository transfer, and a fine-grained token is
bound to its resource owner, so the previous owner's token cannot work here.
A new token must be created on the new owner's account.

1. Accept the transfer. The repo must exist as `AIkry/upptime` before step 2,
   otherwise it cannot be selected when scoping the token.
2. Create a fine-grained PAT at
   <https://github.com/settings/personal-access-tokens/new>:
   - Resource owner: `AIkry`
   - Repository access: Only select repositories -> `AIkry/upptime`
   - **Repository** permissions (not Account), all set to **Read and write**:
     Contents, Actions, Issues, Workflows
3. Add it under Settings -> Secrets and variables -> Actions as a repository
   secret named `GH_PAT`.
4. Update `.upptimerc.yml`: set `owner: AIkry` and `assignees: [AIkry]`.
5. Settings -> Pages: confirm the source is branch `gh-pages`, folder `/`.
6. Actions -> Setup CI -> Run workflow.

Until step 3 is done the workflows fail with
`remote: Permission to ... denied` (HTTP 403). That is the missing token, not
a broken configuration.

## Maintenance

- Template and dependency updates are automatic: `Update Template CI` runs
  daily at 00:00 UTC, `Updates CI` at 03:00 UTC. Both need a valid `GH_PAT`.
- When the PAT expires, monitoring and auto-updates stop until it is
  regenerated and the `GH_PAT` secret is replaced.
- To add another endpoint, append to `sites:` in `.upptimerc.yml`:

  ```yaml
  sites:
    - name: gerardwu.com
      url: https://gerardwu.com
    - name: Blog
      url: https://blog.gerardwu.com
  ```

- Downtime opens a GitHub issue assigned to whoever is in `assignees:`, and
  closes it automatically on recovery. Keep `assignees` pointing at a real
  account with access or outages go unnoticed.
- Collaborators on a user-owned repo cap at `write`; `admin` and `maintain`
  are only grantable on organization-owned repos.

## Notes

- Repo is public so GitHub Actions minutes are free. Making it private means
  paying for roughly 3,000 minutes/month, and the status page would need an
  authenticated API proxy.
- `.upptimerc.yml` is the only file to change for configuration. See
  <https://upptime.js.org/docs/configuration>.
