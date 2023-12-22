pipeline {
    agent any
    
    tools {
        maven "maven"
    }
    parameters {
         string(name: 'tomcat_stag', defaultValue: '3.86.165.49', description: 'Node1-Remote Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.234.204.107', description: 'Node2-Remote Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving the artifacts'
                    archiveArtifacts artifacts: '/target/.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp */.war jenkins@${params.tomcat_stag}:/usr/share/apache-tomcat-9.0.84/webapps/"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp */.war jenkins@${params.tomcat_prod}:/usr/share/apache-tomcat-9.0.84/webapps"
                    }
                }
            }
        }
    }
}
