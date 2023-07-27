# ConfigMap 이란

---

- Configmap 은 Container 에 환경변수를 주입하는데 주로 사용된다.
- 클러스터 내부의 컨테이너에게 환경 변수를 넘기고 싶은 경우 “Pod 이 생성될 때 어떤 Configmap 의 어떤 key 값을 참조해 이것을 환겨엽ㄴ수로 써라” 는 스크립트를 넣는다.
- 그러면 container 는 생성될 때 해당 Configmap 의 value 값을 참조해서 환경변수로 넣어둔다.

## ConfigMap.yaml 신규배포

---

- nginx 설정 파일 (/etc/nginx/conf.d/default.conf) 을 volume 으로 mount 하기
- deployment.yaml 수정

### ConfigMap.yaml

```java
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-config
  labels:
    configmap: nginx-config
data:
  config: |-
    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;
        #access_log  /var/log/nginx/host.access.log  main;

        location / {
            root   /usr/share/nginx/html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   /usr/share/nginx/html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }
```

### deployment.yaml

- volumes (configMap) & VolumeMounts 부분 추가

```java
apiVersion: apps/v1          
kind: Deployment             
metadata:                
  name: junho  
  labels:
    app: junho              
spec:
  selector:                   
    matchLabels:     
      app: junho
  template:
    metadata:
      labels:                
        app: junho
    spec:
      nodeSelector:
        junho: in
      volumes:       
        - configMap:
            defaultMode: 256
            items:
            - key: config
              path: default.conf
            name: nginx-config
            optional: false
          name: vol3
        # [START app container]        
      containers:             
      - name: junho          
        image: {{ .Values.web.image }}:{{ .Values.web.tag  }}
        ports:
        - containerPort: 80
        volumeMounts:
         - mountPath: /etc/nginx/conf.d
           name: vol3
```