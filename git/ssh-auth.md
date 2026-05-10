# Git SSH Authentication

## Start agent

```bash
eval "$(ssh-agent -s)"
```

## Add key

```bash
ssh-add ~/.ssh/id_ed25519
```

## Test

```bash
ssh -T git@github.com
```
