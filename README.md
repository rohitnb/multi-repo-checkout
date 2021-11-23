# Using GitHub Actions to checkout multiple repos

## Using SSH Keys
1. Generate an SSH Key without a passphrase. Not using a passphrase is important to ensure CI/CD processes do not abort due to lack of passphrase input. [How to generate SSH Key?](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
2. Add the public key to your GitHub profile. Follow [this link](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) for a guide on adding an SSH Key to your account. For Enterprise customers, you might want to consider a service GitHub account with least privileges to link the SSH Key to.
3. Add the private key either as an Organization secret or Repo secret. For example, name the secret as `ORG_SSHKEY`. Follow [this link](https://docs.github.com/en/actions/security-guides/encrypted-secrets#creating-encrypted-secrets-for-a-repository) for a guide on adding a secret to the repository. If you want the secret to be accessible by all repository, create an Organization secret
4. In the GitHub Actions workflow (YAML file), be sure to use `actions/checkout` in the below manner:
```
- uses: actions/checkout@v2
  with:
    repository: <ORG-NAME>/<REPO-1>
    path: repo-1
    ssh-key: '${{secrets.ORG_SSHKEY}}'
- uses: actions/checkout@v2
  with:
    repository: <ORG-NAME>/<REPO-2>
    path: repo-2
    ssh-key: '${{secrets.ORG_SSHKEY}}'
```

### Call outs:
- A public SSH key may be associated with only one user or one repository (as deploy key). The private key maybe added as an Org secret but the public key will be linked with only one resource.


## Using Personal Access Tokens:
1. Generate a Personal Access Token and ensure that it has the `repo` scopes at minimum
2. Add the personal access token as an Organization or Repo secret. 
3. Use the `token` keyword while checking out the repos
```
- uses: actions/checkout@v2
  with:
    repository: <ORG-NAME>/<REPO-1>
    path: repo-1
    token: '${{secrets.ORG_TOKEN}}'
- uses: actions/checkout@v2
  with:
    repository: <ORG-NAME>/<REPO-2>
    path: repo-2
    token: '${{secrets.ORG_TOKEN}}'
```

### Call outs:
- Personal Access Tokens offer fine grained permissions and a better handling of permissions. 
