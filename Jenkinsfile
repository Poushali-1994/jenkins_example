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
           // The below will clone your repo and will be checked out to master branch by default.
           git credentialsId: 'Poushali-1994', url: 'https://github.com/Poushali-1994/jenkins_example.git'
           // Do a ls -lart to view all the files are cloned. It will be clonned. This is just for you to be sure about it.
           sh "ls -lart ./*" 
           // List all branches in your repo. 
           sh "git branch -a"
           // Checkout to a specific branch in your repo.
           sh "git checkout master"
          }
       }
    }
	stage('Build') {
		steps {
			script {
				echo "Building a maven project ....................."
				sh "mvn -f MavenProject/pom.xml clean install"	
			}
		}
	}
        stage('Archive') {
            steps {
				script{
				echo "Archiving ..................."
				archiveArtifacts 'MavenProject/multi3/target/*.war'
				}
			}
        }
		stage('clean up') {
            steps {
                echo "cleaning up the workspace"
                cleanWs()
            }
        }
	}
    post {
        always {
            archiveArtifacts artifacts: 'generatedFile.txt', onlyIfSuccessful: true
			emailext (
				subject: "Job '${env.JOB_NAME} ${env.BUILD_NUMBER}'",
				body: """<p>Successfully completed pipeline project with archiving the artifacts, check console output at <a href="${env.BUILD_URL}">${env.JOB_NAME}</a></p>""",
				to: "pbpoushalibanerjee7@gmail.com",
				from: "jenkins@code-maven.com"
			)
        }
	}    
}
