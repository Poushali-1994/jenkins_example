pipeline {
    agent any 
    stages {
        stage('Clone') {
            steps {
                echo "checking out the repo"
                git 'https://github.com/Poushali-1994/jenkins_example.git'
            
            }
        }
        stage('build'){
            steps {
                echo "building the project"
                sh "cd MavenProject ; mvn clean install ; pwd"
            }
        }
        
        stage('Archieve Artifacts'){
            steps {
                echo "archiving the artifacts"
                archiveArtifacts 'MavenProject/multi3/target/*.war'
            }
            
        }
        
        stage('clean up') {
            steps {
                echo "cleaning up the workspace"
                cleanWs()
            }
        }
        stage("Metrics"){
            steps{
            parallel ( "JavaNcss Report":   
            {
                git 'https://github.com/Poushali-1994/jenkins_example.git'
                sh "cd javancss-master ; mvn test javancss:report ; pwd"
                  
            },
            "FindBugs Report" : {
                sh "mkdir javancss1 ; cd javancss1 ;pwd"
                git 'https://github.com/Poushali-1994/jenkins_example.git'
                sh "cd javancss-master ; mvn findbugs:findbugs ; pwd"
                deleteDir()

              }
         )
            }
         post{
                success {
                    emailext body: 'Successfully completed pipeline project with archiving the artifacts', subject: 'Pipeline was successfull', to: 'pbpoushalibanerjee7@gmail.com'
                }
    }
}
        
    }
}
