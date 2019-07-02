# private_docker_repo

```bash
# mkdir -p /docker-data/certs

```

```bash
# openssl req -newkey rsa:4096 -nodes -sha256 -keyout /docker-data/certs/domain.key -x509 -days 365 -out /docker-data/certs/domain.crt


Country Name (2 letter code) [XX]:
State or Province Name (full name) []:
Locality Name (eg, city) [Default City]:
Organization Name (eg, company) [Default Company Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (eg, your name or your server's hostname) []:docker-repo.pure.mel.lab
Email Address []:


```
```
# mkdir -p /docker-data/auth

# docker run --entrypoint htpasswd registry -Bbn admin Passw0rd > auth/htpasswd

```



```bash
# docker run -d -p 5000:5000 -v /docker-data/images:/var/lib/registry -v /docker-data/certs:/certs -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key -v `pwd`/auth:/auth -e "REGISTRY_AUTH=htpasswd" -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" -e REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd --restart on-failure --name myregistry docker.io/registry

```
### copy domain.crt to each edge cluster

```
# scp -r /docker-data/certs/domain.crt 10.1.11.211:/root/
```

### log on to the remote server

```
# mkdir -p /etc/docker/certs.d/docker-repo.pure.mel.lab:5000
# cp -rf /root/domain.crt /etc/docker/certs.d/docker-repo.pure.mel.lab\:5000/
```
```
docker push docker-repo.pure.mel.lab:5000/hyperkube:v1.13.5-ee
docker push docker-repo.pure.mel.lab:5000/pause:3.1
```