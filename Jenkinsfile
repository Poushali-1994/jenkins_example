pipeline {
   agent { label 'window' }
   tools{
		maven "MyMaven"
		jdk "jdk"
   }
   stages {
    stage('Clone and Check out') {
      steps {
        script {
			echo "Git clone and check step"
           git credentialsId: 'Poushali-1994', url: 'https://github.com/Poushali-1994/jenkins_example.git'
           sh "ls -lart ./*" 
           sh "git branch -a"
           sh "git checkout master"
          }
       }
    }
	stage('Build') {
		steps {
			script {
				echo "Building a maven project ....................."
				sh "mvn -f MavenProject/pom.xml clean verify"	
			}
		}
	}
    
    stage('Archieve Artifacts'){
        steps {
            echo "Archiving the artifacts ............."
            archiveArtifacts 'MavenProject/multi3/target/*.war'
        }  
    }
        
	stage('Deployment') {
            // Deployment
            steps {
                script {
                    echo "Deployment ............."
                    sh 'cp MavenProject/multi3/target/*.war /var/lib/tomcat9/webapps/'
					// sh "mvn tomcat9:deploy"
                }
            }
        }
    
    stage('publish html report') {
            steps{
                echo "publishing the html report .................."
                publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])
            }
        }
    
    stage('clean up') {
        steps {
            echo "cleaning up the workspace............"
            cleanWs()
        }
    }
    
   stage("Metrics"){
            steps{
            parallel ( "JavaNcss Report":   
            {
              node('window'){
                git 'https://github.com/Poushali-1994/jenkins_example.git'
                sh "cd javancss-master ; mvn test javancss:report ; pwd"
                  }
            },
            "FindBugs Report" : {
            node('window'){
                sh "mkdir javancss1 ; cd javancss1 ;pwd"
                git 'https://github.com/Poushali-1994/jenkins_example.git'
                sh "cd javancss-master ; mvn findbugs:findbugs ; pwd"
                deleteDir()
                }

              }
         )
            }
   }
        
// stages complete
   } 
   
post {
        always {
            echo 'I will always say Hello again!'
            
            emailext attachLog: true,
				body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
				// recipientProviders: [developers(), requestor()],
				to: env.EMAIL_RECIPIENT,
				subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
        }
	}
// pipeline complete  
}
