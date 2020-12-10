# SageMaker MLOps Project Walkthrough<a name="sagemaker-projects-walkthrough"></a>

This walkthrough demonstrates how to use MLOps projects to create a CI/CD system to build, train, and deploy models\.

**Prerequisites**

To complete this walkthrough, you need:
+ An AWS SSO or IAM account to sign in to Studio\. For information, see [Onboard to Amazon SageMaker Studio](gs-studio-onboard.md)\.
+ Permission to use SageMaker provided project templates\. For information, see [SageMaker Studio Permissions Required to Use Projects](sagemaker-projects-studio-updates.md)\.
+ Basic familiarity with the Studio user interface\. For information, see [Amazon SageMaker Studio UI Overview](studio-ui.md)\.

**Topics**
+ [Step 1: Create the Project](#sagemaker-proejcts-walkthrough-create)
+ [Step 2: Clone the Code Repository](#sagemaker-proejcts-walkthrough-clone)
+ [Step 3: Make a Change in the Code](#sagemaker-projects-walkthrough-change)
+ [Step 4: Approve the Model](#sagemaker-proejcts-walkthrough-approve)
+ [\(Optional\) Step 5: Deploy the Model Version to Production](#sagemaker-projects-walkthrough-prod)
+ [Step 6: Cleanup Resources](#sagemaker-projectcts-walkthrough-cleanup)

## Step 1: Create the Project<a name="sagemaker-proejcts-walkthrough-create"></a>

In this step, you create a SageMaker MLOps project by using a SageMaker provided project template to build, train, and deploy models\.

**To create the SageMaker MLOps project**

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Studio](gs-studio-onboard.md)\.

1. Choose **Components and registries**, and then choose **Projects** in the drop\-down list\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/projects/studio-projects.png)

1. Choose **Create project**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/projects/studio-project-create.png)

   The **Create project** tab appears\.

1. For **SageMaker project templates**, choose **Organization templates**, then choose **MLOps template for model building, training, and deployment**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/projects/projects-walkthrough-template.png)

1. For **Project details** enter a name and description for your project\.**Create project**  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/projects/studio-project-create-details.png)

When the project appears in the **Projects** list with a **Status** of **Created**, move on to the next step\.

## Step 2: Clone the Code Repository<a name="sagemaker-proejcts-walkthrough-clone"></a>

After you create the project, 2 CodeCommit repositories are created in the project, one that contains code to build and train a model and one that contains code to deploy the model\. In this step, you clone the repository to the local SageMaker that contains the code to build and train the model to the local SageMaker Studio environment so that you can work with the code\.

**To clone the code repository**

1. Choose **Components and registries**, and then choose **Projects** in the drop\-down list\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/projects/studio-projects.png)

1. Find the name of the project you created in the previous step and double\-click on it to open the project tab for your project\.

1. In the project tab, choose **Repositories**, and in the **Local path** column for the repository that ends with **modelbuild**, choose **clone repo\.\.\.**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/projects/projects-walkthrough-clone.png)

1. In the dialog box that appears, accept the defaults and choose **Clone repository**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/projects/projects-walkthrough-clone-details.png)

   When clone of the repository is complete, the local path appears in the **Local path** column\. Click on the path to open the local folder that contains the repository code in SageMaker Studio\.

## Step 3: Make a Change in the Code<a name="sagemaker-projects-walkthrough-change"></a>

Now make a change to the pipeline code that builds the model and check in the change to trigger a new pipeline run\. The pipeline run registers a new model version\.

**To make a code change**

1. In SageMaker Studio, choose the file browser icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_browser_squid.png) \), and navigate to the `pipelines/abalone` folder\. Double\-click `pipeline.py` to open the code file\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/projects/projects-walkthrough-pipeline.png)

1. In the `pipeline.py` file, find the line that sets the training instance type\.

   ```
   training_instance_type = ParameterString(
           name="TrainingInstanceType", default_value="ml.m5.xlarge"
   ```

   Change `ml.m5.xlarge` to `ml.m5.large`, then type `Ctrl+S` to save the change\.

