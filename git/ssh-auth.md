# Git SSH Authentication

SSH authentication lets Git use your SSH key instead of repeatedly asking for your GitHub username and password.

---

## Start SSH Agent

```bash
eval "$(ssh-agent -s)"
```

### What it does

- `ssh-agent` starts a background process that securely stores your SSH keys in memory.
- `-s` outputs shell commands needed to configure your current terminal session.
- `$(...)` captures that output.
- `eval` executes the captured commands in the current shell.

Without `eval`, the agent may start, but your terminal would not know how to communicate with it.

### Example output

```bash
SSH_AUTH_SOCK=/tmp/ssh-abc123/agent.1234;
SSH_AGENT_PID=1235;
echo Agent pid 1235;
```

`eval` runs those lines automatically.

### Why it's needed

It allows commands like:

```bash
ssh-add ~/.ssh/id_ed25519
```

to connect to the running SSH agent.

---

## Add SSH Key

```bash
ssh-add ~/.ssh/id_ed25519
```

### What it does

Adds your private SSH key into the SSH agent memory.

After adding it:
- Git can authenticate with GitHub
- you avoid entering passwords repeatedly
- SSH connections become seamless

### Notes

- `~/.ssh/id_ed25519` is your private key
- never share private keys
- you may be asked for your key passphrase

---

## Test GitHub Authentication

```bash
ssh -T git@github.com
```

### What it does

Attempts an SSH connection to GitHub to verify authentication.

### Successful output

```bash
Hi username! You've successfully authenticated...
```

### Meaning of flags

- `ssh` → SSH client
- `-T` → disable interactive shell access

GitHub does not provide a shell session, so this is only a verification request.

---

## Typical Workflow

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
ssh -T git@github.com
```

After this, Git operations like:

```bash
git push
git pull
git clone
```

can work over SSH authentication.
