pipeline {
    agent any
    
    environment {
        // The MY_KUBECONFIG environment variable will be assigned
        // the value of a temporary file.  For example:
        //   /home/user/.jenkins/workspace/cred_test@tmp/secretFiles/546a5cf3-9b56-4165-a0fd-19e2afe6b31f/kubeconfig.txt
        KUBECONFIG = credentials('kubeconfig')
    }

    stages {
        stage('Git') {
            steps {
                sh 'rm -Rf helm-assignment'
                sh 'git clone https://github.com/neogopher/helm-assignment'
            }
        }
        
        stage('Helm') {
            steps {
                sh 'helm dependency update helm-assignment/newchart'
                sh 'helm lint helm-assignment/newchart'
		sh 'rm *.tgz'
                sh 'helm package helm-assignment/newchart'
                sh 'helm uninstall newchart && helm install newchart *.tgz'
            }
        }
    }
}
