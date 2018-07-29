# manage-multiple-git-accounts

Tutorial on managing multiple GitHub accounts

## Why ?

Most of the developers have their personal git handles and also organization handle. This will help you to manage your personal, work and other git accounts.

Note: This is intended for unix users.

## Steps

* [SSH keys](#ssh-keys)<br />
* [Add keys to your github profile](#add-keys-to-your-github-profile)<br />
* [Create configuration file to manage separate keys](#create-configuration-file-to-manage-separate-keys)<br />
* [Update stored identities](#update-stored-identities)<br />
* [Test Push](#test-push)<br />
* [Test Pull](#test-pull)<br />


# SSH keys

Assuming you've two accounts.

  * your_name@gmail.com
  * your_name@organization.com
  
Create ssh key for each account.

  ```
  cd ~/.ssh
  ```
  ```
  ssh-keygen -t rsa -C "your_name@gmail.com"
  ```
  Note: save it as id_rsa_personal when prompted
  ```
  ssh-keygen -t rsa -C "your_name@organization.com"
  ```
  Note: save it as id_rsa_organization<br />
 
You'll see following files in the ~/.ssh directory

  * id_rsa_personal
  * id_rsa_personal.pub
  * id_rsa_organization
  * id_rsa_organization.pub
  
# Add keys to your github profile

  ```
  pbcopy < ~/.ssh/id_rsa_personal.pub
  ```
  
  * Go to your personal github account
  * Click on settings
  * Select SSH keys & GPG keys
  * Add new SSH key
  * Paste the ssh key & give a name
  * Save the key
  
  Note: Repeat it for your organization account.
  
# Create configuration file to manage separate keys

Create config file in `.ssh` folder

  ```
  touch ~/.ssh/config
  ```
  
  ```
  # Personal
  Host personal
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa_personal

  # Organization
  Host organization
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa_organization
  ```

# Update stored identities

Clear current identities

  ```
  ssh-add -D
  ```

Add new keys
  
  ```
  ssh-add id_rsa_personal
  ssh-add id_rsa_organization
  ```
  
Quick test ?
 
  ```
  ssh-add -l
  ```
  
  ```
  ssh -T personal
  Hi Parthakodekart! You've successfully authenticated, but GitHub does not provide shell access.
  ```
  
  ```
  ssh -T organization
  Hi ParthaOrganization! You've successfully authenticated, but GitHub does not provide shell access.
  ```

# Test Push

  * Create a repo in your personal github
  * Clone the repo
    ```
    git clone git@github.com:kodekart/personal.git
    ```
  * Add origin
    ```
    git remote add personal git@personal:kodekart/personal.git
    ```
  * Do some changes add something
    ```
    touch README.md
    ```
  * Push
    ```
    git add --all
    git commit -m "Added readme"
    git push personal master
    ```
# Test Pull
  ```
  git pull personal master
  ```


Ref: https://mherman.org/blog/2013/09/16/managing-multiple-github-accounts/
