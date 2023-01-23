# Change Manager approval type examples<a name="approval-type-samples"></a>

The following samples demonstrate the console view and JSON content for the three types of approval types in Change Manager\.

**Topics**
+ [Sample per\-level approval configuration](#per-level-approvals)
+ [Sample per\-line approval configuration](#per-line-approvals)
+ [Sample combined per\-level and per\-line approval configuration](#combined-approval-levels)

## Sample per\-level approval configuration<a name="per-level-approvals"></a>

In the per\-level approval level setup shown in the following image, three approvals are required\. Those approvals can come from any combination of IAM users, groups, and roles that are specified as approvers\. Specified approvers include two IAM users \(John Stiles and Ana Carolina Silva\), a user group that contains three members \(`GroupOfThree`\), and a user role that represents ten users \(`RoleOfTen`\)\. 

If all three users in the `GroupOfThree` group approve the change request, it is approved for that level\. It's not necessary to receive an approval from each user, group, or role\. The minimum number of approvals can come from any combination of specified approvers\. We recommend per\-level approvals for your Change Manager operations\.

![\[Approval level showing three approvals are required and four specified approvers.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/Add-approval-2.png)

The following sample illustrates part of the YAML code for this configuration\. 

**Note**  
This version of the YAML code include an additional input, `MinRequiredApprovals` \(with an initial capital `M`\)\. The value for this input indicates how many approvals are required from among all available reviewers\. Note also that the `minRequiredApprovals` \(lowercase initial `m`\) value for each approver in the `Approvers` list is `0` \(zero\)\. This indicates that the approver can contribute to the overall approvals but is not required to do so\.

```
schemaVersion: "0.3"
emergencyChange: false
autoApprovable: false
mainSteps:
  - name: ApproveAction1
    action: aws:approve
    timeoutSeconds: 604800
    inputs:
      Message: Please approve this change request
      MinRequiredApprovals: 3
      EnhancedApprovals:
        Approvers:
          - approver: John Stiles
            type: IamUser
            minRequiredApprovals: 0
          - approver: Ana Carolina Silva
            type: IamUser
            minRequiredApprovals: 0
          - approver: GroupOfThree
            type: IamGroup
            minRequiredApprovals: 0
          - approver: RoleOfTen
            type: IamRole
            minRequiredApprovals: 0
templateInformation: >
  #### What is the purpose of this change?
    //truncated
```

## Sample per\-line approval configuration<a name="per-line-approvals"></a>

In the approval level setup shown in the following image, four approvers are specified\. These include two IAM users \(John Stiles and Ana Carolina Silva\), a user group that contains three members \(`GroupOfThree`\), and a user role that represents ten users \(`RoleOfTen`\)\. Per\-line approvals are supported for backwards compatibility but not recommended\.

![\[Approval level showing four required per-line approvers.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/Add-approval-1.png)

For the change request to be approved in this per\-line approval configuration, it must be approved by all approver lines: John Stiles, Ana Carolina Silva, one member of the `GroupOfThree` group, and one member of the `RoleOfTen` role\.

The following sample illustrates part of the YAML code for this configuration\.

**Note**  
Observe that the value for each `minRequiredApprovals` approver is `1`\. This indicates that one approval is required from each approver\.

```
schemaVersion: "0.3"
emergencyChange: false
autoApprovable: false
mainSteps:
  - name: ApproveAction1
    action: aws:approve
    timeoutSeconds: 10000
    inputs:
      Message: Please approve this change request
      EnhancedApprovals:
        Approvers:
          - approver: John Stiles
            type: IamUser
            minRequiredApprovals: 1
          - approver: Ana Carolina Silva
            type: IamUser
            minRequiredApprovals: 1
          - approver: GroupOfThree
            type: IamGroup
            minRequiredApprovals: 1
          - approver: RoleOfTen
            type: IamRole
            minRequiredApprovals: 1
executableRunBooks:
  - name: AWS-HelloWorld
    version: $DEFAULT
templateInformation: >
  #### What is the purpose of this change?
    //truncated
```

## Sample combined per\-level and per\-line approval configuration<a name="combined-approval-levels"></a>

In the combined per\-level and per\-line approval setup shown in the following image, three approvals are specified for the level, but four approvals are specified for the line\-item approvals\. Whichever approval type requires more approvals takes precedence over the other, so four approvals are required by this configuration\. Combined per\-level and per\-line approval are not recommended\.

![\[Approval level showing three approvals required for the level but four required at the line level.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/Add-approval-3.png)

```
schemaVersion: "0.3"
emergencyChange: false
autoApprovable: false
mainSteps:
  - name: ApproveAction1
    action: aws:approve
    timeoutSeconds: 604800
    inputs:
      Message: Please approve this change request
      MinRequiredApprovals: 3
      EnhancedApprovals:
        Approvers:
          - approver: John Stiles
            type: IamUser
            minRequiredApprovals: 1
          - approver: Ana Carolina Silva
            type: IamUser
            minRequiredApprovals: 1
          - approver: GroupOfThree
            type: IamGroup
            minRequiredApprovals: 1
          - approver: RoleOfTen
            type: IamRole
            minRequiredApprovals: 1
templateInformation: >
  #### What is the purpose of this change?
    //truncated
```