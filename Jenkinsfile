pipeline {
    // 'agent' là nơi pipeline sẽ chạy. 
    // Ở đây, ta bảo Jenkins chạy trong một Docker container có cài sẵn Python 3.9
    agent {
        docker { image 'python:3.9-slim' } 
    }

    stages {
        // Một pipeline có nhiều 'stage' (giai đoạn)
        stage('Checkout') {
            steps {
                // Tự động kéo code từ Git về
                checkout scm
            }
        }

        stage('Run Tests') {
            steps {
                // Chạy file test_app.py với verbose output
                // Test fail sẽ tự động làm build fail (exit code != 0)
                sh '''
                    python -m unittest test_app.py -v
                '''
            }
            post {
                always {
                    // Luôn chạy sau khi stage kết thúc (dù pass hay fail)
                    echo 'Test stage completed'
                }
                success {
                    echo 'All tests passed!'
                }
                failure {
                    echo 'Tests failed! Build will be marked as failed.'
                }
            }
        }
    }
}