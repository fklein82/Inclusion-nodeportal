allow_k8s_contexts('tap-demo1-3')

SOURCE_IMAGE = os.getenv("SOURCE_IMAGE")
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='dev')

k8s_custom_deploy(
    'inclusion-frontend-web',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes >/dev/null" +
               " && kubectl get workload inclusion-frontend-web --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['.'],
    container_selector='workload',
    live_update=[
      sync('/express','/workspace/express')
    ]
)

k8s_resource('inclusion-frontend-web', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'inclusion-frontend-web'}])
