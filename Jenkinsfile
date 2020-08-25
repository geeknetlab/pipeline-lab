pipeline {
  agent none
  stages {
    stage('Fluffy Build') {
      agent {
        node {
          label 'java8'
        }

      }
      steps {
        sh './jenkins/build.sh'
        archiveArtifacts 'target/*.jar'
        stash(name: 'Java 8', includes: 'target/**')
      }
    }

    stage('Fluffy Test') {
      parallel {
        stage('Backend') {
          agent {
            node {
              label 'java8'
            }

          }
          steps {
            unstash 'Java 8'
            sh './jenkins/test-backend.sh'
            junit 'target/surefire-reports/**/TEST*.xml'
          }
        }

        stage('Frontend') {
          agent {
            node {
              label 'java8'
            }

          }
          steps {
            unstash 'Java 8'
            sh './jenkins/test-frontend.sh'
            junit 'target/test-results/**/TEST*.xml'
          }
        }

        stage('Performance') {
          agent {
            node {
              label 'java8'
            }

          }
          steps {
            unstash 'Java 8'
            sh './jenkins/test-performance.sh'
          }
        }

        stage('Static') {
          steps {
            unstash 'Java 8'
            sh './jenkins/test-static.sh'
          }
        }

      }
    }

    stage('Fluffy Deploy') {
      agent {
        node {
          label 'java8'
        }

      }
      steps {
        sh './jenkins/deploy.sh staging'
      }
    }

  }
}