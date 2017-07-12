pipeline {
  agent any

  environment {
    MAJOR_VERSION = 1
  }

  stages {
    stage('build') {
      steps {
        sh 'javac -d . src/*.java'
        sh 'echo Main-Class: Rectangulator > MANIFEST.MF'
        sh 'jar -cvmf MANIFEST.MF rectangle.jar *.class'
      }
      post {
        success {
          archiveArtifacts artifacts: 'rectangle.jar', fingerprint: true
        }
      }
    }
    stage('run') {
      steps {
        sh 'java -jar rectangle.jar 7 9'
      }
    }
    stage('Promote development to master') {
      when {
        branch 'development'
      }
      steps {
        echo "stashing local chnages"
        sh "git stash"
        echo "git checkout development"
        sh "git checkout development"
        echo "git checkout master"
        sh "git checkout master"
        echo "merging development to master"
        sh "git merge development"
        echo "git push toorigin"
        sh "git push origin master"
      }
    }
  }
}
