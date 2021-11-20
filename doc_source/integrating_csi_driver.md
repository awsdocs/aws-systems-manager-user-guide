# Use Parameter Store parameters in Amazon Elastic Kubernetes Service<a name="integrating_csi_driver"></a>

To show secrets from Secrets Manager and parameters from Parameter Store as files mounted in [Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html) pods, you can use the AWS Secrets and Configuration Provider \(ASCP\) for the [Kubernetes Secrets Store CSI Driver](https://secrets-store-csi-driver.sigs.k8s.io/)\. The ASCP works with Amazon Elastic Kubernetes Service \(Amazon EKS\) 1\.17\+\. Parameter Store is a capability of AWS Systems Manager\.

With the ASCP, you can retrieve parameters that are stored and managed in Parameter Store\. Then you can use the parameters in your workloads running on Amazon EKS\. If your parameter contains multiple key value pairs in JSON format, you can optionally choose to mount them in Amazon EKS\. The ASCP can use JMESPath syntax to query the key value pairs in your parameter\.

You can use AWS Identity and Access Management \(IAM\) roles and policies to limit access to your parameters to specific Amazon EKS pods in a cluster\. The ASCP retrieves the pod identity and exchanges the identity for an IAM role\. ASCP assumes the IAM role of the pod\. Then it can retrieve parameters from Parameter Store that are authorized for that role\. 

To learn how to integrate Secrets Manager with Amazon EKS, see [Using Secrets Manager secrets in Amazon Elastic Kubernetes Service](https://docs.aws.amazon.com/secretsmanager/latest/userguide/integrating_csi_driver.html)\.

## Installing the ASCP<a name="integrating_csi_driver_install"></a>

The ASCP is available on GitHub in the [secrets\-store\-csi\-driver\-provider\-aws](https://github.com/aws/secrets-store-csi-driver-provider-aws) repository\. The repository also contains example YAML files for creating and mounting a secret\. You first install the Kubernetes Secrets Store CSI Driver, and then you install the ASCP\.

**To install the Kubernetes Secrets Store CSI Driver and the ASCP**

1. To install the Kubernetes Secrets Store CSI Driver, run the following commands\. For full installation instructions, see [Installation](https://secrets-store-csi-driver.sigs.k8s.io/getting-started/installation.html) in the Kubernetes Secrets Store CSI Driver Book\. For information about installing Helm, see [Using Helm with Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/helm.html)\.

   ```
   helm repo add secrets-store-csi-driver https://kubernetes-sigs.github.io/secrets-store-csi-driver/charts
   helm install -n kube-system csi-secrets-store secrets-store-csi-driver/secrets-store-csi-driver
   ```

1. To install the ASCP, use the YAML file in the GitHub repository deployment directory\. For information about installing `kubectl`, see [Installing `kubectl`](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html) \.

   ```
   kubectl apply -f https://raw.githubusercontent.com/aws/secrets-store-csi-driver-provider-aws/main/deployment/aws-provider-installer.yaml
   ```

## Step 1: Set up access control<a name="integrating_csi_driver_access"></a>

To grant your Amazon EKS pod access to parameters in Parameter Store, you first create a policy that limits access to the parameters that the pod needs to access\. Then you create an [IAM role for service account](https://docs.aws.amazon.com/eks/latest/userguide/iam-roles-for-service-accounts.html) and attach the policy to it\. For more information about restricting access to Systems Manager parameters using IAM policies, see [Restricting access to Systems Manager parameters using IAM policies](sysman-paramstore-access.md)\.

**Note**  
When using Parameter Store parameters, the permission `ssm:GetParameters` is needed in the policy\.

The ASCP retrieves the pod identity and exchanges it for the IAM role\. ASCP assumes the IAM role of the pod, which gives it access to the parameters you authorized\. Other containers can't access the parameters unless you also associate them with the IAM role\. 

## Step 2: Mount parameters in Amazon EKS<a name="integrating_csi_driver_mount"></a>

To show parameters in Amazon EKS as though they are files on the filesystem, you create a `SecretProviderClass` YAML file that contains information about your parameters and how to mount them in the Amazon EKS pod\. 

The `SecretProviderClass` must be in the same namespace as the Amazon EKS pod it references\. 

### `SecretProviderClass`<a name="integrating_csi_driver_SecretProviderClass"></a>

The `SecretProviderClass` YAML has the following format\.

```
apiVersion: secrets-store.csi.x-k8s.io/v1alpha1
kind: SecretProviderClass
metadata:
   name: <NAME>
spec:
  provider: aws
  parameters:
```

**parameters**  
Contains the details of the mount request\.    
**objects**  
A string containing a YAML declaration of the parameters to be mounted\. We recommend using a YAML multi\-line string or pipe \(\|\) character\.    
**objectName**  
The friendly name of the parameter\. This becomes the file name of the parameter in the Amazon EKS pod unless you specify `objectAlias`\. Use the `Name` of the parameter, not a full Amazon Resource Name \(ARN\)\.  
**jmesPath**  
\(Optional\) A map of the keys in the JSON encoded parameter to the files to be mounted in Amazon EKS\. The following example shows what a JSON encoded parameter looks like\.  

```
{
    "username" : "myusername",
    "password" : "mypassword"
}
```
The keys are `username` and `password`\. The value associated with `username` is `myusername`, and the value associated with `password` is `mypassword`\.    
**path**  
The key in the parameter\.  
**objectAlias**  
The file name to be mounted in the Amazon EKS pod\.  
**objectType**  
Required if you don't use a Secrets Manager ARN for `objectName`\. Can be either `secretsmanager` or `ssmparameter`\.  
**objectAlias**  
\(Optional\) The file name of the parameter in the Amazon EKS pod\. If you don't specify this field, the `objectName` appears as the file name\.  
**objectVersion**  
\(Optional\) The version number of the parameter\. We recommend that you don't use this field because you must update it every time you update the parameter\. By default, the most recent version is used\. For Parameter Store parameters, you can use `objectVersion` or `objectVersionLabel` but not both\.  
**objectVersionLabel**  
\(Optional\) The parameter label for the version\. The default is the most recent version\. For Parameter Store parameters, you can use `objectVersion` or `objectVersionLabel` but not both\.

**region**  
\(Optional\) The AWS Region of the parameter\. If you don't use this field, the ASCP looks up the Region from the annotation on the node\. This lookup adds overhead to mount requests, so we recommend that you provide the Region for clusters that use large numbers of pods\.

**pathTranslation**  
\(Optional\) A single substitution character to use if the file name \(either `objectName` or `objectAlias`\) contains the path separator character, such as slash \(/\) on Linux\. If a parameter name contains the path separator, the ASCP can't create a mounted file with that name\. Instead, you can replace the path separator character with a different character by entering it in this field\. If you don't use this field, the default is underscore \(\_\), so for example, `My/Path/Parameter` mounts as `My_Path_Parameter`\.   
To prevent character substitution, enter the string `False`\.

#### Example<a name="integrating_csi_driver_example"></a>

The following example configuration shows a `SecretProviderClass` with a Parameter Store parameter resource\.

```
apiVersion: secrets-store.csi.x-k8s.io/v1alpha1
kind: SecretProviderClass
metadata:
    name: aws-secrets
spec:
    provider: aws
    parameters:
        objects: |
            - objectName: "MyParameter"
              objectType: "ssmparameter"
```

## Step 3: Update the deployment YAML<a name="integrating_csi_driver_update_deployment"></a>

Update your deployment YAML to use the `secrets-store.csi.k8s.io` driver and reference the `SecretProviderClass` resource created in the previous step\. This ensures your cluster is using the Secrets Store CSI driver\.

Below is a sample deployment YAML using a `SecretProviderClass` named `aws-secrets`\.

```
volumes:
  - name: secrets-store-inline
    csi:
      driver: secrets-store.csi.k8s.io
      readOnly: true
      volumeAttributes:
        secretProviderClass: "my-secret-provider-class"
```

## Tutorial: Create and mount a parameter in an Amazon EKS pod<a name="integrating_csi_driver_tutorial"></a>

In this tutorial, you create an example parameter in Parameter Store, and then you mount the parameter in an Amazon EKS pod and deploy it\.

Before you begin, install the ASCP\. For more information, see [Installing the ASCP](#integrating_csi_driver_install)\.

**To create and mount a secret**

1. Set the AWS Region and the name of your cluster as shell variables so you can use them in `bash` commands\. For *<REGION>*, enter the AWS Region where your Amazon EKS cluster runs\. For *<CLUSTERNAME>*, enter the name of your cluster\.

   ```
   REGION=<REGION>
   CLUSTERNAME=<CLUSTERNAME>
   ```

1. Create a test parameter\.

   ```
   aws ssm put-parameter --name "MyParameter" --value "EKS parameter" --type String --region "$Region"
   ```

1. Create a resource policy for the pod that limits its access to the parameter you created in the previous step\. For *<PARAMETERARN>*, use the ARN of the parameter\. Save the policy ARN in a shell variable\. To retrieve the parameter ARN, use `get-parameter`\.

   ```
   POLICY_ARN=$(aws --region "$REGION" --query Policy.Arn --output text iam create-policy --policy-name nginx-parameter-deployment-policy --policy-document '{
       "Version": "2012-10-17",
       "Statement": [ {
           "Effect": "Allow",
           "Action": ["ssm:GetParameter", "ssm:GetParameters"],
           "Resource": ["<PARAMETERARN>"]
       } ]
   }')
   ```

1. Create an IAM OpenID Connect \(OIDC\) provider for the cluster if you don't already have one\. For more information, see [Create an IAM OIDC provider for your cluster](https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html)\.

   ```
   eksctl utils associate-iam-oidc-provider --region="$REGION" --cluster="$CLUSTERNAME" --approve # Only run this once
   ```

1. Create the service account the pod uses, and associate the resource policy you created in step 3 with that service account\. For this tutorial, for the service account name, you use *nginx\-deployment\-sa*\. For more information, see [Create an IAM role for a service account](https://docs.aws.amazon.com/eks/latest/userguide/create-service-account-iam-policy-and-role.html#create-service-account-iam-role)\.

   ```
   eksctl create iamserviceaccount --name nginx-deployment-sa --region="$REGION" --cluster "$CLUSTERNAME" --attach-policy-arn "$POLICY_ARN" --approve --override-existing-serviceaccounts
   ```

1. Create the `SecretProviderClass` to specify which parameter to mount in the pod\. The following command uses the file location of a `SecretProviderClass` file named `ExampleSecretProviderClass.yaml`\. For information about creating your own `SecretProviderClass`, see [`SecretProviderClass`](#integrating_csi_driver_SecretProviderClass)\.

   ```
   kubectl apply -f ./ExampleSecretProviderClass.yaml
   ```

1. Deploy your pod\. The following command uses a deployment file named `ExampleDeployment.yaml`\. For information about creating your own `SecretProviderClass`, see [Step 3: Update the deployment YAML](#integrating_csi_driver_update_deployment)\.

   ```
   kubectl apply -f ./ExampleDeployment.yaml
   ```

1. To verify the parameter has been mounted properly, use the following command and confirm that your parameter value appears\.

   ```
   kubectl exec -it $(kubectl get pods | awk '/nginx-deployment/{print $1}' | head -1) cat /mnt/secrets-store/MyParameter; echo
   ```

   The parameter value appears\. 

   ```
   "EKS parameter"
   ```

## Troubleshooting<a name="integrating_csi_driver_trouble"></a>

You can view most errors by describing the pod deployment\. 

**To see error messages for your container**

1. Get a list of pod names with the following command\. If you aren't using the default namespace, use `-n <NAMESPACE>`\.

   ```
   kubectl get pods
   ```

1. To describe the pod, in the following command, for *<PODID>* use the pod ID from the pods you found in the previous step\. If you aren't using the default namespace, use `-n <NAMESPACE>`\.

   ```
   kubectl describe pod/<PODID>
   ```

**To see errors for the ASCP**
+ To find more information in the provider logs, in the following command, for *<PODID>* use the ID of the *csi\-secrets\-store\-provider\-aws* pod\.

  ```
  kubectl -n kube-system get pods
  kubectl -n kube-system logs pod/<PODID>
  ```