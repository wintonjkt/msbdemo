podTemplate(label: 'mypod',
    volumes: [
        hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock'),
        secretVolume(secretName: 'registry-account', mountPath: '/var/run/secrets/registry-account'),
        configMapVolume(configMapName: 'registry-config', mountPath: '/var/run/configs/registry-config')
    ],
    containers: [
        containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl', ttyEnabled: true, command: 'cat'),
        containerTemplate(name: 'docker' , image: 'docker:17.06.1-ce', ttyEnabled: true, command: 'cat')
  ]) {

    node('mypod') {
        checkout scm
        container('docker') {
            stage('Build Docker Image') {
                sh """
                #!/bin/bash
                #NAMESPACE=wintondev
                #REGISTRY=mycluster.icp:8500
		kubectl config set-cluster mycluster.icp --server=https://192.168.64.220:8001 --insecure-skip-tls-verify=true
		kubectl config set-context mycluster.icp-context --cluster=mycluster.icp
		kubectl config set-credentials mycluster.icp-user --	token=eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImF0X2hhc2giOiJIU3J1VHJoTnRGUkRjNldyUk02aTdRIiwiaXNzIjoiaHR0cHM6Ly9teWNsdXN0ZXIuaWNwOjk0NDMvb2lkYy9lbmRwb2ludC9PUCIsImF1ZCI6ImNiNzQ4YWIyN2UxMDgzMjdjMmVjMGJmOTNiYzZjZjQ4IiwiZXhwIjoxNTEwMjYwNzQwLCJpYXQiOjE1MTAyMTc1NDB9.lFDmEA-1VzcGGh-KsVKaGA-zAWGkcVuOHDJiUR5Oq3yVnkPPqAfx3wgUnBlXD6nCWPq2xn6RgwgQHRgkTOIcbyAELKKjZnXsp1pm5lRvoguKQw9Segq8xlkmeU-JS49EbI6OlGG2amQPzjNe86N4xForWI6C7QtE5it6Glujl8lDdN4Rbo03n1UiwQ3qJ1BB6_2nYD3UtrXMvqnr9XCumYtmN1MupOC70tWFONI0ojQS43WL7wCab780HxVSGmCapsoOX25mbzV3wlI6JERJ6iiMp1-ups2IQ7Fe2zd_bXED_beQMh-ry1IVY18jSrBI5x4v8eXomphF2Pv5ArDkjw
		kubectl config set-context mycluster.icp-context --user=mycluster.icp-user --namespace=default
		kubectl config use-context mycluster.icp-context
		cd containers/compute-interest-api
		mvn package
                docker build -t mycluster.icp:8500/wintondev/compute-interest-api:${env.BUILD_NUMBER} .
                """
            }
            stage('Push Docker Image to Registry') {
                sh """
                #!/bin/bash
                NAMESPACE=`cat /var/run/configs/registry-config/namespace`
                REGISTRY=`cat /var/run/configs/registry-config/registry`
                set +x
                DOCKER_USER=`cat /var/run/secrets/registry-account/username`
                DOCKER_PASSWORD=`cat /var/run/secrets/registry-account/password`
                docker login -u=admin -p=admin mycluster.icp
                set -x
                docker push mycluster.icp:8500/wintondev/compute-interest-api:${env.BUILD_NUMBER}
                """
            }
        }
        container('kubectl') {
            stage('Deploy new Docker Image') {
                sh """
                #!/bin/bash
                set +e

		kubectl config set-cluster mycluster.icp --server=https://192.168.64.220:8001 --insecure-skip-tls-verify=true
		kubectl config set-context mycluster.icp-context --cluster=mycluster.icp
		kubectl config set-credentials mycluster.icp-user --token=eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImF0X2hhc2giOiJIU3J1VHJoTnRGUkRjNldyUk02aTdRIiwiaXNzIjoiaHR0cHM6Ly9teWNsdXN0ZXIuaWNwOjk0NDMvb2lkYy9lbmRwb2ludC9PUCIsImF1ZCI6ImNiNzQ4YWIyN2UxMDgzMjdjMmVjMGJmOTNiYzZjZjQ4IiwiZXhwIjoxNTEwMjYwNzQwLCJpYXQiOjE1MTAyMTc1NDB9.lFDmEA-1VzcGGh-KsVKaGA-zAWGkcVuOHDJiUR5Oq3yVnkPPqAfx3wgUnBlXD6nCWPq2xn6RgwgQHRgkTOIcbyAELKKjZnXsp1pm5lRvoguKQw9Segq8xlkmeU-JS49EbI6OlGG2amQPzjNe86N4xForWI6C7QtE5it6Glujl8lDdN4Rbo03n1UiwQ3qJ1BB6_2nYD3UtrXMvqnr9XCumYtmN1MupOC70tWFONI0ojQS43WL7wCab780HxVSGmCapsoOX25mbzV3wlI6JERJ6iiMp1-ups2IQ7Fe2zd_bXED_beQMh-ry1IVY18jSrBI5x4v8eXomphF2Pv5ArDkjw
		kubectl config set-context mycluster.icp-context --user=mycluster.icp-user --namespace=default
		kubectl config use-context mycluster.icp-context
                #NAMESPACE=`cat /var/run/configs/registry-config/namespace`
                #REGISTRY=`cat /var/run/configs/registry-config/registry`
                DEPLOYMENT=`kubectl get deployments | grep compute-interest-api | awk '{print \$1}'`
                kubectl get deployments \${DEPLOYMENT}
                if [ \${?} -ne "0" ]; then
                    # No deployment to update
                    echo 'No deployment to update'
                    exit 1
                fi
                # Update Deployment
                kubectl set image deployment/\${DEPLOYMENT} web=mycluster.icp:8500/wintondev/compute-interest-api:${env.BUILD_NUMBER}
                kubectl rollout status deployment/\${DEPLOYMENT}
                """
            }
        }
    }
}
