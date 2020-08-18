// pipeline {
//     agent { docker { image 'maven:3.3.3' } }
//     stages {
//         stage('build') {
//             steps {
//                 sh 'mvn --version'
//             }
//         }
//     }
// }


node {
   stage 'checkout'
        checkout scm

   stage 'test'
        parallel (
            phase1: { sh "echo p1; sleep 20s; echo phase1" },
            phase2: { sh "echo p2; sleep 40s; echo phase2" }
        )
   stage name: 'init', concurrency: 1
        sh "terraform init"

   stage name: 'plan', concurrency: 1
        sh "terraform plan --out plan"

   stage name: 'deploy', concurrency: 1
        def deploy_validation = input(
            id: 'Deploy',
            message: 'Let\'s continue the deploy plan',
            type: "boolean")

        sh "terraform apply plan"
}