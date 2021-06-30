pipeline {
agent {
        kubernetes {
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: test-odu
spec:
  securityContext:
    runAsUser: 10000
    runAsGroup: 10000
  containers:
  - name: jnlp
    image: 'jenkins/jnlp-slave:4.3-4-alpine'
    args: ['\$(JENKINS_SECRET)', '\$(JENKINS_NAME)']
  - name: json-lint
    image: peterdavehello/jsonlint
    command:
    - cat
    tty: true
    securityContext: # https://github.com/GoogleContainerTools/kaniko/issues/681
      runAsUser: 0
      runAsGroup: 0
  volumes:
  - name: regsecret
    projected:
      sources:
      - secret:
          name: regsecret
          items:
            - key: .dockerconfigjson
              path: config.json
  imagePullSecrets:
  - name: oduregsecret
  - name: regsecret
"""
        }
    }
Skip to content
Search or jump to…

Pull requests
Issues
Marketplace
Explore
 
@anilsaini1212 
anilsaini1212
/
jsonlint
1
00
Code
Issues
Pull requests
Actions
Projects
Wiki
Security
Insights
Settings
jsonlint/Jenkinsfile
@anilsaini1212
anilsaini1212 update
Latest commit 932346a 4 minutes ago
 History
 1 contributor
61 lines (60 sloc)  1.51 KB
  
pipeline {
agent {
        kubernetes {
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: test-odu
spec:
  securityContext:
    runAsUser: 10000
    runAsGroup: 10000
  containers:
  - name: jnlp
    image: 'jenkins/jnlp-slave:4.3-4-alpine'
    args: ['\$(JENKINS_SECRET)', '\$(JENKINS_NAME)']
  - name: ansible-molecule
    image: quay.io/ansible/toolset
    command:
    - cat
    tty: true
    securityContext: # https://github.com/GoogleContainerTools/kaniko/issues/681
      runAsUser: 0
      runAsGroup: 0
  volumes:
  - name: regsecret
    projected:
      sources:
      - secret:
          name: regsecret
          items:
            - key: .dockerconfigjson
              path: config.json
  imagePullSecrets:
  - name: oduregsecret
  - name: regsecret
"""
        }
    }
    stage ('SonarQube') {
      steps {
        container('ansible-molecule') {
        sh """
           apt-get update
           apt-get install wget -y
           apt-get install unzip -y
           wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.6.2.2472-linux.zip
           unzip sonar-scanner-cli-4.6.2.2472-linux.zip
           mkdir /opt/sonar
           mv sonar-scanner-4.6.2.2472-linux /opt/sonar/
           export PATH=$PATH:/opt/sonar/sonar-scanner-4.6.2.2472-linux/bin
           mv sonarconf /opt/sonar/sonar-scanner-4.6.2.2472-linux/conf/sonar-scanner.properties
           sonar-scanner
           """
              }
             }
           }      
         }
       }
    
