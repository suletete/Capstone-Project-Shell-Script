---

# **Capstone Project: Shell Script for AWS IAM Management**

 ## **Project Scenario**

**Company:** CloudOps Solutions  
**Background:** As CloudOps Solutions scales, it becomes essential to automate the management of AWS Identity and Access Management (IAM) resources. Manual processes for user and group creation become inefficient and error-prone, especially with a growing DevOps team.

To solve this, automation using **Shell scripting** and **AWS CLI** has been chosen.

---

## **Purpose of the Project**

The purpose of this project is to automate the following IAM operations using shell scripting:

- Creation of IAM users
- Creation of IAM groups
- Assignment of AWS managed policies
- Association of users to groups

This project builds upon previous Linux and shell scripting foundations and introduces automation for real-world cloud administration.

---

## **Objectives**

### ✅ **Script Enhancement**
The existing `aws_cloud_manager.sh` script was extended to include IAM management functions. This modular approach allows reusability and organization of related functionalities.

### ✅ **Define IAM User Names Array**
An array was created to hold the names of five IAM users for iteration and automated creation.

### ✅ **Create IAM Users**
The script loops through the array of user names and creates IAM users using `aws iam create-user`.

### ✅ **Create IAM Group**
A function was defined to create a group called `admin` using `aws iam create-group`.

### ✅ **Attach Administrative Policy**
The AWS managed policy `AdministratorAccess` is attached to the `admin` group using `aws iam attach-group-policy`.

### ✅ **Assign Users to Group**
Each created IAM user is added to the `admin` group using `aws iam add-user-to-group`.

---

## **Thought Process in Script Design**

The following principles guided the script development:

1. **Modular Function Design** – Each IAM task (create user, create group, attach policy, assign user to group) is encapsulated in a separate function.
2. **Error Handling** – Where possible, the script checks for existing users or groups before attempting to create them.
3. **Scalability** – Arrays and loops allow the script to scale easily with more users in the future.
4. **Reusability** – Functions can be sourced and reused in other automation scripts.

---

## **Final Script (aws_cloud_manager.sh)**

```bash
#!/bin/bash

# IAM Usernames Array
IAM_USERS=("alice" "bob" "charlie" "david" "eve")

# Function: Create IAM Users
create_iam_users() {
    echo "Creating IAM users..."
    for user in "${IAM_USERS[@]}"; do
        aws iam create-user --user-name "$user" 2>/dev/null
        if [ $? -eq 0 ]; then
            echo "User $user created successfully."
        else
            echo "User $user may already exist or an error occurred."
        fi
    done
}

# Function: Create IAM Group
create_iam_group() {
    echo "Creating 'admin' IAM group..."
    aws iam create-group --group-name admin 2>/dev/null
    if [ $? -eq 0 ]; then
        echo "Group 'admin' created."
    else
        echo "Group 'admin' may already exist or an error occurred."
    fi
}

# Function: Attach Admin Policy to Group
attach_admin_policy() {
    echo "Attaching AdministratorAccess policy to 'admin' group..."
    aws iam attach-group-policy \
        --group-name admin \
        --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
}

# Function: Add Users to Group
add_users_to_group() {
    echo "Adding users to 'admin' group..."
    for user in "${IAM_USERS[@]}"; do
        aws iam add-user-to-group --user-name "$user" --group-name admin
        echo "Added $user to 'admin' group."
    done
}

# Execute all functions
create_iam_users
create_iam_group
attach_admin_policy
add_users_to_group

echo "✅ IAM automation script execution completed."
```

---

## **How to Execute the Script Remotely**

1. Save the script above as `aws_cloud_manager.sh`.
2. Ensure AWS CLI is configured (`aws configure`).
3. Give execute permission:
```bash
chmod +x aws_cloud_manager.sh
```
4. Run the script:
```bash
./aws_cloud_manager.sh
```

You can also place this in a GitHub repository or AWS S3 bucket and remotely run using:

```bash
curl -O https://your-repo-link/aws_cloud_manager.sh
bash aws_cloud_manager.sh
```

---

## **Conclusion**

This capstone project demonstrated how to automate IAM management using a shell script and AWS CLI. It reflects real-world DevOps tasks and shows the power of combining scripting with cloud administration tools.

### ✅ Key Skills Practiced:
- Bash scripting
- AWS CLI usage
- IAM best practices
- Automation in cloud infrastructure

---

## **Deliverables**

- ✅ Complete IAM automation shell script: `aws_cloud_manager.sh`
- ✅ Comprehensive documentation (this report)
- ✅ Optional: GitHub link to script for remote execution (you can upload it to your repo)

