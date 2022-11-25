## Before the hands-on lab

**Duration**: 60 minutes

## Overview

Before the hands-on lab, you will need to prepare the environment by deploying the database and the application locally on a virtual machine using Docker and MongoDB. You will also need to fork the GitHub repository containing the lab to your own GitHub account to be able to set up the CI/CD pipeline.

You should follow all the steps provided in this section _before_ taking part in the hands-on lab ahead of time as some of these steps take time.

>**Note**: Please go through every instruction properly while performing the before hands-on-lab. If at all the lab guide is not followed correctly, attendees will be facing issues in next part of the lab due to the dependency.

### Task 1: Set up Azure Cloud Shell

1. In the **JumpVM** provided to you on the left side, double click on the **Azure Portal** browser shortcut on the desktop.
 
   ![](media_prod/azureportal.png "Cloud Shell Bash Window")
   
1. On **Sign in to Microsoft Azure** tab, **Sign in** with following Azure credentials.
 
     * Azure Usename/Email: <inject key="AzureAdUserEmail"></inject> 
 
     * Azure Password: <inject key="AzureAdUserPassword"></inject> 
 
1. If you see the pop-up **Stay Signed in?**, click Yes. 
 
1. If you see the pop-up **You have free Azure Advisor recommendations!**, close the window to continue the lab. 
 
1. If a **Welcome to Microsoft Azure** popup window appears, click **Maybe Later** to skip the tour.

1. In the **Azure portal**, open the **Azure Cloud Shell** by clicking on the cloud shell icon in the top menu bar. Alternatively, you can open cloud shell by navigating to ```https://shell.azure.com```.

   ![The cloud shell icon is highlighted on the menu bar.](media_prod/cloudshell.png)

1. After launching the Azure Cloud Shell, select the **Bash** option.
   
   ![Permissions GH](media_prod/cna20.png)
   
1. Now in You have no storage mounted dialog box, Click on **Show advanced settings**. 

   ![Permissions GH](media_prod/cna21.png)
   
5. In You have no storage mounted dialog box, Select Create new under Storage account and provide values as below: 
  
   - **Storage account** : **storage<inject key="DeploymentID" enableCopy="false"/>**
   - **File Share** : **blob**

    ![This is a screenshot of the cloud shell opened in a browser window. Bash was selected](media_prod/b4-image36-new.png)

   >**Note**: If the Azure CloudShell gets stuck. Please try restarting the CloudShell and continue with the lab.

### Task 2: Download Starter Files

In this task, you use `git` to copy the lab content to your cloud shell so that the lab starter files will be available.

1. Copy the following command to clone the lab files using cloudshell and check out the starter files from the MCW Cloud-native applications GitHub repository and detach them from the existing remote repository via the following commands:

   ```bash
   cd ~
   git clone \
      --depth 1 \
      --branch main \
      https://github.com/microsoft/MCW-Cloud-native-applications.git \
      MCW-Cloud-native-applications
   cd MCW-Cloud-native-applications
   ```

   ![In this screenshot of a Bash window, git clone has been typed and run at the command prompt. The output from git clone is shown.](media_prod/bhol-starterfiles.png "Bash Git Clone")
   
### Task 3: Create GitHub Personal Access Token

1. In a new browser tab open ```https://www.github.com``` and Log in with your personal GitHub account.

    > **Note** : You have to use your own GitHub account. If you don't have a GitHub account then navigate to the following link ```https://github.com/join ``` and create one.
    
2. Create a Personal Access Token as described below:

   - In the upper-right corner of your GitHub page, select your profile photo, then click **Settings**.

     ![Permissions GH](media_prod/github-01.png)
    
   - From the left hand side menu, select **Developer settings**.
     
     ![Permissions GH](media_prod/github-devset.png)

   - Now on the left sidebar, expand **Personal access tokens**, select **Tokens (classic) (1)**, click on **Generate new token (2)** and select **Generate new token (classic) (3)**. Provide the GitHub password if prompted. 
  
     ![Permissions GH](media_prod/BHOL-T3-S2.3.png)
     
3. Select the scopes or permissions you would like to grant this token

    - **Note**: Provide the following text in the note field, **<inject key="DeploymentID" enableCopy="false"/>-token**. 
    
    - **Select scopes**: Select the following scopes when configuring your GitHub Personal Access Token

        - `repo` - Full control of private repositories
        - `workflow` - Update GitHub Action workflows
        - `write:packages` - Upload packages to GitHub Package Registry
        - `delete:packages` - Delete packages from GitHub Package Registry
        - `read:org` - Read org and team membership, read org projects

      ![Permissions GH](media_prod/image10-new.png)

    - Leave other values as default and select **Generate token**.

      ![Permissions GH](media_prod/github-05.png)

