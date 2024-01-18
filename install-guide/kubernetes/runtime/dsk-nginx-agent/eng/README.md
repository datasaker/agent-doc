# dsk-nginx-agent (english)
## Installing DataSaker Nginx agent in Kubernetes environment
`Nginx agent` is an agent that collects Nginx information from DataSaker.

## Have you done DataSaker pre-work?
If you have not done the pre-work of `DataSaker` in the current Kubernetes environment, please do the pre-work of `DataSaker` first. [DataSaker pre-work](README.md)

## Install Nginx agent
### 1. Activate nginx_status
Add the stub_status setting to the nginx configuration file. If you modify the file and reload nginx, prometheus can check the status of nginx and create metrics.
```
server {
    listen       80;
    server_name  localhost;
    
    # ... added part ...
    location /stub_status {
        stub_status on;
        allow all;
    }
    # ... skip ...
}
```

Reload nginx to apply the above settings.
```bash
$ sudo nginx -s reload
```

When you reload nginx, you can check the stub_status as follows.
```bash
$ curl http://localhost/stub_status

Active connections: 1
server accepts handled requests
 2 2 2
Reading: 0 Writing: 1 Waiting: 0
```

### 2. Register Nginx agent settings
Please register nginx information in nginxAgent.list[].
```shell
cat << EOF >> ~/datasaker/config.yaml

nginxAgent:
  enabled: true
  tolerations: []
  imgPolicy: 'Always'
  imgVersion: 'latest'
  tags: []
  resource: {}
  logLevel: 'INFO'
  targetAddr: "http://localhost:8080/nginx_status"
  listenPort: 19113
EOF
```

The following is a detailed description of each setting value.

| Setting     | Description                        |
|-------------|------------------------------------|
| enabled     | Whether to enable agent            |
| tolerations | Kubernetes toleration settings     |
| imgPolicy   | Kubernetes image policy settings   |
| imgVersion  | Agent image version settings       |
| tags        | Agent tag settings                 |
| resource    | Agent Kubernetes resource settings |
| logLevel    | Agent log level settings           |
| targetAddr  | Nginx address                      |
| listenPort  | Agent port settings                |

### 3. Install Nginx agent
```shell
helm upgrade datasaker datasaker/agent-helm -n datasaker \
  -f ~/datasaker/config.yaml
```
