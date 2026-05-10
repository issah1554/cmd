# SSH Key Generation

SSH keys are used for secure authentication between your computer and remote services like GitHub, servers, or cloud platforms.

They replace passwords with cryptographic authentication.

---

# Common Use Cases

| Use Case | Example |
|---|---|
| Git authentication | GitHub, GitLab, Bitbucket |
| Server login | VPS, Linux servers |
| Deployment automation | CI/CD pipelines |
| Secure remote access | SSH terminal sessions |
| Machine-to-machine trust | Automated scripts |

---

# Generate Ed25519 Key (Recommended)

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

## What it does

- `ssh-keygen` → SSH key generation utility
- `-t ed25519` → use Ed25519 algorithm
- `-C` → add a label/comment to identify the key

---

# Why Ed25519?

Ed25519 is modern and recommended because it is:

- faster
- smaller
- more secure
- resistant to several attack classes
- supported by GitHub, GitLab, Linux, and modern SSH systems

---

# Example Flow

After running:

```bash
ssh-keygen -t ed25519 -C "stephen@example.com"
```

You may see:

```bash
Enter file in which to save the key:
/home/user/.ssh/id_ed25519
```

Press Enter to use the default location.

Then:

```bash
Enter passphrase:
```

Optional but recommended.

---

# Generated Files

## Private Key

```txt
~/.ssh/id_ed25519
```

### Purpose

- proves your identity
- used for authentication
- must remain secret

### Important

Never:
- upload it publicly
- send it to others
- commit it to Git

---

## Public Key

```txt
~/.ssh/id_ed25519.pub
```

### Purpose

Shared with:
- GitHub
- servers
- cloud providers

The server uses it to verify your private key.

Safe to share publicly.

---

# Add Key to SSH Agent

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

## Purpose

- starts SSH agent
- stores key in memory
- avoids repeated passphrase prompts

---

# Copy Public Key

## Linux

```bash
cat ~/.ssh/id_ed25519.pub
```

## macOS

```bash
pbcopy < ~/.ssh/id_ed25519.pub
```

## Windows Git Bash

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the displayed content.

---

# Add Key to GitHub

1. Open GitHub
2. Go to:

```txt
Settings → SSH and GPG Keys
```

3. Click:

```txt
New SSH Key
```

4. Paste public key

---

# Test Authentication

```bash
ssh -T git@github.com
```

## Expected Output

```bash
Hi username! You've successfully authenticated...
```

---

# RSA Keys (Older Alternative)

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

## Notes

- older algorithm
- still widely supported
- useful for legacy systems

Prefer Ed25519 unless compatibility issues exist.

---

# Typical Git Setup Workflow

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"

eval "$(ssh-agent -s)"

ssh-add ~/.ssh/id_ed25519

cat ~/.ssh/id_ed25519.pub

ssh -T git@github.com
```

---

# Multiple SSH Keys

You can create different keys for:

- personal GitHub
- work GitHub
- servers
- deployments

Example:

```txt
~/.ssh/github_personal
~/.ssh/github_work
~/.ssh/server_prod
```

Useful for separating identities and permissions.

---

# SSH Config Example

```txt
~/.ssh/config
```

```config
Host github-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/github_work

Host github-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/github_personal
```

Allows multiple GitHub accounts on one machine.
