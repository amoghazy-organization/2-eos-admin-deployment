  def label = "agent"
  def env = "qa"
  pipeline {
    agent {
        kubernetes {
            label 'agent'
            defaultContainer 'build'
        }
    } {
      stages  {

          stage ('Checkout SCM'){
            git credentialsId: 'git', url: 'https://github.com/amoghazy-organization/2-eos-admin-deployment.git', branch:  "${env}"
          }

          stage ('Helm Chart') {
            container('build') {
                withCredentials([usernamePassword(credentialsId: 'jfrog', usernameVariable: 'username', passwordVariable: 'password')]) {
                      sh '/usr/local/bin/helm repo add eos-helm-local  https://triallekevd.jfrog.io/artifactory/ecom-helm-local --username $username --password $password'
                      sh "/usr/local/bin/helm repo update"
                      sh "/usr/local/bin/helm upgrade  --install --force micro-services-admin  --namespace ${env} -f values.yaml eos-helm-local/micro-services-admin"
                      sh "/usr/local/bin/helm list -a --namespace ${env}"
                      sh "rm -rf values.yaml"
              }
          }
          }
      }
  }}