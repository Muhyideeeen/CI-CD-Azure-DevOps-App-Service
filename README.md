SETTING UP OF CI/CD PIPELINES IN AZURE DEVOPS TO DEPLOY an ASP.NET APP TO AZURE APP SERVICE
1.	Clone the source code from the remote repository (Github) to your local machine
git clone https://github.com/chunglee-ochieze/HelloWorld
2.	Create a new organization in Azure DevOps
i.	Login to Azure portal and search for Azure DevOps
ii.	Click on Azure DevOps Organization
 
iii.	Create a new organization and create a new project
3.	Connect the Azure Repo to your local git repo
To do this, you need to copy the link in your Azure Repo and then run the following command using the copied link
git remote add Myworks.zxy https://MyWorksXYZ@dev.azure.com/MyWorksXYZ/Myworks.xyz/_git/Myworks.xyz
4.	Push the source code into azure repo
From your local repo run the following commands
git push Myworks.xyz master
(You may have to sign in to your Azure DevOps Organization to authorize this)
5.	Create and run an Azure Build Pipeline for the solution
i.	In Azure DevOps environment, click on pipeline and create a new pipeline.
ii.	Select the repository to create the pipeline for. Here, you will have to select the repo created earlier Myworks.xyz
iii.	Configure your pipeline by selecting the framework for which the app is built, in our case it is ASP.NET Core
iv.	Check the yaml file and add the trigger task below to enable continuous integration 
  - task: PublishBuildArtifacts@1
    inputs:
      pathtoPublish: '$(Build.ArtifactStagingDirectory)'
      artifactName: 'Myworks.xyz'
v.	Save and run.
Because the created pipeline will be added to our source code in the repo you have to commit it to the repo and give a commit message.
6.	Create an Azure app service for the solution.
i.	Go to Azure portal and search for App Service, click on App Service from the search result
ii.	Click on create and select web app
iii.	In the new page, select the appropriate subscription and resource group
iv.	In the instance details, fill the following as appropriate;
-	Name: myworksxyz (This is the name of our app, give the name of yours)
-	Publish: Code
-	Runtime stack: .NET 6 (LTS)
-	Operating system: Windows
-	Region: West Europe
v.	For pricing plan, select according to your compute requirement, but for the purpose of this tutorial, we are using Standard S1, since our app is just a simple hello world app.
vi.	After this, click on review and create at the bottom left corner of the page
Note: There are other settings after this page but they are not our concern as our app is a simple hello world app. You can check out the settings and tweak them according to your preference before clicking on review and create.
vii.	After the settings has been successfully reviewed, you can click on create.
viii.	After the service has been successfully created, click on Go to Resource. Then click on the Default domain name provided in the overview board to verify that the service has been successfully deployed.
7.	Create a release pipeline for the solution.
i.	Go to Azure DevOps project environment, under the Pipeline blade, click on release.
ii.	Select Azure App Service deployment from the list provided
iii.	Give the stage name and click on the cancel button
iv.	On the new page, click on add artifact and add the artifact created by your build pipeline.
v.	At the top of the artifact box click on the sign and in the pop up, enable continuous deployment.
 
 
vi.	Click on the cancel sign to close this box
 
vii.	Click on task next to pipeline just above the two boxes (Artifacts and Stages)
 
viii.	From the dropdown under Azure Subscription, the appropriate subscription and click on authorize, you will be required to sign in to your azure account.
 
ix.	Scroll down and select the appropriate app service you created earlier
 
x.	Click on save to save the settings and click OK
xi.	Click on create release close to the save button, confirm that everything is Ok and click on create. Note that you can click on release to see the progress of your pipeline.
8.	Now that the release has been done, click on the url of your app service and check that your deployment has been successful and the page is now serving your app.
After deploying the app, I found out the app is not serving an Hello World page as contrary to the name of the app on github, so, I decided to test out the CI/CD by replacing the served index.cshtml file with a simple “Hello World” file and the hello world was immediately served by the app service. After then, I reverted back the initial index.cshtml that was served to also test the environment one more time. 
You can find the hello world file in the code folder saved as index-HelloWorld.cshtml.
Thank you.
