pipeline {
    agent any

    stages {
        stage('Start Grid') {
            steps {
                bat '''
                    echo === Checking Docker version ===
                    docker --version
                    docker-compose --version

                    echo === Showing current directory ===
                    cd
                    dir

                    echo === Showing grid.yaml ===
                    type grid.yaml

                    echo === Starting Selenium Grid ===
                    docker-compose -f grid.yaml up -d || exit /b 1
                '''
            }
        }

        stage('Run Test') {
            steps {
                bat '''
                    echo === Running Tests ===
                    docker-compose -f test-suites.yaml up --pull=always || exit /b 1
                '''
            }
        }
    }

    post {
        always {
            bat "docker-compose -f grid.yaml down || echo Grid already down"
            bat "docker-compose -f test-suites.yaml down || echo Test container already down"

            archiveArtifacts artifacts: 'output/flight-reservation/emailable-report.html', followSymlinks: false
            archiveArtifacts artifacts: 'output/vendor-portal/emailable-report.html', followSymlinks: false
        }
    }
}
