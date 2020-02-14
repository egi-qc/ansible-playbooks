# ansible-playbooks
This repository collects the Ansible playbooks used by EGI Quality Criteria. 

## umd4-repo-validation.yml

Installs all the packages included UMD4 (base, updates) repositories. The aim is to find any
(unresolved) dependency issue in the UMD4 distribution (CentOS7, Scientific Linux 6).

Jenkinsfile excerpt:

```
stage('Validation on CentOS7') {
    agent {
        label 'centos7'
    }
    steps {
        checkout([$class: 'GitSCM',
                  branches: [[name: '*/master']],
                  doGenerateSubmoduleConfigurations: false,
                  extensions: [[
                    $class: 'SubmoduleOption', 
                    disableSubmodules: false, 
                    parentCredentials: true, 
                    recursiveSubmodules: true, 
                    reference: '', 
                    trackingSubmodules: false
                  ]], 
                  submoduleCfg: [],
                  userRemoteConfigs: [[url: 'https://github.com/egi-qc/ansible-playbooks']]])
        runValidation('umd4')
    }
}
```

## jenkins-docker-slave.yml

Deploys a Docker slave for Jenkins:

- Connection through SSH
- SSH user: `jenkins`

_The playbook requires the SSH public key of Jenkins user as input_. An example:
```
ansible-playbook jenkins-docker-slave.yml --extra-vars "jenkins_sshkey_pub='ssh-rsa ..'"
```

# How to contribute
 - Playbooks (`.yml`): must be added at the repository's root path.
 - Role dependencies: must be added in the `roles` directory as Git submodules.
