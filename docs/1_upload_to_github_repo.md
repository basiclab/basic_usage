# 🔒 Uploading Code to a GitHub Repository

This guide will walk you through the process of uploading your local code to a private GitHub repository.

## 📋 Prerequisites

- Git installed on your computer
- GitHub account
- SSH key set up (recommended) or Personal Access Token

## 🚀 Steps

### 1. Create a Private Repository on GitHub

1. Go to [GitHub](https://github.com)
2. Click the "+" icon in the top right corner
3. Select "New repository"
4. Name your repository
5. Select "Public" or "Private" based on your needs.
6. Click "Create repository"

> **Public Repository**
>
> - Visible to everyone on the internet
> - Great for open-source projects
> - Suitable for published research code
>
> **Private Repository**
>
> - Only visible to you and collaborators you invite
> - Access can be restricted to specific team members
> - Suitable for work-in-progress research

### 2. Initialize Git in Your Local Project (if not already done)

```bash
# Navigate to your project directory
cd your-project-directory

# Initialize git repository
git init
```

### 3. Add Your Files to Git

```bash
# Add all files (NOT RECOMMENDED)
git add .

# Or add specific files
git add file1.py file2.py
```

> Adding specific files by yourself is the best practice which allows you to track the changes of your code. If you would like to use `.`, please add `.gitignore` file first. Check [📝 Setting Up .gitignore](#-setting-up-gitignore) for more details.

### 4. Commit Your Changes

```bash
git commit -m "{commit_message}"
```

> You can add more meaningful commit messages to track the changes of your code.
> Also, check some great [emojis](https://gitmoji.dev/) to make your commit messages more expressive!
> Note that if you choose to add emoji, always use text such as `:emoji_name:` to avoid any issues.

**Advanced Tip:**

You can try [gitmoji-cli](https://github.com/carloscuesta/gitmoji-cli) to add emojis from your terminal.

### 5. Link Your Local Repository to GitHub

You should authenticate your local repository to GitHub to push your code to the remote repository. See [🔐 Authentication Methods](#-authentication-methods) first if you haven't done it yet.

```bash
# Replace with your repository URL
git remote add origin git@github.com:username/repository-name.git
```

### 6. Push Your Code

```bash
# Push to main branch
git push  origin main

# Push from local branch to a different remote branch
git push origin local_branch:remote_branch
```

> For example, to push your local `feature` branch to a remote `dev` branch:
>
> ```bash
> git push -u origin feature:dev
> ```

**Tip**: You can use `-u` to push the branch and set the upstream branch at the same time. Always be aware when using `git push` since you might not know what is the upstream branch. To see the upstream branch, you can use `git branch -vv`.

## 📝 Setting Up .gitignore

`.gitignore` is a configuration file that tells Git which files or directories to ignore in your project. It helps you:

- Keep your repository clean by excluding unnecessary files
- Prevent sensitive information from being committed (e.g., `.env`)
- Avoid committing large files or build artifacts

Create a `.gitignore` file to exclude unnecessary files:

```bash
# Create .gitignore file
touch .gitignore
```

<details>
<summary>Common .gitignore patterns for Python projects (click to expand)</summary>

```plaintext
# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
env/
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
*.egg-info/
.installed.cfg
*.egg

# Jupyter Notebook
.ipynb_checkpoints

# VS Code
.vscode/
*.code-workspace

# PyCharm
.idea/

# Environment
.env
.venv
venv/
ENV/

# OS
.DS_Store
Thumbs.db
```

</details>

> You can find more `.gitignore` templates at [gitignore.io](https://www.gitignore.io/) or [GitHub's gitignore repository](https://github.com/github/gitignore). Or you can use the extension [gitignore-generator](https://github.com/piotrpalarz/vscode-gitignore-generator) to generate your own `.gitignore` file.

## 🔐 Authentication Methods

### Using SSH (Recommended)

1. Generate SSH key if you haven't already:

   ```bash
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```

2. Add SSH key to your GitHub account:
   - Copy your public key: `cat ~/.ssh/id_ed25519.pub`
   - Go to GitHub Settings → SSH and GPG keys → New SSH key
   - Paste your key and save

### Using Personal Access Token

1. Go to GitHub Settings → Developer settings → Personal access tokens
2. Generate new token with 'repo' scope
3. Use the token as your password when pushing

## ⚠️ Common Issues and Solutions

### Authentication Failed

- Check if your SSH key is properly set up
- Verify your Personal Access Token is valid
- Ensure you have the correct permissions for the repository

### Remote Already Exists

If you get an error about remote already existing:

```bash
git remote remove origin
git remote add origin git@github.com:username/repository-name.git
```

## 💡 Best Practices

1. Always use `.gitignore` to exclude unnecessary files
2. Write meaningful commit messages
3. Keep your repository private if the code is sensitive
4. Regularly push your changes to keep your remote repository updated
