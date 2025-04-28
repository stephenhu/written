# minio

* minioadmin/minioadmin
* `docker run -p 9000:9000 -p 9001:9001 minio/minio server /data --console-address :9001`
* ```docker run \
  -p 9000:9000 \
  -p 9001:9001 \
  -e "MINIO_ROOT_USER=AKIAIOSFODNN7EXAMPLE" \
  -e "MINIO_ROOT_PASSWORD=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY" \
  quay.io/minio/minio server /data --console-address ":9001"```

## references

* https://min.io/docs/minio/linux/developers/go/minio-go.html
* https://github.com/minio/minio/blob/master/docs/docker/README.md
* https://min.io/docs/minio/container/operations/monitoring/healthcheck-probe.html
* https://github.com/minio/minio/issues/18389
