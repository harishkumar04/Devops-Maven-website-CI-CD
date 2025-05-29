
# 🚀 Connect a GitHub Repo with AWS

## 🧱 Creating a Java Web App

Repeat the same steps from the previous part of creating and hosting the web app in the cloud. From connecting to the EC2 instance using Remote-SSH to editing the files—everything is the same as before.

---

## 🧩 Downloading Git inside the EC2 Instance

To connect to the repository and track changes in your code, Git must be installed. Run the following commands in the terminal:

```bash
sudo dnf update -y
sudo dnf install git -y
```

<img width="1440" alt="Screenshot: Git installation on EC2" src="https://github.com/user-attachments/assets/2d9d7b1b-a657-44d3-ae76-c5cd1bd94cc6" />

---

## 🔐 Generate GitHub Token

Here’s how to generate a GitHub Personal Access Token:

1. Log into your GitHub account → https://github.com/login
2. Go to Developer Settings:
   - Click your profile picture (top-right) > **Settings**
   - Scroll down and click **Developer settings**
3. Navigate to Personal Access Tokens:
   - Click **Tokens (classic)** or **Fine-grained tokens**
4. Generate a new token:
   - Click **Generate new token**
   - Name it (e.g., "EC2 Git Access")
   - Set expiration (e.g., 7 days)
5. Select required scopes:
   - `repo` → Full control of private repositories
6. Click **Generate token**

💡 **Copy and save it immediately** — you won’t be able to see it again!

---

## 🔐 Using the Token in Git

When Git prompts for your credentials:

- **Username**: your GitHub username  
- **Password**: your personal access token

```bash
git push https://github.com/yourusername/yourrepo.git
```

---

## 🧭 Initialize Git

Navigate to your web app folder and run:

```bash
git init
```

<img width="632" alt="Screenshot: Git initialization" src="https://github.com/user-attachments/assets/f9a856b4-10c7-4189-aad9-6aa45e858994" />

---

## 📤 Committing and Pushing the Changes

After initializing your repository and making changes, follow these steps:

### 1. Stage the Changes

```bash
git add .
```

> The dot (`.`) means “add all changed files in the current and subdirectories.”

### 2. Commit the Changes

```bash
git commit -m "My first commit"
```

> The `-m` flag allows you to add a brief commit message.

### 3. Push to GitHub

```bash
git push -u origin master
```

> The `-u` flag sets the upstream branch, enabling simple `git push` commands in the future.

---

## ✅ Verify Your Changes

To ensure your changes are reflected:

1. Visit your GitHub repository in a browser.
2. Check that the files and commit message have appeared.

<img width="1390" alt="Screenshot: Verified changes on GitHub" src="https://github.com/user-attachments/assets/eb57cfba-4ae0-4f01-95f5-c8e1c8fe27db" />

---

## 📝 Creating a README.md File

You can create a `README.md` file using the terminal:

```bash
touch README.md
```

Now you can add details about your project to this file.
