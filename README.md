# Building a dev ops toolchain on IBM Cloud

These steps will guide you through creating a toolchain that monitors a source repository, and builds and redeploys a Cloud Foundry application on every code commit. Utilizing DevOp tool integrations, and configuring a delivery pipeline to automatically build and deploy Cloud Foundry applications to IBM Cloud, you can focus on your application's code.



## 1. Use existing Git Repository
This guide assumes you have a GitLab project repository created for your team. For example: https://git.ng.bluemix.net/justin.mccoy/jam-toolchain-demo.git

![](docs/images/gitlab_overview.png)

1. Go to https://git.ng.bluemix.net
2. Your team's project will have been created for you

![](docs/images/gitlab_project_view.png)

3. Click on the gray **Add readme** button in the center of the new repo
4. You don't have to type anything in, we just need the file created. Click **Commit Changes**.
5. Click on the **ssh** dropdown and select **https**
6. Copy the git repo address and save for later

### Creating a GitLab access token 
1. Click on the drop down at the very top right of page and select **Settings**

![](docs/images/gitlab_settings.png)

2. Select **Access Tokens** on the left nav panel

![](docs/images/gitlab_select_accesstoken.png)

3. Give the token a name and select the **api** checkbox under the **Scopes** section.
4. Click **Create personal access token**

![](docs/images/gitlab_create_accesstoken.png)

5. Copy and save your *gitlab access token* to a text document; we will need it later. Once you leave the page you can't retrieve it again.

## Creating a toolchain

In this section we will set up a DevOps Toolchain which will allow us to integrate our git repo with different tools including a delivery pipeline. From your IBM Cloud Dashboard [https://console.bluemix.net](https://console.bluemix.net) ensure you're using the Wells Fargo Account.

![](docs/images/set_account.png)
_Click on your account in the upper right corner and select **Wells Fargo**_

### Starting a new toolchain

1. From the IBM Cloud Dashboard [https://console.bluemix.net](https://console.bluemix.net)
2. Click on the menu button at the top left and select **DevOps**

![](docs/images/devops.png)

3. Make sure the *location* drop down is the region closest to you. If unsure, select **Dallas**. Make sure your *resource group* matches your team name, or **default**. Click on **Create Toolchain** on the upper right side of the page.

![](docs/images/toolchain_overview.png)

4. Select **Build your own toolchain** at the bottom
5. Give the toolchain a name and verify the *region* and *resource group*
6. Click **Create**

### Adding tool integrations

![](docs/images/toolchain_integrations.png)

1. Once the toolchain is created, click **Add tool**
2. Select **Git Repos and Issue Tracking**
3. In the *repository type* dropdown, select **existing**
4. Enter the git repo address that you saved earlier.
5. Select the **Track deployment of code changes** checkbox
6. Click **Create Integration**

![](docs/images/create_integration.png)

7. Click **Add Tool** again
8. Select **Delivery Pipeline**
9. Give the pipeline a name and click **Create Integration**
10. Click **Add Tool** again
11. Select **Eclipse Orion Web IDE** and click **Create Integration**

![](docs/images/toolchain_setup.png)

## Creating the delivery pipeline

In this section we will configure an automated delivery pipeline that will detect changes pushed to the GitLab source code repository, build any dependencies, and deploy to IBM cloud.

### Creating the build stage

1. Click on the **Delivery Pipeline** integration box
2. Click **Add Stage**
3. Ensure that *Git repository* is the input type and that the repo you created earlier is in the *Git URL*
4. Select the **Jobs** tab
5. Click **Add Job** and select **Build**.  *Builder type* should be set to **Simple**
6. Click **Save**

### Creating the deploy stage

1. Click **Add Stage**
2. Ensure that *build artifacts* is the input type and that *stage* and *job* reference the build stage we just created.
3. Select the **Jobs** tab
4. Click **Add Job** and select **Deploy**
5. We will need to create a new IBM Cloud API key in order for this stage to work. Click on **Manage** at the top of the page, hover over **Security** and right click to open **Platform API Keys** in a new browser tab.

![](docs/images/manage_api_key.png)

6. On the API key page click on the blue **Create** button
7. Give the API key a name and click on **Create**
8. Copy the API key
9. Go back to the browser tab that has the pipeline and deploy stage.
10. In the *API Key* dropdown, select *Enter an existing API key* and paste in the key we just made.
11. Ensure the *Organization* is **TeamworksNG**
12. Click **Save**

## Sample Projects
Download code from one of these sample Cloud Foundry Projects as a base for your project:

* **Node.js** [Hello World Web App](https://github.com/IBM-Cloud/node-helloworld)
* **Python Flask** [Hello World App]()
* **Python Django** [Hello World App]()
* **ASP.NET Core** [Hello World App](https://github.com/IBM-Cloud/aspnet-core-helloworld)
* **Golang** [Hello World Web App](https://github.com/IBM-Cloud/go-hello-world)
* **Java JAX_RS** [Hello World Web App](https://github.com/IBM-Cloud/java-web-service)
* **Java Liberty** [Hello World Web App](https://github.com/IBM-Cloud/java-helloworld)
* **Swift** [Hello World Web App](https://github.com/IBM-Cloud/swift-helloworld)
* **Ruby on Rails** [Hello World Web App](https://github.com/IBM-Cloud/ruby-rails-helloworld)

## Deploying changes

In this final section, we will test our delivery pipeline by pushing code changes. We will show how to push the code both locally through the git cli, and through the browser with the Orion Web IDE. 

### If developing locally

1. In your termainal, navigate to where you want to clone your project and enter the following command, replacing "token" and "giturl" with your actual *gitlab access token* and git url from the first section. Your git url should start with git.ng.bluemix.net/... and end in .git

```
git clone https://oauth2:token@giturl
```

**Example**
```
git clone https://oauth:zg1rdrYj7_yjracBYcE_@git.ng.bluemix.net/justin.mccoy/jam-toolchain-demo.git
```

2. Make your code changes
3. Add, commit, and push your code like you normally would using the git cli
4. The pipeline should kick off automatically

### If developing in the browser
####
Orion Web IDE 
1. Go back to your toolchain page that shows the integrations you have selected.

![](docs/images/toolchain.png)

2. Click on the **Eclipse Orion Web IDE** integration

![](docs/images/orion.png)

3. In the file explorer, expand your project to see all the files
4. Make your code changes
5. When done making changes, click on the git icon that is under the pencil on the very left side of the page.
6. Review the file changes and enter a commit message. Click **Commit**
7. On the left side of the page under *Outgoing changes* click on **Push**
8. The pipeline should kick off automatically.
