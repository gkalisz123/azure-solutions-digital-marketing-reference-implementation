# Lab: VSTS Team Build

In order to do this lab you'll need a Visual Studio Team Services (VSTS) Team Project with the source code loaded in a Git repo. See the **Getting Started** document if you need help. 

## Part 1: Create a Build

In this lab part, you will create a VSTS Team Build to validate your work.

1.	Access the **Build & Release** Hub in VSTS.

    ![Build & Release Hub](media/build-and-release-hub.png)

1.	Select **Builds**.

1.  Click the **+ New** button.

    ![+ New Build Button](media/new-build-button.png)

1.	In the list of build templaes, select **Visual Studio** and then select **Next**.

    ![+ New Build Button](media/create-new-build-defintion.png)

1.	Accept the defaults otherwise click **Create**.

1.	Select the **Nuget Installer** task and change the **Path to solution or packages.config** to **src/AzureKit/AzureKit - Server Only.sln**.

    ![Change Nuget Path](media/change-nuget-path.png)

1.	Select the **Visual Studio Build** task and change the **Path to solution or packages.config** to **src/AzureKit/AzureKit - Server Only.sln**.

1.	Change the **Visual Studio Version** to **Visual Studio 2015** if necessary.

    ![Set VS Version](media/set-vs2015-version.png)

1.	Select the **Visual Studio Test** task and **check** the **Code Coverage Enabled** option.

1.	Click the **Save** button.

1.	Set the build **Name** to **AzureKit - Server Only**.
	
1.	Set the build **Description** to **Initial build definition**.
	
1.	Click **OK**.

1.	In the upper right hand corner of the build definition, click the **Queue new build** button.

    ![Queue Build](media/queue-build.png)

1.	The default Queue build seetings should be fine, but If necessary adjust the settings, and click **OK** to queue the build.
	
1.	Wait for the build to complete. 

1.	When the build completes, click the link that says **Build YYYYMMDD.X** where **YYYY** is the year, **MM** is the month, etc. to open the build report.

1.	Once you're done, continue to the next part.

## Part 2: Update the Build to Support Continuous Integration

Now that you have a manual build, you'll want to clone it and create a Continuous Integration build that runs whenever changes are pushed to Master.

1.	Access the **Build & Release** Hub in VSTS.

1.	Select **Builds**. You will see a list of all your builds (only one for now).
	
1.	To the right of your build are three dots (**...**). Click it to open the menu. From the menu select **Clone**.

    ![Clone Build](media/clone-build.png)
	
1.	Click the **Triggers** tab.
	
1.	Place a check next to **Continuous integration (CI)**. 

1.	Click the **Save** button.

1.	Set the build **Name** to **CI AzureKit - Server Only**.
	
1.	Set the build **Description** to **Cloned CI AzureKit - Server Only and changed to CI build.**.
	
1.	Make a change to your code (add a comment for example), push your change and verify the build runs.

## Part 3: Update the Build to feed Release Management

In order to deploy your build using Release Management, you need a build definition that creates the correct deployment packages.

1.  Access the **Build & Release** Hub in VSTS.

1.	Select **Builds**. You will see a list of all your builds.
	
1.	As before, to the right of your **AzureKit - Server Only** build are three dots (**...**). Click it to open the menu and select **Clone**.
	
1.	Select the **Visual Studio Build** task.

1.	Change the **MSBuild Arguments** to **/t:AzureKit_PowerShell;AzureKit_Deployment**.

    ![Set MS Build](media/ms-build-args.png)

1.	Click **Add build step**.

1.  In the **Task Catalog**, scroll the list of build tasks until you find the **Visual Studio Build** task. 

1.	Select the **Visual Studio Build** task and click **Add**.

1.	Click the **Close** button.

1.	Select the newly added **Visual Studio Build** task and drag it up below the existing **Visual Studio Build** task.

1.	Select the *new* **Visual Studio Build** task and change the **Path to solution or packages.config** to **src/AzureKit/AzureKit - Server Only.sln**.

1.	Change the **MSBuild Arguments** to **/p:DeployOnBuild=true /p:WebPublishMethod=Package /p:PackageAsSingleFile=true /p:SkipInvalidConfigurations=true /p:PackageLocation="$(build.artifactstagingdirectory)"**.
	
1.	Change the **Visual Studio Version** to **Visual Studio 2015** if necessary.
	
1.	Remove the existing **Copy Files** task.
	
1.	Click **Add build step**.

1.	Select the **Utility** tab.

1.  Scroll the list of tasks until you find the **Publish Build Artifacts** task. 

1.	Select it and click **Add** *two* times.

1.	Click the **Close** button.

1.	Select the *first* newly added **Publish Build Artifacts** task.
	
1.	Change the **Path to Publish** to **src/AzureKit/AzureKit.Deployment/Templates**.

1.	Change the **Artifact Name** to **ARMTemplates**.

1.	Change the **Artifact Type** to **Server**.

    ![Publish ARM Templates](media/publish-arm-templates.png)
	
1.	Select the *second* newly added **Publish Build Artifacts** task.
	
1.	Change the **Path to Publish** to **src/AzureKit/AzureKit.Deployment/bin/Release/**.

1.	Change the **Artifact Name** to **ARMPowerShell**.

1.	Change the **Artifact Type** to **Server**.

    ![Publish ARM Templates](media/publish-arm-templates2.png)

1.	Click the **Save** button.

1.	Set the build **Name** to **CD Artifact Creation AzureKit - Server Only**.
	
1.	Set the build **Description** to **Initial build definition to create artifacts needed by Release Management**.
	
1.	Click **OK**.

1.	In the upper right hand corner of the build definition, click the **Queue new build** button.
	
1.	If necessary adjust the settings and click **OK** to queue the build.
	
1.	Wait for the build to complete. 

1.	When the build complete, click the link that says **Build YYYYMMDD.X** where **YYYY** is the year, **MM** is the month, etc. to open the build report.

1.	At the top of the report click the **Artifacts** tab. You should see three items: **ARMPowerShell**, **ARMTemplates**, and **drop**. Feel free to explore.

1.	Once you're done, you can continue to the [VSTS Release Management](Lab12-VSTS-ReleaseManagement.md) lab.