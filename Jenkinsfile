pipeline{
    agent any 
    
    environment {
        REMOTE_HOST = "ec2-user@3.82.101.199"
        PEM_PATH = "/.ssh-keys/docker-key.pem"
        REPO_URL = "https://github.com/Ulekz/multijuegos"
        REPOS_DIR = "/home/ec2-user/github-repos"
        MULTIJUEGOS_DIR = "/home/ec2-user/github-repos/multijuegos"
    }
    
    stages{
        stage('limpieza') {
            steps {
                sh """
                sudo ssh -i $PEM_PATH $REMOTE_HOST << EOF
                    echo "Paso 1: Limpieza"
                    sudo rm -rf $MULTIJUEGOS_DIR
                    sudo mkdir -p $REPOS_DIR
                    exit
                EOF
                """
            }
        }
        
        stage('Clonado') {
            steps {
                sh """
                sudo ssh -i $PEM_PATH $REMOTE_HOST << EOF
                    echo "Paso 2: Clonado de Repo"
                    cd $REPOS_DIR
                    sudo git clone $REPO_URL
                    exit
                EOF
                """
            }
        }
        
        stage('Build') {
            steps {
                sh """
                sudo ssh -i $PEM_PATH $REMOTE_HOST << EOF
                    echo "Paso 3: Creación de imagen"
                    cd $MULTIJUEGOS_DIR
                    sudo docker build -t multi-image .
                    exit
                EOF
                """
            }
        }
        
        stage('Ejecución') {
            steps {
                sh """
                sudo ssh -i $PEM_PATH $REMOTE_HOST << EOF
                    echo "Paso 4: Ejecución de imagen... correr contenedor"
                    sudo docker rm -f multi-1 || true
                    sudo docker run --name multi-1 -d -p 8090:8080 multi-image
                    exit
                EOF
                """
            }
        }
        
        
    }
}
