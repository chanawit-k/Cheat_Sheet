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


## Install PyCharm and Configuration
**Download JetBrains Toolbox**
- Visit the [JetBrains Toolbox App page](https://www.jetbrains.com/toolbox-app/)

### Configuring PyCharm
1. **Configure Python Interpreter**
    - Navigate to **PyCharm > Preferences > Project: <Your Project Name> > Python Interpreter**.
    - Click on the gear icon next to the interpreter dropdown and select **Add...**.
    - Select the interpreter path or create a new virtual environment.

2. **Install Plugin**
    - Visit the [Requirements plugin page](https://plugins.jetbrains.com/plugin/10837-requirements/versions#tabs).
    - In PyCharm, navigate to `PyCharm > Preferences > Plugins > ⚙️ (Gear Icon) > Install Plugin from Disk...`.
    - Search for **AWS Toolkit** and click **Install** in Marketplace.
3. **Set Shared Folder as Source Root**
    - Right-click on **Shared** folder you want to set as the Source Root.
    - Select **Mark Directory as > Sources Root**.

## Configure AWS SSO in `.aws/config`
- Click [here](https://github.com/benkehoe/aws-sso-credential-process) for tutorial
1. **Setup `.aws/config` File**
   - Open or create the `.aws/config` file in your home directory (`~/.aws/config`).

2. **Create or Update `config` File**
   - Add the AWS SSO profile configuration:
     ```ini
     [profile ka-sso-apim-dev-Administrator]
     credential_process = aws-sso-credential-process --profile ka-sso-apim-dev-Administrator --interactive
     sso_start_url = https://ksauto.awsapps.com/start
     sso_region = ap-southeast-1
     sso_account_id = 644481786460
     sso_role_name = PowerUser
     sso_interactive_auth = true
     region = ap-southeast-1
     output = json
     ```
3. **Create or Update `credentials` File**
   - Open or create the `.aws/credentials` file in your home directory (`~/.aws/credentials`).
   - Add your AWS access key ID and secret access key obtained from the AWS SSO console:
     ```ini
     [default]
     aws_access_key_id = <your-saccess_key_id>
     aws_secret_access_key = <your-secret-access-key>
     ```
## how to config run configuration
1. Select **Use the currently selected credentail profile/region** in the **run/debug Configuration** menu.

2. Add environment variables from your Lambda function:
   - Navigate to **configuration > environment variable** in your Lambda function settings.
   - Copy the environment variables.
   - Paste these variables into the **Environment variable section** of the run/debug Configuration in PyCharm.
