# SonarQube CLI/API quick guide

This guide covers using the SonarQube CLI, skill, and API to investigate build failures, quality-gate results, rule violations, and issue locations.

## SonarQube skill

Use the SonarQube skill when investigating SonarQube/SonarCloud build failures, quality-gate results, rule violations, or issue locations.

Default SonarQube host:

```text
https://sonarqube.us
```

Important rule: do not fix a SonarQube issue by guessing. First query the SonarQube API successfully and use the returned issue evidence: rule, severity, message, component, and line.

## SonarQube CLI (recommended)

The `sonar` CLI reads issues and quality-gate results straight from the server (the latest CI analysis). It does not need a local scan.

Install and log in once. The token is stored in the macOS Keychain, so there is no `SONAR_API` variable to manage:

```bash
# installer adds ~/.local/share/sonarqube-cli/bin to PATH in ~/.zshrc
sonar auth login -s https://sonarqube.us -o YOUR_ORG
sonar auth status        # [✓ Connected]  Server https://sonarqube.us  Org YOUR_ORG
```

Current issues on a pull request:

```bash
sonar list issues -p <project-key> --pull-request <n> --format table
# e.g.
sonar list issues -p YOUR_ORG_project-name --pull-request 23 --format table
```

Each row shows severity | rule | message | file:line. Other filters:

```bash
sonar list issues -p <project-key> --branch main --new-code              # new code on a branch
sonar list issues -p <project-key> --pull-request <n> --file src/x.ts    # one file
sonar list issues -p <project-key> --pull-request <n> --severities BLOCKER,HIGH   # MQR severities: INFO, LOW, MEDIUM, HIGH, BLOCKER
```

JSON is the default format. Informational lines go to stderr, so the output pipes cleanly into `jq`:

```bash
sonar list issues -p <project-key> --pull-request <n> 2>/dev/null \
  | jq -r '.issues[] | [.severity, .rule, (.component|sub(".*:";"")), (.line // ""), .message] | @tsv'
```

Quality gate (exits 0 on success; `--all` also lists passing conditions):

```bash
sonar qg status -p <project-key> --pull-request <n> --all
sonar qg status -p <project-key> --branch main --format json
```

Any other Web API endpoint, authenticated automatically:

```bash
sonar api get "/api/measures/component?component=<project-key>&pullRequest=<n>&metricKeys=new_violations"
sonar api get "/api/rules/show?key=typescript:S3776&organization=YOUR_ORG"   # rules need organization
```

Notes:

- The results reflect the last CI analysis. After you push fixes, wait for CI to rescan before checking again.
- A passing SonarQube gate does not guarantee CI passes. Some pipelines run separate quality checks through `qualitygate/*.properties` (for example `SONAR_NEW_VIOLATIONS=0`). Every issue listed for the PR, including MINOR ones, must be fixed.

## API with curl (alternative)

The sections below use the Web API directly with a `SONAR_API` token. Use them where the CLI is not installed.

### Authentication

The API token must be provided through the `SONAR_API` environment variable. Do not put the token directly in commands, files, logs, PR comments, or source code.

Set it for your current shell:

```bash
export SONAR_API='<your-token>'
```

Persist it in your shell profile only if your workstation secret-handling policy allows it:

```bash
echo 'export SONAR_API=<your-token>' >> ~/.zshrc
source ~/.zshrc
```

Safer option: store it in a password manager and export it only when needed.

Verify it is set without printing the token:

```bash
test -n "$SONAR_API" && echo "SONAR_API is set"
```

### Query SonarQube issues

For quality-gate failures, get the project key from the scanner log or workflow environment, then query open new-code issues for the analyzed branch:

```bash
curl --fail --silent --show-error -u "${SONAR_API}:" --get \
  'https://sonarqube.us/api/issues/search' \
  --data-urlencode 'componentKeys=<project-key>' \
  --data-urlencode 'branch=<branch>' \
  --data-urlencode 'sinceLeakPeriod=true' \
  --data-urlencode 'resolved=false' \
  --data-urlencode 'ps=100'
```

Pretty-print the result:

```bash
curl --fail --silent --show-error -u "${SONAR_API}:" --get \
  'https://sonarqube.us/api/issues/search' \
  --data-urlencode 'componentKeys=<project-key>' \
  --data-urlencode 'branch=<branch>' \
  --data-urlencode 'sinceLeakPeriod=true' \
  --data-urlencode 'resolved=false' \
  --data-urlencode 'ps=100' \
  | jq .
```

Show the most useful fields:

```bash
curl --fail --silent --show-error -u "${SONAR_API}:" --get \
  'https://sonarqube.us/api/issues/search' \
  --data-urlencode 'componentKeys=<project-key>' \
  --data-urlencode 'branch=<branch>' \
  --data-urlencode 'sinceLeakPeriod=true' \
  --data-urlencode 'resolved=false' \
  --data-urlencode 'ps=100' \
  | jq -r '.issues[] | [.severity, .rule, .component, (.line // ""), .message] | @tsv'
```

### Check Sonar quality gate

Get project status:

```bash
curl --fail --silent --show-error -u "${SONAR_API}:" --get \
  'https://sonarqube.us/api/qualitygates/project_status' \
  --data-urlencode 'projectKey=<project-key>' \
  --data-urlencode 'branch=<branch>' \
  | jq .
```

## Common failure workflow

Read the failed CI logs:

```bash
gh run view <run-id> --log-failed
```

Find the Sonar project key in the scanner output or `sonar-project.properties`.

List the issues for that project key and pull request:

```bash
sonar list issues -p <project-key> --pull-request <n> --format table
```

Fix only the issues returned by SonarQube, then commit and push:

```bash
git add <files>
git commit -m "PROJ-1234 fix SonarQube issues"
git push
gh run watch
```