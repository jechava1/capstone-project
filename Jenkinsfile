node{
    stage('checkout'){
        git 'https://github.com/jechava1/capstone-project.git'
    }
    stage('maven build'){
        sh 'mvn clean package'
    }
    stage('containerize'){
        sh 'docker build -t jechava1/insure-me:1.0 .'
    }
    stage('release'){
        withCredentials([string(credentialsId: 'dockerHubPwd1', variable: 'dockerHubPwd1')]) {
            // some block
            sh "docker login -u 'jechava1' -p ${dockerHubPwd1}"
            sh "docker push jechava1/insure-me:1.0"
        }
     
    }
    stage('deploy to test'){
       
       ansiblePlaybook become: true, credentialsId: 'an-Key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-test-server.yml', vaultTmpPath: ''
    }
    stage('checkout regression test source code'){
        git 'https://github.com/jechava1/selenium-test.git'
    }
    stage ('build test scripts'){
        sh 'mvn clean package assembly:single'
    }
    stage ('execute selenium test script'){
        sh 'java -jar  target/my-app-test-0.0.1-SNAPSHOT-jar-with-dependencies.jar '
    }
     
    stage('checkout'){
        git 'https://github.com/jechava1/capstone-project.git'
    }
    
     stage('Deploy to Test'){
     ansiblePlaybook become: true, credentialsId: 'an-Key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-prod-server.yml', vaultTmpPath: ''
    }
    
    
    
}
