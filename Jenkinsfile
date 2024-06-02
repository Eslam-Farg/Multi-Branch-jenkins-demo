

@Library('-jenkins-shared-library')

def gv

pipeline {
    agent any
    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage("build jar") {
            
                when {
                    expression {
                        BRANCH_NAME == 'main'
                    }
                }
            steps {
                script {
                    echo "building jar"
                    buildJar()
                }
            }
        }
        stage("build image") {
                when {
                    expression {
                        BRANCH_NAME == 'main'
                    }
                }
            steps {
                script {
                    echo "building image"
                    buildImage ("eslam1/jenkins-repo:jma-2.0")
                }
            }
        }
        stage("deploy") {
                            when {
                    expression {
                        BRANCH_NAME == 'main'
                    }
                }
            steps {
                script {
                    echo "deploying"
                    gv.deployApp()
                }
            }
        }
    }   
}
