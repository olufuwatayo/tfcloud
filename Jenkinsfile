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
            phase1: { sh "echo p1; sleep 5s; echo phase1" }
        )
   stage name: 'init', concurrency: 1
        sh "terraform init"


   stage name: 'plan', concurrency: 1
               withAWS(credentials:'cb0729c3-175e-469b-9fe1-1fa8790a49ea') {
              sh "terraform plan --out plan"
             
            }
        

   stage name: 'deploy', concurrency: 1
        def deploy_validation = input(
            id: 'Deploy',
            message: 'Let\'s continue the deploy plan',
            type: "boolean")
               withAWS(region:'us-east-1', credentials:'cb0729c3-175e-469b-9fe1-1fa8790a49ea') {
               sh "terraform apply plan"
             
            }
       
}


