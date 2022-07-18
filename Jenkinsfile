properties(
    [
        parameters([
            string(defaultValue: '/var/lib/jenkins/workspace/Pipeline_Job1_Frontend/DockerDirectory/Dockerfile', name: 'PATH_TO_DOCKERFILE', description: ''),
            string(defaultValue: 'frontend_img', name: 'IMAGE_NAME', description: ''),
            string(defaultValue: 'frontend_cont', name: 'CONTAINER_NAME', description: ''),
            choice(choices: ['3000', '3001', '3002'], name: 'PORT'),
            choice(choices: ['3000', '3001', '3002'], name: 'PORT_APP')
        ])
    ]
)
node{
    stage('Preparation'){
        
        sh 'docker stop ${CONTAINER_NAME}'
        
        sh 'docker rm ${CONTAINER_NAME}'
        
        sh 'a=1 && val=`expr $BUILD_NUMBER - $a` && docker image rm ${IMAGE_NAME}:${val}'
    }
    
    stage('GetSourceCode'){
        
        dir('source_code'){
            git url: 'https://github.com/aditya-sridhar/simple-reactjs-app.git', branch: 'master'
    
            sh 'rm -r .git'
        }
    }
    
    stage('Build'){
        dir('source_code'){
            sh 'docker build --tag ${IMAGE_NAME}:${BUILD_NUMBER} --build-arg PORT_NUMBER=${PORT_APP} -f ${PATH_TO_DOCKERFILE} .'
            
        }
    }
    
    stage('Deploy'){
        sh 'docker run --name ${CONTAINER_NAME} -d -p ${PORT}:${PORT_APP} ${IMAGE_NAME}:${BUILD_NUMBER}'
    }
}
