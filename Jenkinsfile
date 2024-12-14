pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/Alejandro1099/DevOps'  // URL de tu repositorio
        BRANCH_DESARROLLO = 'Desarrollo'
        BRANCH_PRUEBAS = 'Prueba'
        //agrego comentario
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Clona el repositorio y se asegura de estar en la rama de desarrollo
                    git url: "${env.GIT_REPO}", branch: "${env.BRANCH_DESARROLLO}"
                }
            }
        }

        stage('Integrar cambios en desarrollo') {
            steps {
                script {
                    // Realiza un pull de la rama de desarrollo, agrega los cambios locales y hace commit
                    sh '''
                        git checkout ${BRANCH_DESARROLLO}
                        git pull origin ${BRANCH_DESARROLLO}  // Asegura que la rama local esté actualizada
                        git add .
                        git commit -m "Integrando cambios locales a desarrollo"
                        git push origin ${BRANCH_DESARROLLO}  // Empuja los cambios al repositorio remoto
                    '''
                }
            }
        }

        stage('Fusionar cambios en pruebas') {
            steps {
                script {
                    // Fusiona los cambios de la rama desarrollo en la rama pruebas
                    sh '''
                        git checkout ${BRANCH_PRUEBAS}
                        git pull origin ${BRANCH_PRUEBAS}  // Actualiza la rama pruebas con los cambios remotos
                        git merge ${BRANCH_DESARROLLO}  // Fusiona los cambios de desarrollo
                        git push origin ${BRANCH_PRUEBAS}  // Empuja los cambios fusionados a la rama pruebas
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline ejecutado con éxito. Cambios fusionados y enviados a la rama de pruebas.'
        }
        failure {
            echo 'Hubo un error en el pipeline.'
        }
    }
}