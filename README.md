# Test fork sync

Forked repo with sync: https://github.com/roborregosteam/test-sync

[Workflow templates used](https://github.com/RoBorregos/.github/tree/main/workflow-templates) 

## Steps to reproduce

1. Create workflows (either by using a workflow template, or by copy-pasting the ones in [this folder](https://github.com/RoBorregos/test-sync/tree/main/.github/workflows))
2. Add org secrets with the required permissions
- E.g. `secrets.RBRGS_TEAM_GITHUB_TOKEN`: token used to create repo, sync forks, etc on the account that owns the forks.
3. Modify `PERSONAL_ACCOUNT` env variable: this corresponds to the account that will house the forks(`roborregosteam` in this example). Note: could also be stored as a secret to hide the account. In such case, the secret must also be added in the account that owns the forks (that is, so that the repo's forked workflow can cancel itself via  `if: ${{ github.repository_owner != secrets.PERSONAL_ACCOUNT }}`).
4. Builds are used to trigger forks on success. Builds may need env variables. You can add mock env variables in the same job that performs the build (e.g `echo "DISCORD_CLIENT_ID=12345678" >> .env`).

## Considerations

1. Beware of loops caused by workflows
2. Some of the repositories must be public for usage without github enterprise.
3. Avoid modifying the fork (fork sync could fail)
4. Jobs use Github API key, which has [some rates](https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api?apiVersion=2022-11-28#primary-rate-limit-for-authenticated-users) (5,000 req/hr for personal tokens) for API usage. Must be considered when scaling.

## Possible improvements

1. Parse .env.example to create temporal .env (very specific to the framework used by this example project)
2. Automatically make default branch protected on forked repo - (would require testing to determine if it affects fork sync)
3. Add a job to completely delete fork and recreate - ATM seems to be a waste of computational resources (?
