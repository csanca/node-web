pipeline {
    /* insert Declarative Pipeline here */
    agent any

    environment {
      regCredenciales= 'carmensancab/node'
      imagenDocker = "$regCredenciales:v.$BUILD_NUMBER"
    }
    // Fases que va a realizar
    stages {
     stage ('build'){
        steps {
          echo "Construyendo la imagen docker..."
           sh "docker build -t nodeweb ." 
        }
      }

      stage ('run'){
        steps {
          echo "Ejecutar imagen docker"
          sh "docker run -d -p 11631:11631 nodeweb"
            }
      }
      stage ('Test'){
        steps {
          echo "Haciendo test sencillos de que todo funciona bien"
          echo "Node Version"
          sh "npm -v" // Ver la version de nodejs, eso nos indicará que está arrancado
          echo "Puerto por el que estás ejecutando"
         
         
        }
      }

      stage ('Push'){
        steps {
          echo "Hacer push a DockerHub"
          script {
            withCredentials([usernamePassword(credentialsId: 'Dockerhub',usernameVariable: 'password', passwordVariable: 'username')]){
              sh "docker login -u $username -p $password"
              sh "docker push $imagenDocker"
              sh "docker logout"
            }
         }
        }
     
      }
  }
  post { 
        always { 
            echo 'Borramos la imagen Docker para no saturar'
           
        }
    }
}