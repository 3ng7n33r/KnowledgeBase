# Docker
summarize [this](https://vsupalov.com/6-docker-basics/)

Docker Containers are similar to virtual machines with the main difference being that containers provide a way to virtualize an OS so that multiple workloads can run on a single OS instance. With VMs, the hardware is being virtualized to run multiple OS instances.
![VMs vs Containers](https://raw.githubusercontent.com/3ng7n33r/KnowledgeBase/master/src/static/VMvsContainer.png)**Container**
An isolated environment
**Image**
It’s like a powered down computer (with software installed), which is ready to be executed with a single command. Only instead of starting the computer, you create a new one from scratch (container) which looks exactly like the one you chose (image).
**Dockerfile**
A Dockerfile is a set of precise instructions, stating how to create a new Docker image, setting defaults for containers being run based on it and a bit more. In the best case it’s going to create the same image for anybody running it at any point in time.
**Volumes**
While containers are lost after removal and Images can't be changed, Volumes are saved files that can stay on the host system.

## VM vs. Container vs. Virtuelenv

Containers sit on top of a physical server and its host OS—for example, Linux or Windows. Each container shares the host OS kernel and, usually, the binaries and libraries, too. Shared components are read-only. Containers are thus exceptionally “light”—they are only megabytes in size and take just seconds to start, versus gigabytes and minutes for a VM.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM2OTUxN119
-->