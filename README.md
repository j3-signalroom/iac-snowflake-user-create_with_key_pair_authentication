# IaC Initialize Terraform Snowflake User
J3 has crafted this script to significantly enhance the efficiency and security of his work at signalRoom.  By simplifying the creation of Terraform Snowflake users and automating the generation of RSA Keys, this tool empowers users to seamlessly manage Snowflake resources through Terraform. The script securely stores user information and RSA Keys in AWS Secrets Manager, ensuring safe and easy reuse without compromising security.

The motivation behind this innovation stems from a desire to streamline the entire process of creating service accounts.  J3 aimed to bundle all necessary steps into a comprehensive solution, eliminating the need to store the original `ACCOUNTADMIN` credentials when setting up a new service account. This script reflects a commitment to excellence, security, and the continual pursuit of more effective ways to manage cloud resources.

**Table of Contents**

<!-- toc -->
+ [1.0 Let's get started!](#10-lets-get-started)
    - [1.1 Snowflake](#11-snowflake)
    - [1.2 AWS Secrets Manager Secrets](#12-aws-secrets-manager-secrets)
        + [1.2.1 `/snowflake_resource`](#121-snowflake_resource)
        + [1.2.2 `/snowflake_resource/rsa_private_key`](#122-snowflake_resourcersa_private_key)
<!-- tocstop -->

## 1.0 Let's get started!

1. Take care of the cloud and local environment prequisities listed below:
    > You need to have the following cloud accounts:
    > - [AWS Account](https://signin.aws.amazon.com/) *with SSO configured*
    > - [`aws2-wrap` utility](https://pypi.org/project/aws2-wrap/#description)
    > - [Snowflake Account](https://app.snowflake.com/)

    > You need to have the following installed on your local machine:
    > - [AWS CLI version 2](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
    > - [Snowflake CLI](https://docs.snowflake.com/en/developer-guide/snowflake-cli-v2/index)

2. Clone the repo:
    ```shell
    git clone https://github.com/j3-signalroom/iac-initialize-tf_snowflake_user.git
    ```

3. From the root folder of the `iac-initialize-tf_snowflake_user/` repository that you cloned, run the script in your Terminal to create the Terraform Snowflake service account user:
    ```shell
    ./init-tf-snowflake-user.sh <create | delete> --profile=<SSO_PROFILE_NAME> \
                                                  --snowflake_account=<SNOWFLAKE_ACCOUNT> \
                                                  --snowflake_user=<SNOWFLAKE_USER> \
                                                  --snowflake_password=<SNOWFLAKE_PASSWORD> \
                                                  --snowflake_warehouse=<SNOWFLAKE_WAREHOUSE> \
                                                  --tf_snowflake_user=<TF_SNOWFLAKE_USER>
    ```
    Argument placeholder|Replace with
    -|-
    `<SSO_PROFILE_NAME>`|your AWS SSO profile name for your AWS infrastructue that houses your AWS Secrets Manager.
    `<SNOWFLAKE_ACCOUNT>`|your organization's [Snowflake account identifier](https://docs.snowflake.com/en/user-guide/admin-account-identifier).
    `<SNOWFLAKE_USER>`|your Snowflake username that has been granted `ACCOUNTADMIN` privileges.
    `<SNOWFLAKE_PASSWORD>`|your Snowflake password of the `<SNOWFLAKE_USER>`.
    `<SNOWFLAKE_WAREHOUSE>`|your Snowflake warehouse is the virtual cluster of compute resources that provides CPU, memory, and temporary storage to perform DML (Data Management Language) operations.
    `<TF_SNOWFLAKE_USER>`|your Snowflake assigned Terraform Snowflake username.


After the script successfully runs it creates the following in Snowflake and the AWS Secrets Manager for you:

### 1.1 Snowflake
Below is a picture of an example of a Terraform Snowflake service account user created with the `SYSADMIN` and `SECURITYADMIN` roles granted by the script:


### 1.2 AWS Secrets Manager Secrets
Here is the list of secrets generated by the Terraform script:

#### 1.2.1 `/snowflake_resource`
> Key|Description
> -|-
> `account`|your organization's [Snowflake account identifier](https://docs.snowflake.com/en/user-guide/admin-account-identifier).
> `tf_user`|your Snowflake assigned Terraform Snowflake username.
> `rsa_public_key`|the `tf_user` RSA public key.

#### 1.2.2 `/snowflake_resource/rsa_private_key`
The `tf_user` RSA private key.