# Anaconda Installation on Linux

This document describes the correct installation procedure for Anaconda on a Linux distribution using the official shell installer. The process applies to Ubuntu, Debian, Fedora, Arch, and other standard GNU/Linux systems.

---

## 1. Verify System Architecture

Before downloading the installer, confirm the CPU architecture.

```bash
# Display machine hardware architecture
uname -m
```

### Expected Output

| Output Value | Architecture Type |
| ------------ | ----------------- |
| `x86_64`     | 64-bit Intel/AMD  |
| `aarch64`    | ARM 64-bit        |

Download the installer matching the detected architecture.

---

## 2. Download the Installer

Navigate to the official documentation page:

[https://www.anaconda.com/docs/getting-started/anaconda/install](https://www.anaconda.com/docs/getting-started/anaconda/install)

Download the latest **Linux 64-bit (.sh)** installer appropriate for your architecture.

Alternatively, download directly using `wget`:

```bash
# Download latest x86_64 installer (replace URL with current version if needed)
wget https://repo.anaconda.com/archive/Anaconda3-<version>-Linux-x86_64.sh
```

Replace `<version>` with the actual release number.

---

## 3. (Optional) Verify Installer Integrity

It is recommended to verify SHA256 checksum for security.

```bash
# Generate SHA256 checksum
sha256sum Anaconda3-<version>-Linux-x86_64.sh
```

Compare the output with the checksum listed on the official website.

---

## 4. Run the Installer

### Step 1 — Grant Execute Permission

```bash
# Make installer executable
chmod +x Anaconda3-<version>-Linux-x86_64.sh
```

### Step 2 — Execute Installer

```bash
# Run the installation script
bash Anaconda3-<version>-Linux-x86_64.sh
```

### Installation Prompts

* Press `Enter` to review license.
* Type `yes` to accept license terms.
* Confirm installation directory (default: `~/anaconda3`).
* Type `yes` when prompted to initialize Conda.

---

## 5. Initialize Conda Manually (If Skipped)

If initialization was skipped during setup:

```bash
# Initialize Conda for current shell
~/anaconda3/bin/conda init
```

Restart the terminal session afterward.

---

## 6. Verify Installation

```bash
# Check Conda version
conda --version
```

### Expected Output

```
conda 24.x.x
```

If the command is not recognized, ensure the following line exists in your shell configuration file (`~/.bashrc` or `~/.zshrc`):

```bash
export PATH="$HOME/anaconda3/bin:$PATH"
```

Reload the shell configuration:

```bash
source ~/.bashrc
```

---

## 7. Update Anaconda (Recommended After Installation)

```bash
# Update Conda package manager
conda update conda

# Update all Anaconda packages
conda update anaconda
```

---

## 8. Create and Activate a Virtual Environment

```bash
# Create a new environment with specific Python version
conda create --name myenv python=3.11

# Activate environment
conda activate myenv
```

---

## 9. Launch Anaconda Navigator (Optional GUI)

```bash
anaconda-navigator
```

---

## Common Issues and Resolution

| Issue                        | Cause                          | Resolution                             |
| ---------------------------- | ------------------------------ | -------------------------------------- |
| `conda: command not found`   | Shell not initialized          | Run `conda init` and restart terminal  |
| Permission denied error      | Insufficient privileges        | Install in home directory              |
| Wrong architecture installed | Incorrect installer downloaded | Download matching architecture version |

---
