pipeline{
    agent any

    stages {
        stage('Compilar') {
            steps {
                echo 'Compilando el codigo de SpringBoot'
                sh './mvnw compile'
            }
        }
        stage('test') {
            steps {
                echo "Estoy probando el codigo de Sprint Boot"
                sh './mvnw test'
            }
        }
        stage('Generar JAR') {
            steps {
                echo "Estoy generando el JAR de Sprint Boot"
                sh './mvnw package'
            }
            post {
                failure {
                    sh "rm -rf ./target"
                }
            }
        }
        stage('deploy') {
            steps {
                echo "Estoy desplegando "
                sh 'ansible all -i maquinas -m copy -a "src=target/calculadora-0.0.1-SNAPSHOT.jar dest=/tmp/calculadora-0.0.1-SNAPSHOT.jar"'
                sh 'ansible all -i maquinas -m ansible.builtin.shell -a "nohup java -jar /tmp/calculadora-0.0.1-SNAPSHOT.jar"'
    
            }
        }
    }
    post {
        success {
            echo "Todo ha funcionado correctamente"
            sh "echo ${CHANGE_AUTHOR} > /tmp/f1.txt"
        }
    }
}


