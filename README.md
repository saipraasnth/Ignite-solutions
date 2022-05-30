# Ignite-solutions

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  namespace: default
spec:
  replicas: 8
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 8080
        resources:
          # You must specify requests for CPU to autoscale
          # based on CPU utilization
          requests:
            cpu: "50m"
            
  We Create another file nginx-hpa.yaml for HPA

apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: nginx
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx
  minReplicas: 5
  maxReplicas: 10
  targetCPUUtilizationPercentage: 50
  targetMEMORYUtilizationPercentage: 60
  
  if you use cli then the code for that one is 
  kubectl autoscale deploy/application-cpu --cpu-percent =50 --min=5 --max=10
  kubectl autoscale deploy/application-memory --cpu-percent =60 --min=5 --max=10
