# quarantine

Private staging area for Falcon Hub uploads. Nothing in this repository is ever published or merged to the main site. Every upload gets its own branch, `upload/<id>`, and the GitHub check bot checks it there.

## What happens here

1. The Falcon Hub worker creates a branch named `upload/<id>` and commits the uploaded files under `uploads/<id>/`.
2. The workflow in `.github/workflows/quarantine.yml` runs on that branch.
3. The **check** job scans the files for unsafe patterns, checks the format for each type (skill, plugin, connector), and compiles code where that applies. It never receives write access, and it uses no secrets.
4. If the check passes, the **promote** job reports the result back to Falcon Hub. It is the only job with a write token, and it uses that token only to record the outcome.
5. If the check fails, the branch stays here and is marked failed. A person can read the report in the workflow run.

Branches are deleted after the result has been recorded.

## Layout

- `.github/workflows/quarantine.yml`: the check and promote workflow
- `uploads/<id>/`: the files for one upload, present only on its `upload/<id>` branch

Nothing else belongs in this repository. Do not add code, documentation or releases to the default branch.

## Setup

These secrets and variables are set under **Settings → Secrets and variables → Actions**.

Secrets:

- `APP_ID`: the ID of the `falcon-hub-bot` GitHub App
- `APP_PRIVATE_KEY`: the App's private key, in PKCS#8 format
- `FALCON_UPLOAD_SECRET`: the same value that the Falcon Hub worker uses to sign callbacks

Variables:

- `FALCON_CALLBACK_URL`: `https://falcon-hub-mcp.pages.dev/api/upload/callback`
- `FALCON_MAIN_REPO`: `quantum-suite-labs/falcon-cloud`

The GitHub App `falcon-hub-bot` needs Contents (read and write) and Pull requests (read and write) on this repository and on `quantum-suite-labs/falcon-cloud`. It must not be installed anywhere else.

## Security

- The check job runs with read-only permissions and no secrets, so uploaded code never runs with access to them.
- Uploaded code is never executed by the check job. It is scanned and compiled only.
- Only the promote job receives the App token, and it only calls the callback URL.
- Please report security problems privately to the repository owner, not as public issues.
