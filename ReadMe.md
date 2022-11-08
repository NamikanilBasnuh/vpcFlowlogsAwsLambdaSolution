*Deleted Vpc Flowlogs will be recreated by AWS Lambda Automation

1)Created a vpc flowlogs and S3 bucket created the destination of the flowlogs set to S3 bucket. S3 bucket arn addressed into the created vpc flowlog.

-We need to create a trusted relationship between different servisec -> lambda fuction between vpc!
That's why we are creating a file as "trust-Policy.json" into our vSCode

2)Go to CloudShell on AWS UI in order to create your policy into IAM service and also a ROLE
  2.1) copy and paste your trust-policy.json file into cat > trust-policy.json
  2.2)aws iam create-role --role-name VPCFlowLogsRole(IT can be anyName) --assume-role-policy-document file://trust-policy.json (This must match with the policy that you copy and paste with cat >) --> This is taken from Aws CLI IAM role UI ->https://docs.aws.amazon.com/cli/latest/reference/iam/index.html#cli-aws-iam

***The role created above HAS NO POLICY YET!!!! The policy that we created above is just a Trust relationship between VPC AND LAMBDA!!!!! Now, We need to create a Policy for the ROLE (VPCFlowLogsRole) that we created above and we need to attach the policy to existed ROLE (VPCFlowLogsRole)!

3))Go to CloudShell on AWS UI. We are creating a POLICY file for the ROLE (VPCFlowLogsRole) as "vpc-flow-logs-iam-role.json" into our vscode --> WE DON:T NEED TO WRITE THOSE POLICIES INTO VSCODE BUT JUST IN CASE I DO THAT!
 and cat > command create your file and copy paste into it. ( in the end control C ENTER)
https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-cwl.html

4)Go to CloudShell on AWS UI in order to create your policy into IAM service and also attach that policy to your ROLE!
command:aws iam put-role-policy --role-name VPCFlowLogsRole --policy-name VPCFlowLogsPolicy --policy-document file://vpc-flow-logs-iam-role.json

5)gO TO aws AND your role name -> you will see the CUSTOMER INLINE POLICY(VPCFlowLogsPolicy) under your ROLE (VPCFlowLogsRole)

6)NOW, We need to create another custom role and policy for LAMBDA --> This is for execution for LAMBDA!!
 6.1) Create Lambda execution policy --> lambda_execution_policy.json --> AWS IAM console (lambda policy is in VsCode as "lambda_execution_policy.json")
 6.2)Creat a role(Lambda_Flowlogs) for Lambda and attach the above policy to it!


7)Lambda create function -> Use existing Role (Lambda_Flowlogs) -> Lambda created!

8)Now we are using BOTO3!!!!-> boto3 is Aws sdk for python!!! Look at the documentation! https://boto3.amazonaws.com/v1/documentation/api/latest/index.html

9)Make sure that whatever you are going to use on code source, you need to import FIRST!!!


10)We are on Lamda Code Source now. We are going to write our code here to do the automation -> As Recreating Flowlogs if the flowlogs got deleted!!!!! For that reason search as boto3 vpc on google!

https://www.google.com/search?q=boto3+vpc&rlz=1C1CHBF_enUS889US889&oq=boto3+vpc&aqs=chrome..69i57j0i512l5j69i60l2.3584j0j4&sourceid=chrome&ie=UTF-8

11)WE WILL USE boto3 EC2 ->Client , Service Resources all the time! 

12) Client-> create_flow_logs() ....

13) WE NEED A eventBridge to Trigger the LAMBDA !! Otherwise we can not execute our code that we write on code source! -> Type eventBridge on AWS consoleee