1. Choose the **Git** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Git_squid.png) \)\. Stage, commit, and push the change in `pipeline.py`\. For information about using Git in SageMaker Studio, see [Clone a Git Repository](studio-tasks-git.md)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/projects/projects-walkthrough-commit.png)

After pushing your code change, the MLOps system triggers a run of the pipeline that creates a new model version\. In the next step, you approve the new model version to deploy it to production\.

## Step 4: Approve the Model<a name="sagemaker-proejcts-walkthrough-approve"></a>

Now you approve the new model version that was created in the previous step to trigger a deployment of the model version to a SageMaker endpoint\.

**To approve the model version**

1. Choose **Components and registries**, and then choose **Projects** in the drop\-down list\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/projects/studio-projects.png)

1. Find the name of the project you created in the first step and double\-click on it to open the project tab for your project\.

1. In the project tab, choose **Model groups**, then double\-click the name of the model group that appears\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/projects/projects-walkthrough-model-group.png)

   The model group tab appears\.

1. In the model group tab, double\-click **Version 2**\. The **Version 2** tab opens\. Choose **Update status**  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/projects/projects-walkthrough-status.png)

1. In the model **Update model version status** dialog box, in the **Status** drop\-down, choose **Approve**, then choose **Update status**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/projects/projects-walkthrough-approve.png)

   Approving the model version causes the MLOps system to deploy the model to staging\. To view the endpoint, choose the **Endpoints** tab on the project tab\.

## \(Optional\) Step 5: Deploy the Model Version to Production<a name="sagemaker-projects-walkthrough-prod"></a>

Now you can deploy the model version to the production environment\.

**Note**  
To complete this step, you need to be an administrator in your SageMaker Studio domain\. If you are not an administrator, skip this step\.

**To deploy the model version to the production environment**

1. Log in to the CodePipeline console at [https://console\.aws\.amazon\.com/codepipeline/](https://console.aws.amazon.com/codepipeline/)

1. Choose **Pipelines**, then choose the pipeline with the name **sagemaker\-*projectname*\-*projectid*\-modeldeploy**, where *projectname* is the name of your project, and *projectid* is the ID of your project\.

1. In the **DeployStaging** stage, choose **Review**\.

1. In the **Review** dialog box, choose **Approve**\.

   Approving the **DeployStaging** stage causes the MLOps system to deploy the model to production\. To view the endpoint, choose the **Endpoints** tab on the project tab in SageMaker Studio\.

## Step 6: Cleanup Resources<a name="sagemaker-projectcts-walkthrough-cleanup"></a>

To stop incurring charges, clean up the resources that were created in this walkthrough\. To do this, complete the following steps\.

**Note**  
To delete the AWS CloudFormation stack and the Amazon S3 bucket, you need to be an administrator in SageMaker Studio\. If you are not an administrator, ask your administrator to complete those steps\.

1. From the SageMaker Studio menu, choose **File **, choose **New**, and then choose **Notebook**\.

1. In the **Select Kernel** dialog box, choose **Python 3 \(Data Science\)**, then choose **Select**\.

1. In the notebook, enter the following code in a cell, and then run the cell\. Replace `MyProject` with the name of your project\.

   ```
   import boto3
   
   sm_client=boto3.client("sagemaker")
   sm_client.delete_project(ProjectName="MyProject")
   ```

   This deletes the Service Catalog provisioned product that the project created\. This includes the CodeCommit, CodePipeline, and CodeBuild resources that were created for the project\.

1. Delete the AWS CloudFormation stacks that the project created\. There are 2 stacks, one for staging and one for production\. The names of the stacks are **sagemaker\-*projectname*\-*project\-id*\-deploy\-staging** and **sagemaker\-*projectname*\-*project\-id*\-deploy\-prod**, where *projectname* is the name of your project, and *project\-id* is the ID of your project\.

   For information about how to delete a AWS CloudFormation stack, see [Deleting a stack on the AWS CloudFormation console](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-delete-stack.html) in the *AWS CloudFormation User Guide*\.

1. Delete the Amazon S3 bucket that the project created\. The name of the bucket is **sagemaker\-project\-*project\-id***, where *project\-id* is the ID of your project\.