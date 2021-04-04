# Docker

## container

**what is container, what's the difference between container and virtual machine?**

container could be treated as a software that packaged up executable code and its dependencies or necessary libraries so that application can be ran on multiple different computing environment.

all these necessary libraries, tools and settings can be packaged into a static container image, which can be an active container in the run time.

Here are some differences:

1. there is no need to use the hypervisor layer under the container. Container will virtualize the Host OS and build on it directly.
2. there is no guest OS
3. container can run faster and more efficiently.
4. they use namespace to isolate. namespace is a Linux kernel construct that can restrict where the application code can run on the system, it can include shared libraries which can be accessible by multiple containers.