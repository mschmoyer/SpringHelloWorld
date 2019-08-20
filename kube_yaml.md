apiVersion: apps/v1
kind: Deployment
metadata:
  name: aaclient
  namespace: prod  
  labels:
    app: aaclient
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aaclient
  template:
   metadata:
     labels:
      app: aaclient
   spec:
     containers:
     - name: aaclient
       image: registry01.humanify.com/root/visual
       ports:
       - containerPort: 3005         
       volumeMounts:
       - mountPath: /opt/fischer/agent-assist/aa-server/config
         name: aaclient
     imagePullSecrets:
     - name: regcred 
     restartPolicy: Always
     volumes:
      - name: aaclient
        persistentVolumeClaim:
         claimName: kube-ai

---
apiVersion: v1
kind: Service
metadata:
  name: aaclient
  namespace: prod
spec:
  type: NodePort
  ports:
  - port: 3005
    targetPort: 3005
    nodePort: 30305
    protocol: TCP
    name: aaclient
  selector:
    app: aaclient
