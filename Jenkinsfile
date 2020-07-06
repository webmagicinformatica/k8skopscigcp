pipeline { 
    agent any 
    
    stages {
        stage('Download SSH Key') { 
            steps { 
                sh 'gsutil cp $sshkey /root/.ssh/' 
            }
        }

        stage('Set GCP Project ID') { 
            steps { 
                sh 'echo $PROJECT' 
            }
        }

        stage('Enable KOPS for GCP') { 
            steps { 
                sh 'export KOPS_FEATURE_FLAGS=AlphaAllowGCE' 
            }
        }

        stage('Download Kops') { 
            steps { 
                sh './kopsdownload.sh' 
            }
        }

        stage('Download kubectl') { 
            steps { 
                sh './kubectldownload.sh' 
            }
        }
        
        stage('Set Kops config') { 
            steps { 
                sh 'kops create cluster simple.k8s.local --zones us-central1-a --state ${KOPS_STATE_STORE}/ --project=${PROJECT}' 
            }
        }

        stage('Apply Kops Config') { 
            steps { 
                sh 'kops update cluster simple.k8s.local --yes' 
            }
        }
    }
}