# Guide: Installing PyCharm, Configuring PyCharm, AWS SSO, and AWS Toolkit

## Generating a Personal Access Token

1. **Open the Personal Access Tokens Page**
   - Click [here](https://gitlab.auto.buk0.com/-/user_settings/personal_access_tokens) to access the Personal Access Tokens page on GitLab.


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


##

## Install PyCharm and Intellij
**Download JetBrains Toolbox**
   - Visit the [JetBrains Toolbox App page](https://www.jetbrains.com/toolbox-app/)

### Configuring PyCharm
1. **Configure Python Interpreter**
   - Navigate to `PyCharm > Preferences > Project: <Your Project Name> > Python Interpreter`.
   - Click on the gear icon next to the interpreter dropdown and select **Add...**.
   - Select the interpreter path or create a new virtual environment.

3. **Install Plugin**
   - Visit the [Requirements plugin page](https://plugins.jetbrains.com/plugin/10837-requirements/versions#tabs).
   - In PyCharm, navigate to `PyCharm > Preferences > Plugins > ⚙️ (Gear Icon) > Install Plugin from Disk...`.
   - Search for **AWS Toolkit** and click **Install** in Marketplace.
