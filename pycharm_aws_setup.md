# Guide: Installing PyCharm, Configuring PyCharm, AWS SSO, and AWS Toolkit
## 1. Generate personal_access_tokens
[link to personal_access_tokens](https://gitlab.auto.buk0.com/-/user_settings/personal_access_tokens)
guild for generate personal_access_tokens and how to insert password first time in terminak
- "Add new token"
- insert name and delete expiration date select "api" in Select scopes Topic
## Generating a Personal Access Token

1. **Open the Personal Access Tokens Page**
   - Visit the [Personal Access Tokens page](https://gitlab.auto.buk0.com/-/user_settings/personal_access_tokens) on GitLab.

2. **Create a New Token**
   - Click the **"Add new token"** button.

3. **Configure the Token**
   - **Name**: Enter a descriptive name for your token.
   - **Expiration Date**: Remove the expiration date to create a non-expiring token if needed.
   - **Select Scopes**: Choose **"api"** to grant full access to the GitLab API.

4. **Generate and Copy Your Token**
   - Click **"Create personal access token"**.
   - **Copy** the token immediately and store it securely, as it will not be shown again.

## Using Your Personal Access Token in the Terminal

1. **Clone Your Repository**
   - Use the following command to clone your repository:
     ```sh
     git clone https://gitlab.auto.buk0.com/username/repository.git
     ```

2. **Authenticate with Your Token**
   - When prompted for your GitLab credentials:
     - **Username**: Enter your GitLab username.
     - **Password**: Paste the personal access token you generated.
- copy your personal key and press when clone first time only
```bash
git init
```
- create .gitignore and README.MD

```bash
git add .
```
```bash
git commit -am "initial commit"
```
```bash
git remote add origin <your-gitrepo>
git branch -M main
git push -u origin main
```
