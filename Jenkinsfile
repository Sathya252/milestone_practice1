pipeline {
    agent any

    environment {
        WAR_FILE = "target/addressbook.war"
        TOMCAT_HOME = "/opt/tomcat9"
        INVENTORY = "/home/devops/.ssh/addressbook_repo/inventory.ini"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/Sathya252/milestone_practice.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Install Tomcat via Ansible') {
            steps {
                sh "ansible-playbook -i ${INVENTORY} /home/devops/.ssh/addressbook_repo/tomcat.yml"
            }
        }

        stage('Restart Tomcat') {
            steps {
                sh "ansible webservers -i ${INVENTORY} -m shell -a '${TOMCAT_HOME}/bin/shutdown.sh || true' --become"
                sh "ansible webservers -i ${INVENTORY} -m shell -a '${TOMCAT_HOME}/bin/startup.sh' --become"
            }
        }
    }
}
