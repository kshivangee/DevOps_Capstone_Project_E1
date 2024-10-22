node{
    stage('Code checkout'){
        git 'https://github.com/kshivangee/DevOps_Capstone_Project_E1'
    }
    stage('Maven and Junit'){
        sh 'mvn clean package'
    }
    stage('Containerize'){
        // sh 'docker build -t shivangee/insureme:1.0 .'
    }
    stage('Release'){
        withCredentials([string(credentialsId: 'dockerHubPassword', variable: 'dockerHubPassword')]) {
            // sh "docker login -u shivangee -p ${dockerHubPassword}"
            // sh 'docker push shivangee/insureme:1.0'
        }
    }
    stage('Deploy to test'){
       ansiblePlaybook become: true, credentialsId: 'test-ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-test-server.yml', vaultTmpPath: ''
    }
    stage('Test Scripts code checkout'){
        git 'https://github.com/kshivangee/DevOps_Project_Testscripts.git'
    }
    stage('Compile and build the test scripts'){
        sh 'mvn clean package assembly:single'
    }
    stage('Execute the test scripts on the test server'){
        sh 'java -jar target/Selenium_test-0.0.1-SNAPSHOT-jar-with-dependencies.jar'
    }
    stage('Prod Server Code checkout'){
        git 'https://github.com/kshivangee/DevOps_Capstone_Project_E1'
    }
    stage('Deploy to Prod'){
       ansiblePlaybook become: true, credentialsId: 'test-ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-prod-server.yml', vaultTmpPath: ''
    }
}
