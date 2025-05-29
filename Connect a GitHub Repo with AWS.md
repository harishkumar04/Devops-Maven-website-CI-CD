# Connect a GitHub Repo with AWS

# Creating a Java Web app

Repeat the same processes, from the previous part of Creating and hosting the Web app in the cloud.

# Generate Github Token

Here is the method on How to Generate a GitHub Token
1. Log into your GitHub account
→ https://github.com/login

2. Go to Developer Settings
 - Click your profile picture (top-right) > Settings
 - Scroll down in the left menu and click Developer settings

Navigate to Personal Access Tokens

Click Personal access tokens > Tokens (classic)
(or select Fine-grained tokens for more specific access control)

Generate a New Token

Click Generate new token

Give it a name (e.g., "EC2 Git Access")

Set expiration (e.g., 90 days or "No expiration")

Select required scopes:

repo → Full control of private repositories

workflow (if using GitHub Actions)

read:org (if using organization resources)

Generate Token

Click Generate token

🔁 Copy and save it immediately — you won’t see it again!

💡 Using the Token in Git
When Git prompts for your username and password, do this:

Username: your GitHub username

Password: your Personal Access Token

bash
Copy
Edit
git push https://github.com/yourusername/yourrepo.git