4. Click on the Copy icon to copy the token to your clipboard and save it on your notepad. For security reasons, after you navigate off the page, you will not be able to see the token again. **DO NOT COMMIT THIS TO YOUR REPO!**

   ![Permissions GH](media_prod/github-06.png)
   
   > **Note**: Use Personal Access Token as Password when ever you asked to provide Password while pushing any Git changes in the Lab.

### Task 4: Create a GitHub repository

FabMedical has provided starter files for you. They have taken a copy of the websites for their customer Contoso Neuro and refactored it from a single node.js site into a website with a content API that serves up the speakers and sessions. This refactored code is a starting point to validate the containerization of their websites. Use this to help them complete a POC that validates the development workflow for running the website and API as Docker containers and managing them within the Azure Kubernetes Service environment.

1. Open a web browser and navigate to <https://www.github.com>. Log in using your GitHub account credentials.

2. In the upper-right corner, expand the user drop-down menu and select **Your repositories**.

    ![The user menu is expanded with the Your repositories item selected.](media/2020-08-23-18-03-40.png "User menu, your repositories")

3. Next to the search criteria, locate and select the **New** button.

    ![The GitHub Find a repository search criteria is shown with the New button selected.](media/2020-08-23-18-08-02.png "New repository button")

4. On the **Create a new repository** screen, name the repository `Fabmedical` and select the **Create repository** button.

    ![Create a new repository page with Repository name field and Create repository button highlighted.](media/2020-08-23-18-11-38.png "Create a new repository")

5. On the **Quick setup** screen, copy the **HTTPS** GitHub URL for your new repository, paste this into notepad for future use.

    ![Quick setup screen is displayed with the copy button next to the GitHub URL textbox selected.](media/2020-08-23-18-15-45.png "Quick setup screen")

### Task 5: Set up Azure Cloud Shell environment

1. Set the following environment variables in an Azure Cloud Shell terminal. Make sure you replace all the values properly.

   > **NOTE**: You can replace the **DeploymentID** with **<inject key="DeploymentID" />**.

   ```bash
   export MCW_SUFFIX=<PASTE DeploymentID>                   # Needs to be a unique letter string
   export MCW_GITHUB_USERNAME=<PASTE YOUR GITHUB USERNAME> # Your Github account username
   export MCW_GITHUB_TOKEN=<PASTE YOUR GITHUB PAT TOKEN>        # A personal access token for your Github account
   export MCW_GITHUB_URL=https://github.com/$MCW_GITHUB_USERNAME/Fabmedical 
   export MCW_GITHUB_EMAIL=<YOUR GITHUB EMAIL ID>
   ```
   
