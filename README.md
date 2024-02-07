# 🪄 GitHub Action for Dynamic Templates

This action uses [mustache](https://mustache.github.io/) to enhance the power
of [GitHub Templates](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository).
It allows the templates to include dynamic values that will be populated with
the information related to the PR (Pull Request) on pre defined template tags.
The most basic usage is for adding the branch name as part of a link to the
ticketing system.

For example, in a scenario where the developer is supposed to use branch names
that matches the ticket number from JIRA and the link should be present on the
PR body, the repository PR template would have something similar to the
following:

```markdown
Related to https://your-jira-url.com/browse/{{head.ref}}

This PR changes:
  - 
  - 
  - 
```

Where, `head.ref` is the path on the [PR payload](https://docs.github.com/en/rest/reference/pulls#get-a-pull-request)
to the current branch that will be merged into the base branch. For instance,
if the base branch for the current PR is called `BUG-123` and the user adds
information to the body while creating the PR, it will save the body as:

```markdown
Related to https://your-jira-url.com/browse/{{head.ref}}

This PR changes:
  - Adds a 500 line snippet from stackoverflow that I don't know how it works,
    but it works
```

After action execution, the body will be:

```markdown
Related to https://your-jira-url.com/browse/BUG-123

This PR changes:
  - Adds a 500 line snippet from stackoverflow that I don't know how it works,
    but it works
```

## Inputs

### `token`

The `GITHUB_TOKEN` secret to access the repository. Defaults to github-actions internal `${{ github.token }}` Token.

### `customInput`

The `customInput` sets additional data to access in your templates. For example data from previous workflow steps. Access it via `{{ custom.YOUR_PROPERTY_NAME }}`


## Example usage

### Basic

```yaml
      - uses: luigibertaco/dynamic-template-action@1.0.0
```
#### Can render:
```handlebars
    Your PR was created: {{ head.ref }}
```

### With customInput

```yaml
      - name: Get current date
        id: date
        run: echo "::set-output name=date::$(date +'%Y-%m-%dT%H:%M:%S')"

      - uses: silvercory/dynamic-template-action@1.0.0
        with:
          customInput: |
            today: ${{ steps.date.outputs.date }}
            now: ${{ steps.date.outputs.date }}
```
#### Can render:
```handlebars
    Your PR was created: {{ custom.today }}
```
