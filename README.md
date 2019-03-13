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

# How to contribute
 - Playbooks (`.yml`): must be added at the repository's root path.
 - Role dependencies: must be added in the `roles` directory as Git submodules.
