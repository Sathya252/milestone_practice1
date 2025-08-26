pipeline {
    agent any

    environment {
        WAR_FILE = "target/addressbook.war"
        TOMCAT_HOME = "/opt/tomcat9"
        INVENTORY = "addressbook_repo/inventory.ini"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/Sathya252/milestone_practice1.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Install and Start Tomcat via Ansible') {
            steps {
                sh 'ansible-playbook -i ${INVENTORY} tomcat.yml'
            }
        }

        stage('Deploy WAR to Tomcat') {
            steps {
                sh """
                ansible webservers -i ${INVENTORY} -m copy -a "src=${WAR_FILE} dest=${TOMCAT_HOME}/webapps/addressbook.war" --become
                ansible webservers -i ${INVENTORY} -m shell -a "${TOMCAT_HOME}/bin/shutdown.sh || true" --become
                ansible webservers -i ${INVENTORY} -m shell -a "${TOMCAT_HOME}/bin/startup.sh" --become
                """
            }
        }
    }
}
