def imageName = 'mlabouardy/movies-loader'
def registry = 'https://registry.slowcoder.com'

node('windows-agent-1'){
    stage('Checkout'){
        checkout scm
    }

    stage('Unit Tests'){
		echo imageName
        def imageTest= docker.build("${imageName}-test", "-f Dockerfile.test .")
        //bat "docker run --rm -v %cd%/reports:/app/reports ${imageName}-test"
        //junit "%cd%/reports/*.xml"
    }

    stage('Build'){
        docker.build(imageName)
    }

    stage('Push'){
        docker.withRegistry(registry, 'registry') {
            docker.image(imageName).push(commitID())

            if (env.BRANCH_NAME == 'develop') {
                docker.image(imageName).push('develop')
            }
        }
    }
}

def commitID() {
    sh 'git rev-parse HEAD > .git/commitID'
    def commitID = readFile('.git/commitID').trim()
    sh 'rm .git/commitID'
    commitID
}