node {
    stage('Checkout') {
        checkout scm
    }
    stage('Debug') {
        docker.image('python:2-alpine').inside {
            sh 'ls -R'
        }
    }
    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    stage('Test') {
        docker.image('qnib/pytest').inside {
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        }
        junit 'test-reports/results.xml'
    }
    stage('Manual Approval') {
        input message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed'
    }
    stage('Deploy') {
        docker.image('python:3.9-slim').inside {
            def x = input message: 'Masukkan nilai X:', parameters: [string(defaultValue: '5', description: 'Nilai X', name: 'x')]
            def y = input message: 'Masukkan nilai Y:', parameters: [string(defaultValue: '10', description: 'Nilai Y', name: 'y')]
            sh "python sources/add2vals.py ${x} ${y}"
            echo 'Aplikasi berjalan selama 1 menit...'
            sleep 60
            echo 'Pipeline selesai setelah jeda 1 menit.'
        }
    }
}
