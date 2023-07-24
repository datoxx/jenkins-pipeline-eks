#!/usr/bin/env groovy

@Library('jenkins-shared-librar')
def gv

pipeline {
    agent any

    tools {
        maven 'Maven'
    }
    environment {
        DOCKER_REPO_SERVER = '611244457668.dkr.ecr.eu-north-1.amazonaws.com'
        DOCKER_REPO = "${DOCKER_REPO_SERVER}/my-apps"
    }


    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage("incremet version") {
            steps {
                script {
                    gv.incremenVersion()
                }
            }
        }
        stage("build jar") {
            steps {
                script {
                    gv.buildJar()
                }
            }
        }
        stage("build image") {
            steps {
                script {
                  gv.buildImage()
                }
            }
        }
        stage("deploy") {
            environment {
                AWS_ACCESS_KEY_ID = credentials('jenkins_aws_access_key_id')
                AWS_SECRET_ACCESS_KEY = credentials('jenkins_aws_secret_access_key')
                APP_NAME = 'java-maven-app'
            }
            steps {
                script {
                    gv.deployEks()
                }
            }
        }
        stage("commit version update") {
           
            steps {    
                script {
                   gv.commitGit()
                }
            }
        }
    }   
}
