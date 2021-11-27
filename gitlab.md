# gitlab


## ci/cd

* `docker pull gitlab/gitlab-runner`

### runner registration

* `docker pull gitlab/gitlab-runner`
* `docker run --rm -t -i -v config:/etc/gitlab-runner gitlab/gitlab-runner register # create config directory and file`
* get token from settings -> ci/cd
* `docker run -d --name gitlab-runner --restart always -v /home/hus/deploy/config:/etc/gitlab-runner -v /var/run/docker.sock:/var/run/docker.sock gitlab/gitlab-runner:latest # run container (as daemon)`


## references

* https://gitlab.com/gitlab-org/gitlab-foss/tree/master/lib/gitlab/ci/templates
* https://docs.gitlab.com/ee/user/project/pages/getting_started_part_four.html
* https://docs.gitlab.com/ee/ci/yaml/README.html
* https://gitlab.eng.vmware.com/help/api/releases/index.md
* https://gitlab.eng.vmware.com/help/user/project/releases/index
* https://docs.gitlab.com/ee/ci/runners/
* https://docs.gitlab.com/runner/install/docker.html
* https://www.lambdatest.com/blog/automated-testing-pipeline-with-gitlab-ci-cd-and-selenium/
* https://stackoverflow.com/questions/29520905/how-to-create-releases-in-gitlab
* https://juhani.gitlab.io/go-semrel-gitlab/
* https://stackoverflow.com/questions/47280922/role-of-docker-in-docker-dind-service-in-gitlab-ci
* https://docs.gitlab.com/ce/ci/docker/using_docker_build.html#use-docker-in-docker-executor
