// ─── 1. IMPORTAR LA SHARED LIBRARY ───────────────────────────────────────────
// "@Library('nombre-registrado-en-jenkins')" importa la library
// El '_' al final activa las variables globales (vars/)
@Library('my-shared-library') _

// ─── 2. PIPELINE DECLARATIVO ─────────────────────────────────────────────────
pipeline {

    // Agente: contenedor Docker con Maven + Java 17
    agent {
        docker {
            image 'maven:3.9.4-eclipse-temurin-17'
            // Montar caché de Maven para acelerar builds
            args '-v $HOME/.m2:/root/.m2'
        }
    }

    // Variables de entorno del pipeline
    environment {
        APP_NAME  = 'app-java-simple'
        APP_VERSION = '1.0'
    }

    stages {

        stage('Checkout') {
            steps {
                echo "📥 Clonando repositorio: ${env.APP_NAME}"
                checkout scm
            }
        }

        stage('Compilar') {
            steps {
                // ✅ Llamada a la función de la Shared Library
                buildJava()
            }
        }

    }

    // ─── POST: SE EJECUTA SIEMPRE AL FINAL ───────────────────────────────────
    post {
        success {
            // ✅ Llamada a la función de notificación de la Shared Library
            notifyBuild('SUCCESS')
        }
        failure {
            notifyBuild('FAILURE')
        }
    }
}