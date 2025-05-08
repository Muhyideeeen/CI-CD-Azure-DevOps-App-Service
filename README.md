# Setting up CI/CD Pipelines in Azure DevOps to Deploy an ASP.NET App to Azure App Service

This guide explains how to set up CI/CD pipelines in Azure DevOps to deploy an ASP.NET application to Azure App Service.

## Prerequisites

- Clone the source code from the remote repository (GitHub) to your local machine:
   ```
   git clone https://github.com/chunglee-ochieze/HelloWorld
   ```

## Steps

1. Create a new organization in Azure DevOps:
    - Login to the Azure portal and search for Azure DevOps.
    - Click on Azure DevOps Organization.
    - Create a new organization and project.

2. Connect the Azure Repo to your local git repo:
    - Copy the link from your Azure Repo.
    - Run the following command using the copied link:
      ```
      git remote add Myworks.zxy https://MyWorksXYZ@dev.azure.com/MyWorksXYZ/Myworks.xyz/_git/Myworks.xyz
      ```

3. Push the source code into the Azure repo:
   - From your local repo, run the following command:
     ```
     git push Myworks.xyz master
     ```

4. Create and run an Azure Build Pipeline for the solution:
    - In the Azure DevOps environment, click on "Pipeline" and create a new pipeline.
    - Select the repository (Myworks.xyz) to create the pipeline for.
    - Configure your pipeline by selecting the framework for the app (ASP.NET Core).
    - Check the YAML file and add the trigger task below to enable continuous integration:
      ```yaml
      - task: PublishBuildArtifacts@1
        inputs:
          pathtoPublish: '$(Build.ArtifactStagingDirectory)'
          artifactName: 'Myworks.xyz'
      ```
    - Save and run the pipeline.

5. Create an Azure App Service for the solution:
    - Go to the Azure portal and search for "App Service", then click on it.
    - Click on "Create" and select "Web App".
    - Fill in the instance details appropriately:
        - Name: myworksxyz (Replace with your app's name)
        - Publish: Code
        - Runtime stack: .NET 6 (LTS)
        - Operating system: Windows
        - Region: West Europe
    - For the pricing plan, select according to your compute requirements. For this tutorial, we are using Standard S1.
    - Click on "Review and create" and then "Create" to create the app service.

6. Create a release pipeline for the solution:
    - In the Azure DevOps project environment, under the "Pipeline" blade, click on "Release".
    - Select "Azure App Service Deployment" from the provided list.
    - Give the stage a name and click on the cancel button.
    - On the new page, click on "Add artifact" and add the artifact created by your build pipeline.
    - Enable continuous deployment by clicking on the sign above the artifact box and enabling continuous deployment in the popup.
    - Close the artifact box by clicking on the cancel sign.
    - Click on "Task" next to the pipeline (above the two boxes - Artifacts and Stages).
    - From the dropdown under "Azure Subscription", select the appropriate subscription and click on "Authorize".
    - Scroll down and select the appropriate app service you created earlier.
    - Save the settings and click "OK".
    - Click on "Create release" near the save button, confirm that everything is correct, and click on "Create". You can monitor the progress of your pipeline by clicking on "Release".

7. Verify the deployment:
    - Visit the URL of your app service and check if the deployment has been successful. The page should serve your app.

## Additional Notes

After deploying the app, I discovered that it was not serving a "Hello World" page as the name suggests on GitHub. To test the CI/CD pipeline, I replaced the served `index.cshtml` file with a simple "Hello World" file, and the app service immediately started serving it. Later, I reverted back to the initial `index.cshtml` file to test the environment once more. You can find the "Hello World" file in the `code` folder, saved as `index-HelloWorld.cshtml`.

Thank you. testing
