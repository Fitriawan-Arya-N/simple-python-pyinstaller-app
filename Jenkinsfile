node {
    stage('Build') {
        docker.image('python:2-alpine').inside {
            try {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            } catch (Exception e) {
                echo "Failed to build: ${e.message}"
                currentBuild.result = 'FAILURE'
            }
        }
    }
    stage('Test') {
        docker.image('qnib/pytest').inside {
            try {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            } catch (Exception e) {
                echo "Failed to run tests: ${e.message}"
                currentBuild.result = 'FAILURE'
            }
        }
        post {
            always {
                junit 'test-reports/results.xml'
            }
        }
    }
}
