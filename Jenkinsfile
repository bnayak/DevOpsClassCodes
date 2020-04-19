pipeline{
    tools{
        jdk 'myjava'
        maven 'mymaven'
    }
    agent none
    stages{
        stage('Checkout'){
            agent any
            steps{
                git 'https://github.com/bnayak/DevOpsClassCodes.git'
            }
        }
        stage('Compile'){
            agent {label 'win_slave'}
            steps{
                git 'https://github.com/bnayak/DevOpsClassCodes.git'
                bat 'mvn compile'
            }
        }
        stage('CodeReview'){
            agent any
            steps{
                sh 'mvn pmd:pmd'
            }
            post{
                always{
                    pmd pattern: 'target/pmd.xml'
                }
            }
        }
        stage('UnitTest'){
            agent any
            steps{
                sh 'mvn test'
            }
            post{
                always{
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('MetricCheck'){
            agent any
            steps{
                sh 'mvn cobertura:cobertura -Dcobertura.report.format=xml'
            }
            post{
                always{
                    cobertura coberturaReportFile: 'target/site/cobertura/coverage.xml'
                }
            }
        }
        stage('Package'){
            agent {label 'win_slave'}
            steps{
                git 'https://github.com/bnayak/DevOpsClassCodes.git'
                bat 'mvn package'
            }
        }
    }
}
