IMAGE_NAME = mi-nginx
IMAGE_TAG = latest
FULL_IMAGE = $(IMAGE_NAME):$(IMAGE_TAG)
DEPLOYMENT_FILE = deployment.yaml
SERVICE_FILE = service.yaml

# Detecta si es Minikube o Kind
CLUSTER_TYPE = $(shell kubectl config current-context | grep -q minikube && echo minikube || echo kind)

.PHONY: build push deploy clean port-forward

build:
	docker build -t $(FULL_IMAGE) .

push:
ifeq ($(CLUSTER_TYPE),minikube)
	minikube image load $(FULL_IMAGE)
else
	kind load docker-image $(FULL_IMAGE)
endif

deploy:
	kubectl apply -f $(DEPLOYMENT_FILE)
	kubectl apply -f $(SERVICE_FILE)

clean:
	kubectl delete -f $(SERVICE_FILE) || true
	kubectl delete -f $(DEPLOYMENT_FILE) || true

port-forward:
	kubectl port-forward service/mi-nginx-service 8080:80

