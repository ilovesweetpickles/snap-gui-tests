#!/usr/bin/env groovy

/**
 * Copyright (C) 2019 CS-SI
 * This program is free software; you can redistribute it and/or modify it
 * under the terms of the GNU General Public License as published by the Free
 * Software Foundation; either version 3 of the License, or (at your option)
 * any later version.
 * This program is distributed in the hope that it will be useful, but WITHOUT
 * ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
 * FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for
 * more details.
 *
 * You should have received a copy of the GNU General Public License along
 * with this program; if not, see http://www.gnu.org/licenses/
 */

pipeline {
    agent any
    parameters {
        string(name: 'dockerTagName', defaultValue: '', description: 'Snap version to use to launch tests')
    }
    stages {
        stage('GUI Tests') {
            agent {
                docker {
                    docker.image('snap-build-server.tilaa.cloud/xvfb:1.0').withRun() { c ->
                        docker.image("${params.dockerTagName}").inside("--link ${c.id} -e DISPLAY=${c.id}:0") {
                        }
                    }
                }
            }
            steps {
                echo "Launch GUI Tests from ${env.JOB_NAME} from ${env.GIT_BRANCH} with docker image ${params.dockerTagName}"
                // sh 'mvn -Duser.home=/var/maven -Dsnap.userdir=/home/snap clean package install -U -Dsnap.reader.tests.data.dir=/data/ssd/s2tbx/ -Dsnap.reader.tests.execute=false -DskipTests=false'
            }
        }
    }
    /*post {
        failure {
            step (
                emailext(
                    subject: "[SNAP] JENKINS-NOTIFICATION: ${currentBuild.result ?: 'SUCCESS'} : Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                    body: """Build status : ${currentBuild.result ?: 'SUCCESS'}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':
Check console output at ${env.BUILD_URL}
${env.JOB_NAME} [${env.BUILD_NUMBER}]""",
                    attachLog: true,
                    compressLog: true,
                    recipientProviders: [[$class: 'CulpritsRecipientProvider'], [$class:'DevelopersRecipientProvider']]
                )
            )
        }
    }*/
}
