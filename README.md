# SAMTemplateforAWSLambda
Cloud Formation | SAM Template | Lambda | AWS Code Pipeline | Python | AWS Textract | IAM Policies


Reflink: https://docs.aws.amazon.com/lambda/latest/dg/build-pipeline.html

Steps to create a AWS code pipeline for Lambda using Cloud formation Template:

1. Use existing or create a new bucket for the pipeline 
2. Create a role that gives AWS CloudFormation permission to access AWS resources. 
    - Trusted entity – AWS CloudFormation
    - Permissions – AWSLambdaExecute
    - Role name – cfn-lambda-pipeline
3. Under the Permissions tab, choose Add inline policy. In Create Policy, choose the JSON tab and add the policy attached in my repo cfn-lambda-pipeline.json
4. Create a new AWS pipeline that deploys your application.
5. Then create a new repository in AWS code commit or connect with your existing GitHub Repository.
6. Choose a detection option as "AWS CodePipeline" both GitHub Repository or AWS code commit repository.(Github Webhook didn't worked as a webhook for my pipeline)
7. Make sure you have uploaded your latest python application code, template.yml and buildspec.yml in your SCM Repository.
8. Create the AWS CodeBuild build project and select AWS CodeBuild as a option for build process and fill up the required fields such as project name, buildspecname etc and click on Next.
    - Project name – lambda-pipeline-build
    - Operating system – Ubuntu
    - Runtime – Standard
    - Runtime version – aws/codebuild/standard:2.0
    - Image version – Latest
    - Buildspec name – buildspec.yml
9.Configure deploy stage settings and choose Next,
    - Deploy provider – AWS CloudFormation
    - Action mode – Create or replace a change set
    - Stack name – lambda-pipeline-stack
    - Change set name – lambda-pipeline-changeset
    - Template – BuildArtifact::outputtemplate.yml
    - Capabilities – CAPABILITY_IAM, CAPABILITY_AUTO_EXPAND
    - Role name – cfn-lambda-pipeline
    - Choose Create pipeline.
10. Choose Create pipeline.

Note: The pipeline fails the first time it runs because it needs additional permissions. In the next section, you add permissions to the role that's generated for your build stage.

11. During the build stage, AWS CodeBuild needs permission to upload the build output to your Amazon S3 bucket.
    - Choose codebuild-lamba-pipeline-build-service-role.
    - Choose Attach policies.
    - Attach AmazonS3FullAccess.

12. Update the deployment stage
    - Open your pipeline in the Developer tools console.
    - Choose Edit.
    - Next to Deploy, choose Edit stage.
    - Choose Add action group.
    - Configure deploy stage settings and choose Next.
    - Action name – execute-changeset
    - Action provider – AWS CloudFormation
    - Input artifacts – BuildArtifact
    - Action mode – Execute a change set
    - Stack name – lambda-pipeline-stack
    - Change set name – lambda-pipeline-changeset
    - Choose Done.
    - Choose Save.
    - Choose Release change to run the pipeline.
    
Your pipeline is ready. Push changes to the master branch to trigger a deployment.




