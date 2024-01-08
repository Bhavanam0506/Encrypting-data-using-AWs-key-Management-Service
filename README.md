# Encrypting-data-using-AWs-key-Management-Service
**creating file in ec2 and encrypting and decrypting that data using aws key management system**

**1. Create AWS key Management service Key:**

login to aws console.
search for KMS and click on create key.
select Symmetric for key type.

![image](https://github.com/Bhavanam0506/Encrypting-data-using-AWs-key-Management-Service/assets/92242002/2a54e34d-5d00-43f6-bbf6-fa1b26256ccd)

**2.Configure your EC2 Instance**


cd ~
aws configure

![image](https://github.com/Bhavanam0506/Encrypting-data-using-AWs-key-Management-Service/assets/92242002/73086ad3-a6cb-4d4b-9247-50087278a9f7)

AWS Access Key ID: Enter 1, and then press Enter.
AWS Secret Access Key: Enter 1, and then press Enter.
Default region name: choose your region
Default output format: Press Enter

**To see your the AWS credentials file, run the following command:**

vi ~/.aws/credentials

**now change your credentials**

[default]
aws_access_key_id= your access key
aws_secret_access_key= your secret key
aws_session_token= your session token 
note: you can create it in your aws console 

**To install the AWS Encryption CLI and set your path, run the following commands:**

pip3 install aws-encryption-sdk-cli
export PATH=$PATH:/home/ssm-user/.local/bin


**For Encryption and decryption create file**

touch file1.txt file2.txt
echo 'TOP SECRET 1!!!' > file1.txt

enter cmd to store KMS arn in variable KeyARN
KeyArn=(your_KeyArn)

**To create a directory to output the encrypted file, run the following command**

mkdir output

**To encrypte the file**

aws-encryption-cli --encrypt \
                     --input secret1.txt \
                     --wrapping-keys key=$keyArn \
                     --metadata-output ~/metadata \
                     --encryption-context purpose=test \
                     --commitment-policy require-encrypt-require-decrypt \
                     --output ~/output/.
 run ls cmd to see encrypted file is created or not
                     
**To decrypte the file**

aws-encryption-cli --decrypt \
                     --input secret1.txt.encrypted \
                     --wrapping-keys key=$keyArn \
                     --commitment-policy require-encrypt-require-decrypt \
                     --encryption-context purpose=test \
                     --metadata-output ~/metadata \
                     --max-encrypted-data-keys 1 \
                     --buffer \
                     --output .

                     
**output**

I have created a file11.txt with content : TOP SECRET 1!!!
as in image you can see the file is successfully encrypted and decrypted


![image](https://github.com/Bhavanam0506/Encrypting-data-using-AWs-key-Management-Service/assets/92242002/728e91ad-74cb-458e-ad9e-40557988c739)
