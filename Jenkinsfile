node {

    try {

        stage('Build') {
            sh '''
            echo "=== BUILD STAGE ==="
            cd "Password Protection"

            rm -rf build
            mkdir -p build

            javac -d build src/*.java

            echo "Build successful"
            '''
        }

        stage('Test') {
            sh '''
            echo "=== TEST STAGE ==="
            cd "Password Protection"

            echo "Downloading fresh JUnit..."
            rm -f junit-platform-console-standalone.jar
            curl -L -o junit-platform-console-standalone.jar https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.10.0/junit-platform-console-standalone-1.10.0.jar

            if [ -d test ] && ls test/*.java 1> /dev/null 2>&1; then
                echo "Tests found — compiling..."
                rm -rf test-build
                mkdir -p test-build

                javac -cp junit-platform-console-standalone.jar:build -d test-build test/*.java

                echo "Running tests..."
                java -jar junit-platform-console-standalone.jar --class-path build:test-build --scan-class-path
            else
                echo "No test files found — skipping tests"
            fi

            echo "Test stage completed"
            '''
        }

        stage('Deploy') {
            sh '''
            echo "=== DEPLOY STAGE ==="
            cd "Password Protection"

            jar cf FileEncrypter.jar -C build .

            echo "Deployment successful"
            '''
        }

        echo "Pipeline executed successfully!"

    } catch (Exception e) {
        echo "Pipeline failed!"
        throw e
    }
}
