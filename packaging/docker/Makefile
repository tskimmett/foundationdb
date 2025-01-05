FDB_VERSION ?= 7.3.56
REGISTRY ?= ghcr.io/tskimmett

all: build-fdb build-sidecar build-monitor
push-all: push-fdb push-sidecar push-monitor

#
# fdb
#

%-fdb: IMAGE = fdb-arm:$(FDB_VERSION)
build-fdb:
	docker build --build-arg FDB_VERSION=$(FDB_VERSION) --tag $(IMAGE) --target foundationdb .
push-fdb: build-fdb
	docker tag $(IMAGE) $(REGISTRY)/$(IMAGE)
	docker push $(REGISTRY)/$(IMAGE)
	docker rmi $(REGISTRY)/$(IMAGE)

#
# sidecar
#

# sidecar image needs the -1 suffix
%-sidecar: IMAGE = fdb-sidecar-arm:$(FDB_VERSION)-1
build-sidecar:
	docker build --build-arg FDB_VERSION=$(FDB_VERSION) --tag $(IMAGE) --target foundationdb-kubernetes-sidecar .
push-sidecar: build-sidecar
	docker tag $(IMAGE) $(REGISTRY)/$(IMAGE)
	docker push $(REGISTRY)/$(IMAGE)
	docker rmi $(REGISTRY)/$(IMAGE)

#
# monitor
#

%-monitor: IMAGE = fdb-monitor-arm:$(FDB_VERSION)
build-monitor:
	# copy the source here so it can be referenced by Dockerfile
	rm -rf ./fdbkubernetesmonitor
	cp -r ../../fdbkubernetesmonitor ./fdbkubernetesmonitor
	docker build --build-arg FDB_VERSION=$(FDB_VERSION) --tag $(IMAGE) --target fdb-kubernetes-monitor .
push-monitor: build-monitor
	docker tag $(IMAGE) $(REGISTRY)/$(IMAGE)
	docker push $(REGISTRY)/$(IMAGE)
	docker rmi $(REGISTRY)/$(IMAGE)
