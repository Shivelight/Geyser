pipeline {
    agent any
    tools {
        gradle 'Gradle 7'
        jdk 'Java 17'
    }
    options {
        buildDiscarder(logRotator(artifactNumToKeepStr: '20'))
    }
    stages {
        stage ('Build') {
            steps {
                sh 'git submodule update --init --recursive'
                rtGradleRun(
                    usesPlugin: true,
                    tool: 'Gradle 7',
                    buildFile: 'build.gradle.kts',
                    tasks: 'clean build',
                )
            }
            post {
                success {
                    archiveArtifacts artifacts: 'bootstrap/**/build/libs/Geyser-*.jar', fingerprint: true
                }
            }
        }
    }

    post {
        success {
            script {
                if (env.BRANCH_NAME == 'master') {
                    build propagate: false, wait: false, job: 'GeyserMC/GeyserConnect/master', parameters: [booleanParam(name: 'SKIP_DISCORD', value: true)]
                }
            }
        }
    }
}
