pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                sh '''
                echo "=== BUILD STAGE ==="
                cd "Password Protection"

                echo "Cleaning previous build..."
                rm -rf build
                mkdir -p build

                echo "Compiling Java sources..."
                javac -d build src/*.java

                echo "Build successful"
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                echo "=== TEST STAGE ==="
                cd "Password Protection"

                echo "Removing old JUnit (if any)..."
                rm -f junit-platform-console-standalone.jar

                echo "Downloading JUnit platform..."
                curl -L -o junit-platform-console-standalone.jar https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.10.0/junit-platform-console-standalone-1.10.0.jar

                echo "Preparing test build..."
                rm -rf test-build
                mkdir -p test-build

                echo "Compiling test classes..."
                javac -cp junit-platform-console-standalone.jar:build -d test-build test/*.java

                echo "Running JUnit tests..."
                java -jar junit-platform-console-standalone.jar --class-path build:test-build --scan-class-path

                echo "JUnit tests executed successfully"
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                echo "=== DEPLOY STAGE ==="
                cd "Password Protection"

                echo "Packaging JAR artifact..."
                jar cf FileEncrypter.jar -C build .

                echo "Deployment successful — JAR created"
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
