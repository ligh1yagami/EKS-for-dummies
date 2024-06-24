# Use service account with EKS

## step 1

- Create an IAM policy that contains permissions needed by the service account. for this example we will use my-policy.json
-  ```
   {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::my-pod-secrets-bucket"
        }
    ]
   }
 
   ```

- aws iam create-policy --policy-name my-policy --policy-document file://my-policy.json
- Create an IAM role using eksctl
- ```
  eksctl create iamserviceaccount --name my-service-account --namespace default --cluster my-cluster --role-name my-role \
    --attach-policy-arn arn:aws:iam::111122223333:policy/my-policy --approve

  ```
- check the service account using kubectl
  
  `kubectl describe serviceaccount my-service-account -n default`

- For more info check the [AWS docs](https://cdocs.aws.amazon.com/eks/latest/userguide/associate-service-account-role.html)

- Create a pod/deployment and use the service account with that.