1. Go to Environment details, Click on **Service principle Credentials** and copy the **Application Id (Client Id)** , **client Secret** , **subscription Id** and **tenant Id**.

   ![](https://github.com/Shivashant25/MCW-Cloud-native-applications/blob/prod-1/Hands-on%20lab/media/cna8.png?raw=true)

   - Replace the values that you copied in below Json
   
   ```json
   { "clientId": "<client id>", "clientSecret": "<client secret>", "subscriptionId": "<subscription id>", "tenantId": "<tenant id>", "activeDirectoryEndpointUrl": "https://login.microsoftonline.com", "resourceManagerEndpointUrl": "https://management.azure.com/", "activeDirectoryGraphResourceId": "https://graph.windows.net/", "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/", "galleryEndpointUrl": "https://gallery.azure.com/", "managementEndpointUrl": "https://management.core.windows.net/" }
   ```
   
   - Copy the complete JSON output to your clipboard.


1. In GitHub, return to the **Fabmedical** repository screen, and select the **Settings** tab.

1. From the left menu, select **Secrets (1)**. Under Security, expand **Secrets (1)** by clicking the drop-down and select **Actions (2)** blade from the left navigation bar. Select the **New repository secret (3)** button.

    ![Settings link, Secrets link, and New secret button are highlighted.](media/cna18.png "GitHub Repository secrets")

1. In the **New secret** form, enter the name `AZURE_CREDENTIALS` and paste the copied value from your clipboard to the value of the secret and click on **Add secret**.

   ![Permissions GH](media_prod/git_secret.png)

1. Paste the following command to go the right directory and to create a bash file named **bhol.sh**.

   ```bash
   cd ~/MCW-Cloud-native-applications/Hands-on\ lab/lab-files/developer/scripts
   vi bhol.sh
   ```
   
1. In the new bash the file, paste the following commands.

   ```bash
   #!/bin/bash

   function replace_json_field {
       tmpfile=/tmp/tmp.json
       cp $1 $tmpfile
       jq "$2 |= \"$3\"" $tmpfile > $1
       rm "$tmpfile"
   }

   # Check if SUFFIX envvar exists
   if [[ -z "${MCW_SUFFIX}" ]]; then
       echo "Please set the MCW_SUFFIX environment variable to a unique three character string."
       exit 1
   fi

   if [[ -z "${MCW_GITHUB_USERNAME}" ]]; then
       echo "Please set the MCW_GITHUB_USERNAME environment variable to your Github Username"
       exit 1
   fi

   if [[ -z "${MCW_GITHUB_TOKEN}" ]]; then
       echo "Please set the MCW_GITHUB_TOKEN environment variable to your Github Token"
       exit 1
   fi

   if [[ -z "${MCW_GITHUB_URL}" ]]; then
       MCW_GITHUB_URL=https://$MCW_GITHUB_TOKEN@github.com/$MCW_GITHUB_USERNAME/Fabmedical.git
   fi

   git config --global user.email "$MCW_GITHUB_EMAIL"
   git config --global user.name "$MCW_GITHUB_USERNAME"

   cp -R ~/MCW-Cloud-native-applications/Hands-on\ lab/lab-files/developer ~/Fabmedical
   cd ~/Fabmedical
   git init
   git remote add origin $MCW_GITHUB_URL

   git config --global --unset credential.helper
   git config --global credential.helper store

   # Configuring github workflows
   cd ~/Fabmedical
   sed -i "s/\[SUFFIX\]/$MCW_SUFFIX/g" ~/Fabmedical/.github/workflows/content-init.yml
   sed -i "s/\[SUFFIX\]/$MCW_SUFFIX/g" ~/Fabmedical/.github/workflows/content-api.yml
   sed -i "s/\[SUFFIX\]/$MCW_SUFFIX/g" ~/Fabmedical/.github/workflows/content-web.yml

   # Commit changes
   git add .
   git commit -m "Initial Commit"

   # Get ACR credentials and add them as secrets to Github
   ACR_CREDENTIALS=$(az acr credential show -n fabmedical$MCW_SUFFIX)
   ACR_USERNAME=$(jq -r -n '$input.username' --argjson input "$ACR_CREDENTIALS")
   ACR_PASSWORD=$(jq -r -n '$input.passwords[0].value' --argjson input "$ACR_CREDENTIALS")

   GITHUB_TOKEN=$MCW_GITHUB_TOKEN
   cd ~/Fabmedical
   echo $GITHUB_TOKEN | gh auth login --with-token
   gh secret set ACR_USERNAME -b "$ACR_USERNAME"
   gh secret set ACR_PASSWORD -b "$ACR_PASSWORD" 

   # Committing repository
   cd ~/Fabmedical
   git branch -m master main
   git push -u origin main
   ```
   
   > Press `i` on your keyboard to enter insert mode, where you can alter the file.

1. Save the file and exit Vim.

   > You can do this by pressing the `Esc` key on your keyboard, followed by `:wq`.

   > **Note**: If you face issue in saving the file in integrated environment, please remote to the LabVM using the credentials provided in the environment details tab and perform this step.

1. Run the following command to execute the `bhol.sh` script. This will provision all of the Azure cloud resources necessary to execute the workshop.

   ```bash
   bash bhol.sh
   ```
   >**NOTE**: When asked for the credentials, Please enter your `GITHUB_USERNAME` and `PAT` as the password. 

1. Upon successful execution of the `bhol.sh` script, Connect to build agent vm using the **Command to Connect to Build Agent VM**, which is given on lab environment details page.

1. When asked to confirm if you want to continue connecting, type `yes`.

1. When asked for the password, enter **Build Agent VM Password** given below.

   * Azure Password: **<inject key="Build Agent VM Password"></inject>**

1. SSH connects to the VM and displays a command prompt such as the following. Keep this cloud shell window open for the next step.

   `adminfabmedical@fabmedical:~$`

   ![In this screenshot of a Cloud Shell window, ssh -i .ssh/fabmedical adminfabmedical@52.174.141.11 has been typed and run at the command prompt. The information detailed above appears in the window.](media/b4-image27.png "Azure Cloud Shell Connect to Host")

### Task 6: Complete the build agent setup

1. From an Azure Cloud Shell terminal, use the SSH command output from the previous task and start an active SSH session to the build agent VM.

1. Clone the Fabmedical GitHub repository created in the previous task by replacing the `GITHUB_USERNAME` in the below mentioned command.

   ```bash
   git clone https://github.com/<GITHUB_USERNAME>/Fabmedical
   ```
   
   >**Note**: Run `ls` command to make sure the repository is cloned properly.

1. Set the following environment variables in the active SSH session to the build agent VM. Use the same GitHub access token used in a previous task.

   > **NOTE**: You can replace the **DeploymentID** with **<inject key="DeploymentID" />**..

   ```bash
   export MCW_SUFFIX=<PASTE DeploymentID>                   # Needs to be a unique letter string
   export MCW_GITHUB_USERNAME=<GITHUB USERNAME> # Your Github account username
   export MCW_GITHUB_TOKEN=<GITHUB PAT>         # A personal access token for your Github account
   ```

1. Update the `create_build_environment.sh` script to set up the build agent VM environment.

   ```bash
   cd ~/Fabmedical/scripts
   vi create_build_environment.sh
   ```
   
1. update the **node.js version** by replacing `curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -` with `curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -`. The updated bash file should look as mentioned in the screeenshot below.

   ![](media_prod/cna50.png)

1. Run the `create_build_environment.sh` script to set up the build agent VM environment. This script installs necessary dependencies on the build agent VM and applies the configuration settings to the VM's environment necessary for proper execution of the workshop.

   ```bash
   cd ~/Fabmedical/scripts
   bash create_build_environment.sh
   ```
   
   > **Note**: Type '**N**' and press enter when asked for sharing the anonymous usage data with Angular team.

   > **Note**: Ignore any errors you encounter regarding the Docker client. That will be resolved after joining a new SSH session in the following steps.

1. After the script completes execution, type `exit` to exit the SSH session. We will need to join a new SSH session to ensure the docker environment on the build agent VM has completed set up.

   ![](media_prod/exit-bhl1.png)

1. After reestablishing an SSH session to the build agent VM, run the `create_and_seed_database.sh` script to create and seed the MongoDB database for use in the workshop.

   ```bash
   cd ~/Fabmedical/scripts
   bash create_and_seed_database.sh
   ```

### Task 7: Build Docker Images

1. Navigate to the `content-api` directory and build the `content-api` container image using the Dockerfile in the directory. Note how the deployed Azure Container Registry is referenced. Replace the `SUFFIX` placeholder with **<inject key="DeploymentID" />**.

   ```bash
   cd ~/Fabmedical/content-api
   docker image build -t fabmedical[SUFFIX].azurecr.io/content-api:latest .
   ```

2. Repeat this step for the `content-web` image, which serves as the application front-end. Replace the `SUFFIX` placeholder with **<inject key="DeploymentID" />**.

   ```bash
   cd ~/Fabmedical/content-web
   docker image build -t fabmedical[SUFFIX].azurecr.io/content-web:latest .
   ```

3. Observe the built Docker images by running `docker image ls`. The images were tagged with `latest`, but it is possible to use other tag values for versioning.

   ![This image demonstrates the tagged Docker images: content-api and content-web.](./media/docker-images.png "Tagged Docker images")

4. Log in to Azure Container Registry using the below mentioned command. Replace the Credentials with the following details mentioned below.

   - LOGINSERVER: **<inject key="Acr LoginServer" />**
   - USERNAME: **<inject key="Acr Username" />**
   - PASSWORD: **<inject key="Acr Password" />**

   ```
   docker login [LOGINSERVER] -u [USERNAME] -p [PASSWORD]
   ```

   ![This image demonstrates the credentials for Azure Container Registry.](./media/acr-credentials.png "ACR credentials")

5. Push the two images you built. Replace the `SUFFIX` placeholder with **<inject key="DeploymentID" />**.

   ```bash
   docker image push fabmedical[SUFFIX].azurecr.io/content-api:latest
   docker image push fabmedical[SUFFIX].azurecr.io/content-web:latest
   ```
   
   >**Note**: You should follow all steps provided _before_ performing the Hands-on lab.

6. Click on the **Next** button present in the bottom-right corner of this lab guide.



