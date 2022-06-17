# common snippets for github cli

## to create a new repo on vsharma-next

#### 2022-06-17

- create the repo on github and create a local clone

```
gh repo create sandbox_rt_jacco --public --template template-repo --clone
```

- add the secret to automatically add docs to jupyter-book

```
gh secret set API_TOKEN_GITHUB < GA_PAT.txt
```
