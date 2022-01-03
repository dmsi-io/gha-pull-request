# gha-pull-request

This GitHub Action facilitates the adding of comments to both pull requests and Jira tickets with links to deployed applications.

For GitHub PRs, the current comment is as follows:

> Jira Tickets: [[JIRA-123](https://atlassian.net/browse/JIRA-123)]
> 
> Feature Branch: https://namespace.example.com

The issue tag(s) will be populated from the branch name that the PR is created from.

The feature branch URL will be populated via an input but is recommended to use this GHA in conjunction with `gha-k8s-deploy` to get the URL as an output.

For Jira tickets, the current comment is as follows:

> Feature branch has been deployed at: https://namespace.example.com

The URL will be identical to the one populating the GitHub comment.

### Usage

```yaml
- name: Pull Request Comments
  uses: dmsi-io/gha-pull-request@v1
  with:
    GHA_ACCESS_TOKEN: ${{ secrets.GHA_ACCESS_TOKEN }}
    JIRA_BASE_URL: ${{ secrets.JIRA_BASE_URL }}
    JIRA_USER_EMAIL: ${{ secrets.JIRA_USER_EMAIL }}
    JIRA_API_TOKEN: ${{ secrets.JIRA_API_TOKEN }}
```

> Note that if the Jira credentials are not provided, this action will skip attempting to comment on Jira tickets.

### Optional Inputs

#### URL

Used to specify a URL to use in feature branch PR comment. Used when the acting repo does branch-based deployment.

```yaml
  with:
    url: ${{ steps.deploy.outputs.url }}
```

#### Endpoint

Used to specify an endpoint to append to the end of the base URL. Mainly used if the acting repo is deployed under a specific endpoint prefix.

```yaml
  with:
    endpoint: '/application_url'
```

#### Branch Name
 
By default, this action will pull in the branch name based on whether the triggering webhook is a `push` or `pull_request`. However, this optional input allows the user to override this default and provide any branch name to use. 

```yaml
  with:
    branch_name: 'feature/JIRA-123'
```