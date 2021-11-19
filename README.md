# aws-systems-manager-automation-playground

learn [AWS Systems Manager Automation](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-automation.html)

**SSM Automation document**

* contains one or more steps that run in sequential order
* can specify parameters
* Each step is built around a single action
* Output from one step can be used as input in a later step
* json or yaml documents
* can run python or powershell scripts via [`aws:executeScript`](https://docs.aws.amazon.com/systems-manager/latest/userguide/automation-action-executeScript.html)
* invoke [`aws:invokeLambdaFunction`](https://docs.aws.amazon.com/systems-manager/latest/userguide/automation-action-lamb.html)
* can specify execution role via `AutomationAssumeRole` parameter.  if not specified, uses the security context of calling principal
* can [trigger based on EventBridge rule](https://docs.aws.amazon.com/systems-manager/latest/userguide/automation-cwe-target.html)
* can reference parameters in Parameter Store within an SSM doc via `{{ssm:parameter-name}}`
* ssm document types (yaml or json)
  * automation (renamed to runbooks) -
  * command - remotely and securely manage the configuration of your managed instances (ec2 or on-prem)


---

```sh
```

## Examples

```sh
# create an ssm document
aws ssm create-document \
    --content file://path/to/file/documentContent.json \
    --name "document-name" \
    --document-type "Command" \
    --tags "Key=tag-key,Value=tag-value"

# run an ssm document
aws ssm start-automation-execution \
--document-name "AWS-UpdateLinuxAmi" \
--parameters "AutomationAssumeRole=arn:aws:iam::123456789012:role/SSMAutomationRole,SourceAmiId=ami-EXAMPLE,IamInstanceProfileName=EC2InstanceRole"


# run `command` type document (runs on EC2 instances)
aws ssm send-command \
    --instance-ids "instance-ID" \
    --document-name "AWS-RunShellScript" \
    --comment "IP config" \
    --parameters commands=ifconfig \
    --output text

```

---

## Resources

* [AWS Systems Manager Automation](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-automation.html)
* [AWS Systems Manager documents](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-ssm-docs.html) - describes the various document types (e.g. command, automation, etc.)
* [Custom Automation document samples](https://docs.aws.amazon.com/systems-manager/latest/userguide/automation-document-samples.html)
* [AWS Management and Governance Tools Workshop | AWS SYSTEMS MANAGER](https://mng.workshop.aws/ssm.html)
* [awslabs/aws-systems-manager](https://github.com/awslabs/aws-systems-manager) - examples
* [Automation actions reference](https://docs.aws.amazon.com/systems-manager/latest/userguide/automation-actions.html) - actions that you can specify in an AWS Systems Manager Automation document
    * [aws:executeScript – Run a script](https://docs.aws.amazon.com/systems-manager/latest/userguide/automation-action-executeScript.html) - runtimes: python3.6 | python3.7 | PowerShell Core 6.0, as of 2020-10-02 max duration of 10 min (600 secs)
    * [aws:executeAwsApi – Call and run AWS API actions](https://docs.aws.amazon.com/systems-manager/latest/userguide/automation-action-executeAwsApi.html)
* [Walkthrough: Using Automation with Jenkins](https://docs.aws.amazon.com/systems-manager/latest/userguide/automation-jenkins.html)
* [ceshihao/ssm-public-documents](https://github.com/ceshihao/ssm-public-documents) - example ssm automation documents
* [How To Create AWS SSM Automation Workflow | CloudAffaire](https://cloudaffaire.com/how-to-create-aws-ssm-automation-workflow/) - goo walkthrough how to create automation doc and run via aws cli