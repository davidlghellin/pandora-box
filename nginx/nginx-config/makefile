IMAGE_NAME = mi-nginx
IMAGE_TAG = latest
FULL_IMAGE = $(IMAGE_NAME):$(IMAGE_TAG)
DEPLOYMENT_FILE = deployment.yaml
HTML_DIR = ./html

CLUSTER_TYPE = $(shell kubectl config current-context | grep -q minikube && echo minikube || echo kind)

.PHONY: build push deploy clean port-forward update-html

build:
	docker build -t $(FULL_IMAGE) --build-arg HTML_DIR=$(HTML_DIR) .

push:
ifeq ($(CLUSTER_TYPE),minikube)
	minikube image load $(FULL_IMAGE)
else
	kind load docker-image $(FULL_IMAGE)
endif

deploy: update-html
	kubectl apply -f $(DEPLOYMENT_FILE)
	kubectl apply -f service.yaml

clean:
	kubectl delete -f service.yaml || true
	kubectl delete -f $(DEPLOYMENT_FILE) || true
	kubectl delete configmap mi-html || true

port-forward:
	kubectl port-forward service/mi-nginx-service 8080:80

update-html:
	kubectl delete configmap mi-html || true
	kubectl create configmap mi-html --from-file=$(HTML_DIR)

