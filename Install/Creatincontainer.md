## create YAML file and create pod with kubectl

cat << EOF | kubectl create -f -

apiVersion: v1

  kind: Pod

metadata:

  name: nginx

spec:

containers:

    - name: nginx
      
      image: nginx
EOF

## Get a list of pods and verify that your new nginx pod is in the Running state

kubectl get pods

## Get more information about your nginx pod

kubectl describe pod nginx

## Delete the pod:

kubectl delete pod nginx
