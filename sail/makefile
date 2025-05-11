IMAGE_NAME = sail
IMAGE_TAG = latest
FULL_IMAGE = $(IMAGE_NAME):$(IMAGE_TAG)
DEPLOYMENT_FILE = sail-deployment.yaml
SERVICE_FILE = sail-service.yaml
PV_FILE = pv.yaml
PVC_FILE = pvc.yaml
PYSAIL_VERSION ?= 0.2.5

CLUSTER_TYPE = $(shell kubectl config current-context | grep -q minikube && echo minikube || echo kind)

.PHONY: build push deploy clean port-forward volumes

build:
	docker build --build-arg PYSAIL_VERSION=$(PYSAIL_VERSION) -t $(FULL_IMAGE) .

push:
ifeq ($(CLUSTER_TYPE),minikube)
	minikube image load $(FULL_IMAGE)
else
	kind load docker-image $(FULL_IMAGE)
endif

volumes:
	kubectl apply -f $(PV_FILE)
	kubectl apply -f $(PVC_FILE)

deploy: volumes
	kubectl apply -f $(DEPLOYMENT_FILE)
	kubectl apply -f $(SERVICE_FILE)

clean:
	kubectl delete -f $(SERVICE_FILE) || true
	kubectl delete -f $(DEPLOYMENT_FILE) || true
	kubectl delete -f $(PVC_FILE) || true
	kubectl delete -f $(PV_FILE) || true

port-forward:
	kubectl port-forward service/sail-service 50051:50051
