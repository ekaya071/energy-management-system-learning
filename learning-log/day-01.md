# Day 1 - Setting Up an Existing Python Project

**Date:** July 17, 2026

## Goal

The goal of today was to set up an existing Python engineering project, understand the development environment, and successfully run the simulation.

---

# 1. Getting the Project from GitLab

## What we did

The project was hosted on the RWTH Aachen GitLab server.

First, we opened Git Bash and cloned the repository to our local computer.

The repository URL was copied from GitLab and used with the Git clone command.

Example:

```bash
git clone <repository-url>
```

## What does cloning mean?

Cloning means creating a local copy of a remote repository.

The remote repository is stored on GitLab, and cloning downloads:

- source code
- folders
- files
- Git history
- branches information

to the local computer.

After cloning, we had a local project folder where we could work.

---

# 2. Authentication with GitLab

## Why did we need a token?

When cloning the repository, GitLab asked for authentication.

Instead of using the account password, we created a Personal Access Token.

A token is a secure way to give applications permission to access GitLab.

It works like a temporary password but provides more control.

For example, we can decide:

- which projects it can access
- what permissions it has
- when it expires

The token was used as the password during Git authentication.

---

# 3. Understanding Git Branches

## Checking branches

After cloning, we checked available branches:

```bash
git branch -a
```

The output showed:

- main
- origin/main
- origin/Esma

## Why do we use branches?

A branch allows developers to work independently without changing the main project directly.

Example:

```
main
 |
 |---- Esma
 |---- Nicolas
 |---- Other developers
```

My work was done on my own branch instead of directly on the main branch.

---

# 4. Opening the Project in Visual Studio Code

After cloning the repository, we opened the project folder in VS Code.

The project contained:

- Python scripts
- documentation files
- requirement files
- configuration files

---

# 5. Creating a Python Virtual Environment (.venv)

## What we did

Inside the project folder, we created a virtual environment:

```bash
python -m venv .venv
```

Then we activated it:

```powershell
.\.venv\Scripts\activate
```

After activation, the terminal showed:

```
(.venv)
```

which means Python was running inside the project's virtual environment.

---

## Why do we need a virtual environment?

A virtual environment creates an isolated Python environment for one project.

Different projects may require different package versions.

Without virtual environments:

```
Computer Python
 |
 |---- Project A packages
 |
 |---- Project B packages
```

Packages can conflict with each other.

With virtual environments:

```
Project A
 |
 └── .venv
     ├── numpy
     ├── pandas


Project B
 |
 └── .venv
     ├── different numpy version
     └── different packages
```

Each project has its own dependencies.

---

# 6. Selecting Python Interpreter in VS Code

After creating the virtual environment, we selected the interpreter:

```
Python: Select Interpreter
```

and chose:

```
.venv
```

This tells VS Code:

"Use the Python and packages from this project environment."

---

# 7. Installing Required Python Packages

The project contained requirement files:

Example:

```
Requirements_PC_simulation.txt
```

We installed the required packages:

```bash
pip install -r Requirements_PC_simulation.txt
```

The packages included:

- numpy
- pandas
- matplotlib
- dash
- casadi
- do-mpc

These libraries are required for simulation, analysis, and visualization.

---

# 8. Running the Simulation Post Analysis

We executed:

```bash
python Simulation_main_controller_post_analysis.py
```

The script started processing simulation data.

It performed post-analysis for different timestamps.

Example:

```
Post Analysis at 2026-04-07 09:15
```

This means the program was analyzing simulation data for that specific time.

---

# 9. Solving the Missing Output Directory Problem

During execution, we received an error:

```
FileNotFoundError:
No such file or directory:
Simulation_Controller_Post_Analysis/EMSArchive
```

## Why did this happen?

The script expected an output folder called:

```
EMSArchive
```

to store generated CSV files.

However, the folder was missing after cloning the repository.

## Solution

We manually created: (i created it from explorer)

```
Simulation_Controller_Post_Analysis
 |
 └── EMSArchive
```

After creating this folder, the script successfully continued.

---

# 10. Running the Dashboard

After successful execution, Dash started a local server:

```
Dash is running on http://127.0.0.1:8050/
```

This means the analysis results can be visualized through a local web dashboard.

---

# Key Learnings from Day 1

Today I learned:

- How Git repositories are cloned from GitLab
- How authentication with Personal Access Tokens works
- Why branches are used
- How Python virtual environments isolate dependencies
- How VS Code connects to a Python interpreter
- How requirement files manage project dependencies
- How to run an existing Python simulation project
- How to debug a missing file path problem
- How simulation results can be visualized using Dash

---

# Reflection

Today I learned that running an engineering project is not only about writing code.

Before working with the code itself, understanding the development environment, dependencies, version control system, and project structure is essential.
