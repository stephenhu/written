# golang

since google services are blocked in china, use: `github.com/golang`

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
* `export GOARCH=386`
