# Managing parameter tiers<a name="parameter-store-advanced-parameters"></a>

AWS Systems Manager Parameter Store includes *standard parameters * and *advanced parameters*\. You individually configure parameters to use either the standard\-parameter tier \(the default tier\) or the advanced\-parameter tier\. 

You can change a standard parameter to an advanced parameter at any time, but you can’t revert an advanced parameter to a standard parameter\. Reverting an advanced parameter to a standard parameter would result in data loss because the system would truncate the size of the parameter from 8 KB to 4 KB\. Reverting would also remove any policies attached to the parameter\. Also, advanced parameters use a different form of encryption than standard parameters\. For more information, see [How AWS Systems Manager Parameter Store Uses AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/services-parameter-store.html) in the *AWS Key Management Service Developer Guide*\.

If you no longer need an advanced parameter, or if you no longer want to incur charges for an advanced parameter, you must delete it and recreate it as a new standard parameter\. 

The following table describes the differences between the tiers\.


****  

|  | Standard | Advanced | 
| --- | --- | --- | 
|  Total number of parameters allowed \(per AWS account and Region\)  |  10,000  |  100,000  | 
|  Maximum size of a parameter value  |  4 KB  |  8 KB  | 
|  Parameter policies available  |  No  |  Yes For more information, see [Assigning parameter policies](parameter-store-policies.md)\.  | 
|  Cost  |  No additional charge  |  Charges apply For more information, see [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/)\.  | 

**Topics**
+ [Specifying a default parameter tier](ps-default-tier.md)
+ [Changing a standard parameter to an advanced parameter](parameter-store-advanced-parameters-enabling.md)