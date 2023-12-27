pipeline {
    agent any
    
    tools {
        maven 'maven3'
    }
    parameters {
         string(name: 'tomcat_stag', defaultValue: '16.171.115.13', description: 'Node1-Remote Staging Server')
         string(name: 'tomcat_prod', defaultValue: '51.20.96.29', description: 'Node2-Remote Production Server')
    }

    triggers {
         pollSCM('* * * * *')
    }
           stages{
               stage('Build'){
                   steps {
                      sh 'mvn package'
                   }
               post {
                success {
                    echo 'Archiving the artifacts'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
              }
           }
                stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp **/*.war jenkins@${params.tomcat_stag}:/usr/share/tomcat/webapps/"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp **/*.war jenkins@${params.tomcat_prod}:/usr/share/tomcat/webapps/"
                    }
                }
            }
        }
    }
}
	
