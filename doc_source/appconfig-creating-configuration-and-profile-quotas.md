# About configuration store quotas and limitations<a name="appconfig-creating-configuration-and-profile-quotas"></a>

You can store configurations in one of the following locations:
+ Objects in an Amazon Simple Storage Service \(Amazon S3\) bucket
+ Parameters in Parameter Store
+ Documents in the Systems Manager document store

These configuration stores have the following quotas and limitations\.


****  

|  | S3 | Parameter Store | Document store | 
| --- | --- | --- | --- | 
|  **Configuration size limit**  |  1 MB Enforced by AppConfig, not S3\.  |  4 KB \(free tier\) / 8 KB \(advanced parameters\)  |  64 KB  | 
|  **Resource storage limit**  |  Unlimited  |  10,000 parameters \(free tier\) / 100,000 parameters \(advanced parameters\)  |  500 documents  | 
|  **Server\-side encryption**  |  No  |  No  |  No  | 
|  **AWS CloudFormation support**  |  Not for creating or updating data  |  Yes  |  No  | 
|  **Validate create or update API actions**  |  Not supported  |  Regex supported  |  JSON Schema required for all put and update API actions  | 
|  **Pricing**  |  See [Amazon S3 Pricing](https://aws.amazon.com/s3/pricing/)  |  See [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/)  |  Free  | 