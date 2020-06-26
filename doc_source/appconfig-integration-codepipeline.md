# AppConfig integration with CodePipeline<a name="appconfig-integration-codepipeline"></a>

AppConfig is an integrated deploy action for AWS CodePipeline \(CodePipeline\)\. CodePipeline is a fully managed continuous delivery service that helps you automate your release pipelines for fast and reliable application and infrastructure updates\. CodePipeline automates the build, test, and deploy phases of your release process every time there is a code change, based on the release model you define\. For more information, see [What is AWS CodePipeline?](https://docs.aws.amazon.com/codepipeline/latest/userguide/welcome.html)

The integration of AppConfig with CodePipeline offers the following benefits:
+ Customers who use CodePipeline to manage orchestration now have a lightweight means of deploying configuration changes to their applications without having to deploy their entire codebase\.
+ Customers who want to use AppConfig to manage configuration deployments but are limited because AppConfig does not support their current code or configuration store, now have additional options\. CodePipeline supports AWS CodeCommit, GitHub, and BitBucket \(to name a few\)\.

## How integration works<a name="appconfig-integration-codepipeline-how"></a>

You start by setting up and configuring CodePipeline\. This includes adding your configuration to a CodePipeline\-supported code store\. Next, you set up your AppConfig environment by performing the following tasks\.
+ [Create an application](https://docs.aws.amazon.com/systems-manager/latest/userguide/appconfig-creating-application.html)\.
+ [Create an environment](https://docs.aws.amazon.com/systems-manager/latest/userguide/appconfig-creating-environment.html)\.
+ [Create an **AWS CodePipeline** configuration profile](https://docs.aws.amazon.com/systems-manager/latest/userguide/appconfig-creating-configuration-and-profile.html)\.
+ [Choose a predefined deployment strategy or create your own](https://docs.aws.amazon.com/systems-manager/latest/userguide/appconfig-creating-deployment-strategy.html)\.

After you complete these tasks, you create a pipeline in CodePipeline that specifies AWS AppConfig as the *deploy provider*\. You can then make a change to your configuration and upload it to your CodePipeline code store\. Uploading the new configuration automatically triggers a new deployment in CodePipeline\. After the deployment completes, you can verify your changes\. For information about creating a pipeline that specifies AppConfig as the deploy provider, see [Tutorial: Create a Pipeline That Uses AppConfig as a Deployment Provider](https://docs.aws.amazon.com/codepipeline/latest/userguide/tutorials-AppConfig.html) in the *AWS CodePipeline User Guide*\. 