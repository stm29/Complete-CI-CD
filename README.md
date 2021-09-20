### CI-CD
## Jenkins Pipeline

- Create Jenkins user on the server, and set password, 
   - Install publish over SSH plugin, in Manage Jenkins option --> Configure System --> set IP on SSH option 
   - You need to user same password for Global credentials and SSh [on Configure system option as well]
- You Need to create Github Access token moving to [https://github.com/settings/tokens](https://github.com/settings/tokens)
  > If, the repo is public this will not be required
- Now you Need to Get into Jenkins, Multi Branch Pipeline --> Branch Source [Give username & acces token of Github]

- [https://www.jenkins.io/doc/pipeline/steps/](https://www.jenkins.io/doc/pipeline/steps/) - Steps for DSL functionality available in Jenkins File

## Points to Remenber

- First try with root user for ssh'ing, because some user might not have proper permission to run jenkins Publish over SSH
  > it will show Access Denied even if created user have proper permission
- Remember to user **`cleanWs()`** in `post` operation in Jenkins File to clear Workspace after Job is build, if not it will show error some times
- You need to use sanme Creds for GlobalUser on Jenkins as well setting up SSH Plug-in in `Configure Jenkins --? manage system`

***
