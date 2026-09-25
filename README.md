# Reinforcement Learning: SARSA & Q-Learning on Cliff Walking

Two Jupyter notebooks implementing classic **Temporal-Difference (TD) control**
algorithms — **SARSA** and **Q-Learning** — on Gymnasium's `CliffWalking-v1`
environment.

| File | Algorithm | Status |
|---|---|---|
| `SARSA.ipynb` | SARSA (on-policy TD control) | ✅ Complete — trains for 500 episodes and renders the learned policy |
| `Q_Learning.ipynb` | Q-Learning (off-policy TD control) | 🚧 Work in progress — hyperparameters and the ε-greedy policy are set up, but the training loop itself is not written yet |

---

## Table of Contents
1. [About the Project](#about-the-project)
2. [Requirements](#requirements)
3. [Setup — Windows](#setup--windows)
4. [Setup — macOS](#setup--macos)
5. [Running the Notebooks](#running-the-notebooks)
6. [Understanding the Output](#understanding-the-output)
7. [Hyperparameters](#hyperparameters)
8. [Troubleshooting](#troubleshooting)

---

## About the Project

Both notebooks use Gymnasium's **`CliffWalking-v1`** environment:

- A 4×12 grid → **48 states**, **4 actions** (up, right, down, left)
- The agent starts at the bottom-left corner and must reach the bottom-right goal
- The entire bottom row between start and goal is a **cliff** — stepping into it
  gives a **-100** penalty and sends the agent back to start
- Every other step costs **-1** (so the agent is pushed to find the shortest safe path)

**SARSA** (on-policy) updates its Q-values using the action it *actually takes*
next, which tends to make it learn a slightly safer path further from the cliff.

**Q-Learning** (off-policy) updates using the *best possible* next action,
regardless of what the agent actually does — it tends to learn the truly
optimal (shortest) path, but can be riskier during exploration.

Both notebooks use an **ε-greedy policy**: with probability `epsilon` the agent
picks a random action (explore), otherwise it picks the best known action
(exploit).

---

## Requirements

- **Python 3.9 or newer** (developed with Python 3.13)
- `pip` (comes with Python)
- Packages: `gymnasium`, `gymnasium[toy-text]`, `numpy`, `jupyter`
  - `gymnasium[toy-text]` pulls in `pygame-ce`, which is needed for the
    rendering step at the end of each notebook

A `requirements.txt` is included in this repo — you'll install everything from
it in one command below.

---

## Setup — Windows

**Step 1: Install Python**
Download and install Python from [python.org/downloads](https://www.python.org/downloads/).
During installation, **check the box "Add python.exe to PATH"** — this saves
a lot of headaches later.

**Step 2: Confirm the install**
Open **Command Prompt** or **PowerShell** and run:
```powershell
python --version
pip --version
```
Both should print a version number.

**Step 3: Download this repository**
```powershell
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
```
(If you don't have Git installed, you can also just click **Code → Download ZIP**
on GitHub and extract it.)

**Step 4: Create a virtual environment (recommended)**
```powershell
python -m venv venv
venv\Scripts\activate
```
Your terminal prompt should now start with `(venv)`.

**Step 5: Install the dependencies**
```powershell
pip install -r requirements.txt
```

**Step 6: Launch Jupyter**
```powershell
jupyter notebook
```
This opens Jupyter in your browser. Click on `SARSA.ipynb` or `Q_Learning.ipynb`
to open it.

**Step 7: Run the notebook**
With the notebook open, click **Run → Run All Cells**, or select each cell and
press `Shift + Enter` to run them top to bottom, in order.

> **Using Anaconda instead?** The original notebooks were built with a conda
> environment (`conda-base-py`). If you prefer Anaconda: open **Anaconda Prompt**,
> run `conda create -n cliffwalk python=3.11`, then `conda activate cliffwalk`,
> then continue from Step 5 above.

---

## Setup — macOS

**Step 1: Install Python**
macOS usually ships with an old system Python — install a current one instead:
- Easiest way: install [Homebrew](https://brew.sh) if you don't have it, then run:
```bash
brew install python
```
- Or download the installer directly from [python.org/downloads](https://www.python.org/downloads/).

**Step 2: Confirm the install**
Open **Terminal** and run:
```bash
python3 --version
pip3 --version
```

**Step 3: Download this repository**
```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
```
(Git comes preinstalled on most Macs. If not, `brew install git` or install
Xcode Command Line Tools with `xcode-select --install`.)

**Step 4: Create a virtual environment (recommended)**
```bash
python3 -m venv venv
source venv/bin/activate
```
Your terminal prompt should now start with `(venv)`.

**Step 5: Install the dependencies**
```bash
pip3 install -r requirements.txt
```

**Step 6: Launch Jupyter**
```bash
jupyter notebook
```
This opens Jupyter in your default browser. Click on `SARSA.ipynb` or
`Q_Learning.ipynb` to open it.

**Step 7: Run the notebook**
With the notebook open, go to **Run → Run All Cells**, or select each cell and
press `Shift + Return` to run them top to bottom, in order.

> **Using Anaconda instead?** Open **Terminal**, run
> `conda create -n cliffwalk python=3.11`, then `conda activate cliffwalk`,
> then continue from Step 5 above.

---

## Running the Notebooks

**`SARSA.ipynb`** — run every cell in order, top to bottom:
1. Installs and imports `gymnasium`, `numpy`, `random`
2. Creates the `CliffWalking-v1` environment and prints its size
3. Sets hyperparameters and initializes the Q-table to zeros
4. Trains for 500 episodes, printing the total reward and episode length after
   each one
5. The final cell replays **one greedy episode** with `render_mode="human"` —
   a window will pop up showing the agent walking its learned path

**`Q_Learning.ipynb`** — currently only the setup is in place (imports and
hyperparameters). The actual Q-Learning training loop still needs to be written
before this notebook will do anything. It's included here as a work in progress.

---

## Understanding the Output

While SARSA trains, you'll see a line like this after every episode:
```
episode = 245/500: total reward = -19 & ep length = 19
```
- **total reward** — sum of all rewards in that episode (more negative = worse;
  a big negative spike usually means the agent fell off the cliff)
- **ep length** — how many steps the episode took

As training progresses, both numbers should trend downward in magnitude and
stabilize — that's the agent converging on a consistent, safe route from start
to goal.

---

## Hyperparameters

| Parameter | Value | Meaning |
|---|---|---|
| `alpha` | 0.5 | Learning rate — how much each update shifts the Q-value |
| `gamma` | 0.99 | Discount factor — how much future rewards are valued |
| `epsilon` | 0.1 | Exploration rate — chance of taking a random action |
| `episodes` | 500 | Number of training episodes |

Feel free to tweak these at the top of each notebook and re-run to see how
they change learning behavior.

---

## Troubleshooting

- **`ModuleNotFoundError: No module named 'gymnasium'`** — make sure your
  virtual environment is activated, then re-run `pip install -r requirements.txt`.
- **The rendering window (last cell of `SARSA.ipynb`) never appears, or errors
  out** — this needs a graphical display. It works fine on a normal Windows/Mac
  desktop, but will fail on a headless machine, SSH session, or some cloud
  notebook environments (e.g. Codespaces, plain WSL without an X server). If
  that's your situation, just skip that final cell — training still works.
- **Jupyter opens but the kernel doesn't match your virtual environment** — in
  Jupyter, go to **Kernel → Change Kernel** and pick the one matching your
  `venv` (or your conda environment).
- **`git` command not found** — install Git from
  [git-scm.com](https://git-scm.com/downloads), or just download the repo as a
  ZIP from GitHub instead of cloning it.
