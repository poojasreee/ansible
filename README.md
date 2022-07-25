Task -- Playbook for creating a directory in host VM's through jenkins/ADO

prequisites : 3 ansible VM's one as master controller & other 2 as slaves. so we require one VM with jenkins installed in it.
login to jenkins UI.

steps:
1. used github as a source repo. https://github.com/poojasreee/ansible
2. jenkins - manage jenkins -> manage plugins -> available -> install ansible plugins.
3. once installed we can use that through freestyle project or pipeline.
4. Install ansible & python in master server
5. configure global tool configuration add ansible path.
6. check /etc/hosts in ansible controller & add host server ip in hosts file.
7. check ansible all --list-hosts
8. Now go to jenkins UI & write pipeline script as follows

pipeline{
agent any
stages{
  stage('CheckOutCode'){
    steps{
        git branch: 'main', url: 'https://github.com/poojasreee/ansible.git'
    }
  }
  stage('Execute ansible'){
    steps{
        ansiblePlaybook become: true, becomeUser: 'azureuser', credentialsId: 'ansi', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: 'filecreate.yml'
    }
  } 
}
}

once succeded it will create a dir in your host VM.
