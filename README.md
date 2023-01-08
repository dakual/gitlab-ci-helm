Install kubernetes gitlab agent
```sh
helm repo add gitlab https://charts.gitlab.io
helm repo update
helm upgrade --install test gitlab/gitlab-agent \
    --namespace gitlab-agent-test \
    --create-namespace \
    --set image.tag=v15.8.0-rc2 \
    --set config.token=yKUkM8YtexhG1AwNw-KG4CDbxxzT9TsRquAEjDwQ3LxzMCL4LQ \
    --set config.kasAddress=wss://kas.gitlab.com
```

Install gitlab runner
```sh
docker run -d --name gitlab-runner --restart always \
-v ~/Genel/gitlab/config:/etc/gitlab-runner \
-v /var/run/docker.sock:/var/run/docker.sock \
gitlab/gitlab-runner:latest
```

Register gitlab agent
```sh
docker exec -it gitlab-runner bash

gitlab-runner register -n \
--description "Sample Runner 1" \
--url <GITLAB-URL> \
--registration-token <GITLAB-REGISTRATION-TOKEN> \
--executor docker \
--docker-image "docker:latest" \
--docker-volumes /var/run/docker.sock:/var/run/docker.sock \
--docker-privileged \
--tag-list prod

docker logs gitlab-runner -f
```

```sh
gitlab-runner list
gitlab-runner stop
gitlab-runner start
gitlab-runner restart
gitlab-runner unregister --name <runner-name>
gitlab-runner unregister --all-runners
```