
# USING DVC FOR REMOTE STORAGE AND CREATE VERSIONS OF DATA

In this project i use wine quality dataset to create different version and use azure virtual machine as remote storage where i use ssh and port 22 to get and receive data from remote storage

## Configration

Before storing data on VM 
- First make folder in VM 
- Then u must have username and password/ssh-key of VM

## Installation

### Install Git in your local machine

#### For linux

```bash
    sudo apt install git-all
```
#### For windows

- [Download git](https://git-scm.com/download/win)

### Install DVC in Your Local machine

```bash
    pip install dvc
    pip isnatll dvc[ssh]
```
## Initialization

First initialize git by init or by repo

```bash
    git init
    git clone http://....../--.git
```
Second initalize DVC

```bash
    dvc init
```
After initalization of dvc u will see .dvc folder and dvc files in your folder

*Note DVC require an scm like git. Where git track files of dvc in which data path save and dvc track data files git never track data file or save in repo)

## Configure remote storage of DVC

To set the storage point of dvc fist we configure .dvc/config file by using this command

```bash
    dvc remote add -d <remote_name> ssh://<username_of_VM>@<VM_IP>:<ssh_port>/<complete_path_to_that_folder_in_VM>
```
To access the remote storage run these command

```bash
    dvc remote modify <remote_name> user <username>
    dvc remote modify <remote_name> port <port_number>
```
If VM has password the use this command

```bash
    dvc remote modify --local <remote_name> password <mypassword>
```

if vm have SSH key
```bash
    dvc remote modify --local <remote_name> keyfile </path/to/keyfile>
```
After configure remote storage .dvc/config file change then commit change in config file by

```bash
    git commit .dvc/config -m '<ur msg>'
```
## Get dataset

Now create folder in this location where you clone repo aur initialize repo by command or u can make menually 

```bash
    mkdir <foldername>
```
Paste your dataset in this folder

### ADD data in dvc
Now add data in dvc using command
```bash
    dvc add <foldername>/datafile.anyext
```
after adding u see file add in uot data folder with dvc extension

then add in git by command

```bash
    git add <foldername>/<data_withextension>.csv.dvc <foldername>/.gitignore
```
Then commit changes in file

```bash
    git commit -m 'your msg'
```
Then add tag to your data by using command

```bash
    git tag -a '<your_tag_name>' -m '<ur msg>'
```
After gining tag delete data file from folder and also delete .dvc/cache
*Note: Do not delete file having extension .dvc

to delete files use command

```bash
    rm -rf <foldername>/filename
    rm -rf .dvc/cache
```

### Add multiple data and save in remote with tags 

To add multiple dataset repeat the process with different tags 

### Add dvc files on Git

Aftre adding multiple files on remote storage with tags  run these commands

```bash
    git push origin main
    git push --tags
```
all files of dvc added in git repo which only have info of data not the data in repo when ever u use this git repository u will mension the repo link version and foldername with file name as follow

```python
    import dvc.api
    repo = 'your git hub repo url wheredvc infor saved with extension .git'
    path = '<foldername_in repowhere ur data was present>/<datafile name with extension>'
    version = '<desired version>

    dataset = dvc.api.get_url(repo,version,path)
```
This dataset variable have complete path of ur that version dataset u can download by SFTP from ur remote






