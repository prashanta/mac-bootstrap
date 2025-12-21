# Mac Bootstrap

```
     __  ___              ____              __       __                 
    /  |/  /___ ______   / __ )____  ____  / /______/ /__________ _____ 
   / /|_/ / __ `/ ___/  / __  / __ \/ __ \/ __/ ___/ __/ ___/ __ `/ __ \
  / /  / / /_/ / /__   / /_/ / /_/ / /_/ / /_(__  ) /_/ /  / /_/ / /_/ /
 /_/  /_/\__,_/\___/  /_____/\____/\____/\__/____/\__/_/   \__,_/ .___/ 
                                                               /_/      
```

A repository to bootstrap macOS development environments using Ansible roles.

## Overview

This project uses Ansible to automate the setup of a new macOS machine with essential development tools, shell configurations, and environment setup.

## What It Does

This bootstrap automates:
- Installation and configuration of Homebrew packages and applications
- Git user setup with aliases and default tooling
- Shell environment enhancement with Oh My Posh and Oh My Zsh
- Custom shell aliases for common commands
- All tasks are idempotent and safe to run repeatedly

## Prerequisites

- macOS 10.15 or later
- Ansible 2.9+ (`pip3 install ansible`)
- Git installed
- Administrator access (for Homebrew installation)
- `community.general` Ansible collection

## Quick Start

Run the complete bootstrap playbook:
```bash
ansible-playbook -i localhost, -c local ansible/bootstrap.yml
```

If Homebrew needs sudo access:
```bash
ansible-playbook -i localhost, -c local ansible/bootstrap.yml --become --ask-become-pass
```

## Usage

### Run All Roles

Execute the full bootstrap:
```bash
ansible-playbook -i localhost, -c local ansible/bootstrap.yml
```

### Run Specific Role

Run only Git configuration:
```bash
ansible-playbook -i localhost, -c local ansible/bootstrap.yml --tags git-config
```

Run only Homebrew:
```bash
ansible-playbook -i localhost, -c local ansible/bootstrap.yml --tags homebrew
```

Run only Oh My Posh:
```bash
ansible-playbook -i localhost, -c local ansible/bootstrap.yml --tags oh-my-posh
```

Run only Oh My Zsh:
```bash
ansible-playbook -i localhost, -c local ansible/bootstrap.yml --tags oh-my-zsh
```

Run only aliases:
```bash
ansible-playbook -i localhost, -c local ansible/bootstrap.yml --tags aliases
```

### Dry-Run (Preview Changes)

See what would be changed without making modifications:
```bash
ansible-playbook -i localhost, -c local ansible/bootstrap.yml --check --diff -v
```

Dry-run for a specific role:
```bash
ansible-playbook -i localhost, -c local ansible/bootstrap.yml --tags git-config --check --diff -v
```

### Verbose Output

Get detailed execution information:
```bash
# One level of verbosity
ansible-playbook -i localhost, -c local ansible/bootstrap.yml -v

# Two levels (more detail)
ansible-playbook -i localhost, -c local ansible/bootstrap.yml -vv

# Maximum detail
ansible-playbook -i localhost, -c local ansible/bootstrap.yml -vvv
```

### Custom Variables

Override defaults at runtime:
```bash
ansible-playbook -i localhost, -c local ansible/bootstrap.yml -e "git_user_name=NewName"
ansible-playbook -i localhost, -c local ansible/bootstrap.yml -e "use_homebrew=false"
```

### Syntax Check

Validate playbook syntax before running:
```bash
ansible-playbook -i localhost, -c local ansible/bootstrap.yml --syntax-check
```

## Configuration

### Customize Homebrew Packages

Edit `ansible/roles/homebrew/defaults/main.yml` to add/remove packages:
```yaml
use_homebrew: true
brew_packages:
  - arduino-cli
  - azure-cli
  - cpplint
brew_casks:
  - jumpcut
```

Or pass as variable:
```bash
ansible-playbook -i localhost, -c local ansible/bootstrap.yml \
  -e "brew_packages=['git', 'python3']"
```

### Customize Git Configuration

Edit `ansible/roles/git/defaults/main.yml` and set your Git identity:
```yaml
git_user_name: "Your Name"
git_user_email: "you@example.com"
git_diff_tool: "vimdiff"
```

Important: change `git_user_name` and `git_user_email` to your personal values. Alternatively, set them directly with Git:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Or provide them at runtime to Ansible without changing files:

```bash
ansible-playbook -i localhost, -c local ansible/bootstrap.yml -e "git_user_name='Your Name' git_user_email='you@example.com'"
```

Verify your global Git settings:

```bash
git config --global --list
```

### Customize Aliases

Edit `ansible/templates/aliases.j2` to add or modify shell aliases.

### Customize Oh My Posh Theme

Update the `oh_my_posh_config_url` variable in `ansible/bootstrap.yml`:
```yaml
vars:
  oh_my_posh_config_url: "https://your-custom-config-url"
```

## Troubleshooting

**Command not found errors after setup:**
Reload your shell configuration:
```bash
source ~/.zshrc
# or start a fresh login shell:
exec $SHELL -l
```

**Homebrew installation fails:**
Run with privilege escalation:
```bash
ansible-playbook -i localhost, -c local ansible/bootstrap.yml --become --ask-become-pass
```

**Git configuration not applied:**
Verify the configuration was written:
```bash
git config --global --list
cat ~/.gitconfig
```

## License

MIT License - see LICENSE file for details.