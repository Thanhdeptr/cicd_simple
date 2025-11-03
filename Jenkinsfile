pipeline {
    // 'agent' là nơi pipeline sẽ chạy.
    // Sử dụng 'any' để chạy trên Jenkins node có sẵn (cần cài Python 3)
    agent any

    stages {
        // Một pipeline có nhiều 'stage' (giai đoạn)
        stage('Checkout') {
            steps {
                // Tự động kéo code từ Git về
                checkout scm
                
                // Đảm bảo lấy code mới nhất từ remote
                sh '''
                    git fetch origin
                    git reset --hard origin/master || git reset --hard origin/main
                '''
                
                // Debug: Hiển thị commit hash và nội dung file để xác nhận code đúng
                sh '''
                    echo "=== Git Information ==="
                    echo "Current commit:"
                    git rev-parse HEAD
                    git log -1 --oneline
                    echo ""
                    echo "=== app.py content ==="
                    cat app.py
                    echo ""
                '''
            }
        }

        stage('Run Tests') {
            steps {
                // Chạy file test_app.py với verbose output
                // Test fail sẽ tự động làm build fail (exit code != 0)
                sh '''
                    python3 -m unittest test_app.py -v || python -m unittest test_app.py -v
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