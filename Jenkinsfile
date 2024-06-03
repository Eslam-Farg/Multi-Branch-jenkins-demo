            

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

stage("increment version'") {
            steps {
                script {
                    echo 'incrementing app version ... '
                    sh 'mvn build-helper:parse-version versions:set -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.newIncrementedVersion} versions:commit'                                                             

def matcher = readFile('pom.xml') =~  '<version>(.+)</version>'
def version = matcher [0] [1]
env.IMAGE_NAME = "eslam1/jenkins-repo:$version-$BUILD_NUMBER"

                }
            }
        }

            stage("commit version update") {
            steps {
                script {
            sh 'git config --global user.name "Jenkins"'
            sh 'git config --global user.email "loom2141@gmail.com"'
                    sh 'git status'
                    
                    sh 'git rm --cached Multi-Branch-jenkins-demo'
                    
                    sh 'git add .'
                    sh 'git commit -m "ci: version bump"'
                    sh 'git push origin HEAD:main'

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
                    buildImage ("${IMAGE_NAME}")
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
