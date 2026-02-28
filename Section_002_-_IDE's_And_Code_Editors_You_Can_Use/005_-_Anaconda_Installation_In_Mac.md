## Installing Anaconda on macOS

This guide describes the correct installation procedure for Anaconda on macOS using the official distribution. The process differs slightly depending on whether the system uses **Apple Silicon (M1/M2/M3)** or **Intel x86_64** architecture.

---

## 1. Verify System Architecture

Open Terminal and execute:

```bash
uname -m
```

### Expected Output

* `arm64` → Apple Silicon Mac
* `x86_64` → Intel-based Mac

Download the installer matching the detected architecture.

---

## 2. Download the Installer

Navigate to the official Anaconda distribution page:

[https://www.anaconda.com/docs/getting-started/anaconda/install](https://www.anaconda.com/docs/getting-started/anaconda/install)

Select:

* macOS **Apple Silicon** installer (`.pkg` or `.sh`)
* macOS **Intel** installer (`.pkg` or `.sh`)

Two formats are typically available:

| Installer Type | Recommended For | Installation Method   |
| -------------- | --------------- | --------------------- |
| `.pkg`         | Standard users  | GUI installer         |
| `.sh`          | Developers      | Terminal installation |

---

## 3. Install Using Graphical Installer (.pkg)

1. Double-click the downloaded `.pkg` file.
2. Follow the installation wizard.
3. Accept license agreement.
4. Select installation location (default is recommended).
5. Complete installation.

After installation, restart Terminal.

---

## 4. Install Using Terminal Installer (.sh)

### Step 1 — Navigate to Download Directory

```bash
cd ~/Downloads
```

### Step 2 — Run the Installer

```bash
bash Anaconda3-<version>-MacOSX-<architecture>.sh
```

Replace `<version>` and `<architecture>` with the actual file name.

### Step 3 — Follow Interactive Prompts

* Press `Enter` to review license.
* Type `yes` to accept license.
* Press `Enter` to confirm installation path.
* Type `yes` when prompted to initialize Anaconda.

---

## 5. Initialize Conda (If Not Auto-Initialized)

If shell initialization was skipped, execute:

```bash
~/anaconda3/bin/conda init
```

Restart the terminal afterward.

---

## 6. Verify Installation

```bash
conda --version
```

### Expected Output

```bash
conda 24.x.x
```

If the command is not found, ensure the following exists in your shell configuration file (`~/.zshrc` for modern macOS):

```bash
export PATH="$HOME/anaconda3/bin:$PATH"
```

Then reload:

```bash
source ~/.zshrc
```

---

## 7. Update Anaconda (Recommended)

After installation:

```bash
conda update conda
conda update anaconda
```

---

## 8. Create and Activate a Virtual Environment

```bash
conda create --name myenv python=3.11
conda activate myenv
```

---

## 9. Launch Anaconda Navigator (Optional GUI)

```bash
anaconda-navigator
```

---

## Common Issues and Resolutions

| Issue                              | Cause                           | Resolution                            |
| ---------------------------------- | ------------------------------- | ------------------------------------- |
| `conda: command not found`         | PATH not configured             | Run `conda init` and restart terminal |
| Installation fails on M-series Mac | Wrong architecture downloaded   | Use Apple Silicon build               |
| Permission denied during install   | Incorrect directory permissions | Install in user home directory        |


