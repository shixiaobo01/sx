```
repo=10.1.84.8
imageTag="${GIT_COMMIT}"
imageTag="${imageTag:0:8}_test"
image=10.1.84.8/touyan_image/mpstad:${imageTag}
war=target/mps-tad.war

# Generate a Dockerfile
cat <<-EOS>Dockerfile.dist
FROM $repo/touyan_base_image/centos7_jdk8_tomcat8:latest
RUN rm -rf /app/mps-tad.war
ADD $war /app/mps-tad.war
EOS
docker build --force-rm -f Dockerfile.dist -t $image .
docker push $image


cat <<-EOS>tad.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
   name: mpstad
   namespace: default
   labels:
     app: mpstad
spec:
   replicas: 1
   selector:
     matchLabels:
      app: mpstad
   template:
     metadata:
      labels:
        app: mpstad
     spec:
       hostAliases:
           - ip: "10.1.93.239"
             hostnames:
              - "mps.thfund.com.cn"
       imagePullSecrets:
       - name: harborsecret
       containers:
       - name: mpstad
         image: 10.1.84.8/touyan_image/mpstad:f7a883b4_pro
         ports:
           - containerPort: 8080
         volumeMounts:
           - name: drws-logs
             mountPath: /app/apache-tomcat-8.5.33/logs
           - name: drws-config
             mountPath: /app/apache-tomcat-8.5.33/conf/server.xml
           - name: mps
             mountPath: /app/mps/
         args: ["/app/mps/start.sh"]
       volumes:
         - name: drws-logs
           hostPath:
             path: /data/mpstad/logs
         - name: drws-config
           hostPath:
             path: /data/mpstad/config/server.xml
         - name: mps
           hostPath:
             path: /data/mpstad/mps/
EOS

cat <<-EOS>tad-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: service-mpstad
spec:
 ports:
 - port: 8080
   nodePort: 30022
   targetPort: 8185
   protocol: TCP
 selector:
   app: mpstad
 type: NodePort
EOS

cat <<-EOS>deployment.sh
if kubectl get Deployment mpstad >/dev/null 2>&1;then
kubectl set image deployment mpstad mpstad=$image
exit 0
else 
kubectl apply -f tad.yaml && kubectl apply -f tad-service.yaml
exit 0
fi
EOS

sh deployment.sh
```
