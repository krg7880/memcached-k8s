node {
  stage 'Clone'

  checkout scm

  stage concurrency: 1, name: 'Deploy'

  // set the image tag in the deployment
  sh '''
    MEMCACHED_TAG="${MEMCACHED_TAG:-latest}" 
    sed -i.bak "s#MEMCACHED_TAG#${MEMCACHED_TAG}#" deploy/deployment.yml
  '''

  // set the namespace
  sh("sed -i.bak 's#FQ_NAMESPACE#${FQ_NAMESPACE}#' deploy/namespace.yml")
  sh("sed -i.bak 's#FQ_NAMESPACE#${FQ_NAMESPACE}#' deploy/deployment.yml")
  sh("sed -i.bak 's#FQ_NAMESPACE#${FQ_NAMESPACE}#' deploy/service.yml")

  // create objects
  sh("kubectl apply -f deploy/namespace.yml")
  sh("kubectl apply -f deploy/deployment.yml") 
  sh("kubectl apply -f deploy/service.yml")
}