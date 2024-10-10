pipeline {
    agent { label 'ansible-controller'}
    stages {
        stage('Preparation') {
            steps {
                script {
                
                    if (env.BRANCH_NAME == 'dev') {
                        env.INVENTORY = 'inventories/dev/inventory.init'
                    }
                }
                echo "Using inventory: ${env.INVENTORY}"
            }
        }
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Copy Files') {
            steps {
                script {
                    // Creamos los directorios necesarios en el agente
                    sh """
                    mkdir -p ~/inventories/dev ~/playbook
                    """

                    // Copiamos los archivos necesarios al agente remoto
                    sh """
                    cp ${WORKSPACE}/inventories/dev/inventory.init ~/inventories/dev/
                    cp ${WORKSPACE}/playbook/main.yml ~/playbook/
                    """
                }
            }
        }
        stage('Run Ansible Playbook from Jenkins') {
            steps {
                sh """
                ansible-playbook -i ~/${env.INVENTORY} ~/playbook/main.yml --ssh-extra-args='-o StrictHostKeyChecking=no'
                """
            }
        }
    }
}