# golang

since google services are blocked in china, use: `github.com/golang`

https://goproxy.cn/

some common packages required:

1. `github.com/golang/text` # move to work/src/golang.org/x/text
1. `github.com/golang/net` # move to work/src/golang.org/x/net


## modules

1. `go mod init`
1. `go build # this will create go.mod/go.sum entries`
1. `go list -m all`
1. `go list -m -versions <mod/name>`
1. `go mod tiny`


## CROSS COMPILE

* `export GOOS=windows`
* `export GOARCH=amd64`

* https://www.digitalocean.com/community/tutorials/building-go-applications-for-different-operating-systems-and-architectures
* https://stackoverflow.com/questions/20728767/all-possible-goos-value
* https://www.digitalocean.com/community/tutorials/customizing-go-binaries-with-build-tags
* https://stackoverflow.com/questions/22367201/sort-a-map-of-structs-in-golang