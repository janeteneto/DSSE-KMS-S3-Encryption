# DSSE-KMS S3 Encryption

- Instead of having one "master" S3 bucket, the idea to increase security and avoid big data leaks would be to transition into multiple buckets for secret storage divided by their functionality or category. This non-complex process can be more time-consuming and require more key and storage management. To add an additional layer of security, I suggest the use of **Dual-layer server-side encryption with keys stored in AWS Key Management Service (DSSE-KMS)**, in which I will also explain the steps.

**This would:**

- Add additional layers of protection
- Be relatively easier to implement (just replication of what has been done but for an x amount of times)
- Provide more granular access control

**The negatives are:**

- Can be more costly if we go for extra security using Dual-layer server-side encryption with keys stored in AWS Key Management Service (DSSE-KMS) ($0.003 per GB of data) - This would depend on the amount of data you have
- Requires more management for multiple buckets and separate IAM and Bucket Policies

The steps are straightforward and would go as follows:

**1. Identify and Categorise Secrets**

Review the secrets stored in the single S3 bucket and categorise them based on their functionality or sensitivity. For example, you might have secrets related to database credentials, API keys, encryption keys, or other specific purposes.

**2. Create Separate S3 Buckets**

Create individual S3 buckets for each category of secrets identified in the previous step. Use AWS Management Console, AWS CLI, or infrastructure-as-code (IaC) tools like Terraform or AWS CloudFormation to create the new buckets.

**3. Adjust IAM Policies**

Update the IAM policies associated with the new buckets to ensure that only authorised users, roles, or services have access. Apply the principle of **least privilege** and tailor the permissions according to the specific requirements of each bucket.

**4. Migrate Secrets**

Migrate the secrets from the existing single S3 bucket to the appropriate newly created buckets. This can be done manually by downloading the secrets from the original bucket and uploading them to the corresponding new buckets using AWS Management Console or AWS CLI.

**5. Update Applications and Services**

Update your applications, services, or systems that rely on these secrets to use the new bucket locations. Make the necessary configuration changes to ensure they fetch the secrets from the appropriate buckets.

**6. Test and Validate**

Verify that your applications can successfully retrieve the secrets from the newly created buckets. Perform thorough testing to ensure that all functionality and security requirements are met.

**7. Decommission the Original Bucket**

Once you have migrated all the secrets and verified the functionality of your applications, you can decommission the original single S3 bucket. Make sure to back up any necessary data or take appropriate measures based on your specific retention or compliance requirements.

- To add an additional layer of security, I suggest the use of **Dual-layer server-side encryption with keys stored in AWS Key Management Service (DSSE-KMS)**. The process is as follows:

**1. Generate a Client-Side Encryption Key**

Generate a unique encryption key using a cryptographic library or AWS SDKs. Ensure that the encryption key is securely managed and protected.

**2. Encrypt the object Locally**

Using AWS SDKs or compatible client libraries, encrypt the object locally on your machine using the client-side encryption key generated in the previous step.
Ensure that the encryption algorithm and mode used are supported by AWS SDKs and compatible with AWS KMS.

**3. Upload the Encrypted Object to Amazon S3**

Use an AWS SDK or any compatible tool to upload the encrypted object to your desired S3 bucket. During the upload, specify that server-side encryption should be applied using AWS KMS.

**4. Create an AWS KMS Key**

If you haven't already, create a Key Management Service (KMS) key in the AWS Management Console or via the AWS CLI. Please ensure the KMS key is correctly configured with appropriate policies and permissions.

**5. Set Bucket Encryption Configuration**

You can go to the Amazon S3 Management Console.
Select the desired bucket that contains the encrypted object.
Open the bucket's properties or settings.
Navigate to the "Properties" tab and click "Default encryption."
Choose "AWS Key Management Service (AWS KMS)" as the default encryption setting.

**6. Provide AWS KMS Key ID**

Specify the AWS Key Management Service (AWS KMS) Key ID or ARN associated with the KMS key you created in step 4.
This key will be used for the server-side encryption of objects within the bucket.

**7. Save the Encryption Configuration**

Save the encryption configuration to apply the changes to the bucket.


- Once these steps are completed, the object in your S3 bucket will have dual-layer server-side encryption. The object is encrypted locally using the client-side encryption key. Then it is uploaded to S3, automatically encrypted on the server side using your specified AWS KMS key.
