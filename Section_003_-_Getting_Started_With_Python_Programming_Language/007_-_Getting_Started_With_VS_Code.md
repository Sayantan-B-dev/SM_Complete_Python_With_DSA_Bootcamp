## Why Are Environments Needed for Different Projects?

When working on multiple Python projects, each project may require different versions of libraries or even different Python versions. Installing everything globally can lead to **dependency conflicts** (e.g., Project A needs `pandas==1.5`, Project B needs `pandas==2.0`). Environments solve this by creating isolated spaces where you can install project-specific dependencies without interfering with others. They also improve **reproducibility**—you can share the exact environment setup with teammates or deploy it consistently.

Common tools for creating environments:
- **Conda** (manages Python versions and non-Python packages)
- **venv** / **virtualenv** (Python’s built-in lightweight environments)
- **pipenv**, **poetry**, etc.

---

## Explanation of Your Commands

Your commands are setting up a Conda environment specifically for use with Jupyter Notebook (via `ipykernel`). Here’s what each line does:

```bash
# 1. Find where conda is installed on your system
where conda
```
- This is a Windows command (in Anaconda Prompt or cmd) that shows the path(s) to the `conda` executable.

```bash
# 2. Load Conda functions into the current Git Bash session
source /c/Users/Virus404/anaconda3/etc/profile.d/conda.sh
```
- In Git Bash, Conda is not automatically initialized. This script makes `conda` commands available in that shell session.

```bash
# 3. Verify Conda is available
conda --version
```
- Checks that Conda is now recognized.

```bash
# 4-6. Accept the terms of service for Conda’s default channels
conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/main
conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/r
conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/msys2
```
- Starting with recent versions, Conda may require you to accept terms for each channel. These commands do that once, so you don’t get prompted later.

```bash
# 7. Create a Conda environment at the specified path (./venv) with Python 3.12
conda create -p ./venv python=3.12 -y
```
- `-p ./venv` creates the environment inside a folder named `venv` in your current directory (instead of the default envs folder).  
- `-y` automatically confirms the installation.

```bash
# 8. Activate the environment
conda activate ./venv
```
- Switches the current shell to use the Python and packages from that environment.

```bash
# 9. Install the IPython kernel package
pip install ipykernel
```
- `ipykernel` allows Jupyter to use this environment as a kernel.

```bash
# 10. Register the environment as a Jupyter kernel
python -m ipykernel install --user --name venv --display-name "Python (venv)"
```
- `--name venv` is the internal kernel name; `--display-name` is what you’ll see in Jupyter’s kernel menu.  
- `--user` installs the kernel for your current user (so it appears in Jupyter regardless of which environment Jupyter itself runs in).

After running these steps, you can start Jupyter Notebook/Lab and select the kernel named **“Python (venv)”** to run code using that environment.

---

## Detecting Kernels and Using Your `venv`

To see all available Jupyter kernels (including the one you just added):

```bash
jupyter kernelspec list
```
This shows the kernel names and their locations. The kernel you installed (`venv`) should appear.

To use it:
1. Launch Jupyter Notebook/Lab (e.g., `jupyter notebook` or `jupyter lab` from any environment that has Jupyter installed).
2. Open a notebook.
3. Click the **kernel name** in the top‑right corner (or from the **Kernel** menu) and select **“Python (venv)”**.
4. Now any code in that notebook will run with the Python and packages from your `./venv` environment.

If you want to remove the kernel later:
```bash
jupyter kernelspec uninstall venv
```

---

## Keyboard Shortcuts for Jupyter Notebook Operations

Here are the most useful shortcuts in **Command Mode** (press `Esc` to enter command mode, indicated by a blue left border) and **Edit Mode** (press `Enter` to enter edit mode, green border).

### Command Mode Shortcuts (when not typing in a cell)

| Shortcut          | Action                                      |
|-------------------|---------------------------------------------|
| `Enter`           | Enter edit mode                             |
| `Shift + Enter`   | Run current cell, select next cell          |
| `Ctrl + Enter`    | Run current cell                            |
| `Alt + Enter`     | Run current cell and insert a new cell below|
| `A`               | Insert a new cell **above** current cell    |
| `B`               | Insert a new cell **below** current cell    |
| `D` + `D`         | Delete current cell                         |
| `Z`               | Undo cell deletion                          |
| `C`               | Copy current cell                           |
| `X`               | Cut current cell                            |
| `V`               | Paste cell below                             |
| `Shift + V`       | Paste cell above                             |
| `M`               | Change cell type to **Markdown**            |
| `Y`               | Change cell type to **Code**                |
| `R`               | Change cell type to **Raw NBConvert**       |
| `Up` / `Down`     | Move to cell above/below                    |
| `Shift + Up/Down` | Select multiple cells                       |
| `Shift + M`       | Merge selected cells                        |
| `I` + `I`         | Interrupt kernel                            |
| `0` + `0`         | Restart kernel                              |
| `H`               | Show all keyboard shortcuts (help)           |
| `S`               | Save notebook                               |

### Edit Mode Shortcuts (when typing inside a cell)

| Shortcut               | Action                                     |
|------------------------|--------------------------------------------|
| `Esc`                  | Enter command mode                         |
| `Tab`                  | Code completion or indent                   |
| `Shift + Tab`          | Show docstring (tooltip)                    |
| `Ctrl + ]`             | Indent                                      |
| `Ctrl + [`             | Dedent                                      |
| `Ctrl + A`             | Select all in cell                          |
| `Ctrl + Z`             | Undo                                        |
| `Ctrl + Shift + Z`     | Redo                                        |
| `Ctrl + /`             | Toggle comment on selected lines            |
| `Ctrl + Shift + -`     | Split cell at cursor                        |
| `Ctrl + Shift + Space` | Show function parameters/help                |

These shortcuts work in Jupyter Notebook (classic) and JupyterLab (with slight variations). You can always view them in Jupyter by pressing `H` (in command mode) or checking the **Help** menu.