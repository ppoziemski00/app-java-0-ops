pipeline {
    agent any
    tools {
        // Użyj wersji JDK o nazwie "Java 21" skonfigurowanej w Jenkins
        jdk 'Java'
    }

    environment {
        // Definiowanie JAVA_HOME może być opcjonalne, zależnie od konfiguracji Jenkinsa
        JAVA_HOME = tool 'Java'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/por314ug/app-java-1.git', branch: 'main'
            }
        }

        stage('Compile') {
            steps {
                // Tworzenie katalogu dla skompilowanych klas
                bat 'if not exist build\\classes mkdir build\\classes'
                // Kompilacja Main.java
                bat '"%JAVA_HOME%\\bin\\javac" -d build\\classes Main.java'
            }
        }

        stage('Prepare Manifest') {
            steps {
                // Umieszcza plik manifestu bezpośrednio w katalogu build\classes
                bat 'echo Main-Class: Main > build\\classes\\MANIFEST.MF'
            }
        }

        stage('Package') {
            steps {
                // Pakowanie skompilowanych klas do pliku JAR z plikiem manifestu
                bat 'if not exist build\\jar mkdir build\\jar'
                // Używa bezpośredniej ścieżki do pliku manifestu znajdującego się w build\classes
                bat 'cd build\\classes && "%JAVA_HOME%\\bin\\jar" cvmf MANIFEST.MF ..\\jar\\MyApplication.jar *'
            }
        }

        stage('Run') {
            steps {
                // Uruchamianie aplikacji z pliku JAR
                bat '"%JAVA_HOME%\\bin\\java" -jar build\\jar\\MyApplication.jar'
            }
        }
    }
}
