# github

## docker registry

* `https://ghcr.io`


## github actions

### steps

1. create a package release: https://github.com/stephenhu/static/releases/new
1. make package public, default is private
1. set action access for package: https://github.com/users/stephenhu/packages/container/static/settings
1. create action repo: https://github.com/stephenhu/blog-www/actions (or .github/workflows/*.yml)
1. create action workflow: https://github.com/stephenhu/static-action, publish

### accessing ghcr.io container registry

this is github's very own container repository, not docker's.  you need to create a personal access token in order to login, i don't think login credentials work.

1.  create a token here:  https://github.com/settings/tokens under _personal access tokens_ give it repo, write:packages, and delete:packages privileges
1.  copy this token down, it is important and is not retrievable once you close this page
1.  this token also expires, default is 15 days or so, you can set it to something else, but it will expire, and they don't recommend no expiration even though the ui allows it.

## github pages

* dns a record for 185.199.108.153 - 185.199.111.153

## references


## notes

* rate limits are 60/h for unauthenticated requests

## pages

* https://help.github.com/en/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain
* https://docs.github.com/en/rest/reference
* https://docs.github.com/en/rest/overview/endpoints-available-for-github-apps
* https://docs.github.com/en/rest/overview/resources-in-the-rest-api#rate-limiting
* https://docs.docker.com/ci-cd/github-actions/
* https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry
* https://docs.github.com/en/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility
* https://github.com/stephenhu?tab=packages
* https://docs.github.com/en/packages/learn-github-packages/connecting-a-repository-to-a-package
