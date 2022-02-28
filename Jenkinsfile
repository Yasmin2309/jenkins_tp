pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh 'gradle build'
        sh 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
        archiveArtifacts 'build/docs/**'
        archiveArtifacts 'build/reports/**'
      }
    }

    stage('Mail Notification') {
      steps {
        mail(subject: 'TP Jenkins Fin de build', body: 'Build Done', cc: 'kn_larbaoui@esi.dz', from: 'kn_larbaoui@esi.dz')
        mail(subject: 'TP Jenkins Fin de build', body: 'Build done', cc: 'kn_larbaoui@esi.dz ', from: 'kn_larbaoui@esi.dz')
      }
    }

    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          post {
            success {
              mail(subject: 'Analyse de Code', body: 'Fin de l\'analyse de code avec succes', cc: 'kn_larbaoui@esi.dz', from: 'kn_larbaoui@esi.dz')
              mail(subject: 'Analyse de Code', body: 'Fin de l\'analyse de code avec succes', cc: 'kn_larbaoui@esi.dz ', from: 'kn_larbaoui@esi.dz')
            }

            failure {
              mail(subject: 'Analyse de Code', body: 'Fin de l\'analyse de code avec un statu d\'echec', cc: 'kn_larbaoui@esi.dz', from: 'kn_larbaoui@esi.dz')
              mail(subject: 'Analyse de Code', body: 'Fin de l\'analyse de code avec un statu d\' echec', cc: 'kn_larbaoui@esi.dz', from: 'kn_larbaoui@esi.dz')
            }

          }
          steps {
            sh 'gradle sonarqube'
          }
        }

        stage('Test Reporting') {
          steps {
            sh 'gradle cucumberCli'
          }
        }

      }
    }

    stage('Deployment') {
      steps {
        sh 'gradle publish'
      }
    }

    stage('Slack Notification') {
      steps {
        slackSend(baseUrl: 'https://hooks.slack.com/services/', token: 'T02SMBGA6BE/B0355KTRSF2/znMvBg1tkemZQzs6m2ERCjGM', message: 'Une nouvelle version est disponible')
      }
    }

  }
}