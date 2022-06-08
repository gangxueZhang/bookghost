# K8S部署nginx服务

## ingress路由配置

```yaml
          - path: /apaasview
            backend:
              serviceName: scrm-nginx-svc
              servicePort: 80
```

## apisix路由配置

```json
{
  "uris": [
    "/nginx/mock/*"
  ],
  "name": "scrm-nginx-svc",
  "methods": [
    "GET",
    "POST",
    "PUT",
    "DELETE",
    "PATCH",
    "HEAD",
    "OPTIONS",
    "CONNECT",
    "TRACE"
  ],
  "upstream": {
    "nodes": [
      {
        "host": "scrm-nginx-svc",
        "port": 80,
        "weight": 1
      }
    ],
    "timeout": {
      "connect": 30,
      "read": 30,
      "send": 30
    },
    "type": "roundrobin",
    "hash_on": "vars",
    "scheme": "http",
    "pass_host": "pass"
  },
  "labels": {
    "API_VERSION": "v1"
  },
  "status": 1
}
```

## configMap配置

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: scrm-nginx-config
  namespace: scrm-mss
  labels:
    app: scrm-nginx-app
data:
  default.conf: |
    server {
      listen 80;
      location /nginx/mock {
        proxy_pass  http://host/mock;
        proxy_read_timeout 600;
      }
      
      error_page   500 502 503 504  /50x.html;
      location = /50x.html {
        root   html;
      }
    }
```

## deployment配置

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: scrm-nginx
  namespace: scrm-mss
  labels:
    app: scrm-nginx-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: scrm-nginx-app
  template:
    metadata:
      labels:
        app: scrm-nginx-app
    spec:
      volumes:
      - configMap:
          name: scrm-nginx-config
        name: scrm-nginx-config
      containers:
        - name: scrm-nginx-app
          image: nginx
          ports:
            - containerPort: 80
              protocol: TCP
          volumeMounts:
            - mountPath: /etc/nginx/conf.d/default.conf
              name: scrm-nginx-config
              subPath: default.conf
          resources:
            limits:
              cpu: 30m
              memory: 50Mi
            requests:
              cpu: 20m
              memory: 30Mi
          imagePullPolicy: Always
      restartPolicy: Always
      dnsPolicy: ClusterFirst
      securityContext: {}
      imagePullSecrets:
        - name: tcr.ipstcr-dognqnnd-public
      schedulerName: default-scheduler

---
apiVersion: v1
kind: Service
metadata:
  name: scrm-nginx-svc
  namespace: scrm-mss
  labels:
    name: scrm-nginx-svc
spec:
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 80
  selector:
    app: scrm-nginx-app
  type: ClusterIP
```



