## MiniShift Full Demo WalkThru

### Requirements:
* See Installation doc for requirements

### WalkThru:

#### Exercise One: Starting an OpenShift Cluster
* MINISHIFT_USERNAME is your Red Hat ID.  Set as an environment variable or it should prompt
* As MiniShift starts, talk about what it is and RH Developer Network
  * Initial build/start = 10-15m, subsequent starts = 3-4m
  * Minishift is a tool that helps you run OpenShift locally by running a single-node OpenShift cluster inside a VM.
  * OpenShift is our container orchestration product.
  * RH Developer Network is a free membership that gives access to subscriptions for RHEL, JBoss, and OpenShift.  There is also a wealth of additional information including tutorials, e-books, and KBase content.  There’s even  a newsletter to keep you up-to-date on everything happening on RH Developer Network
```
# minishift ssh = runs commands in MiniShift VM
# docker ps = talk about k8s and origin (OpenShift) containers
```
  * Webconsole, router, registry
```
# docker images = pull from RH registry, talk to security and vetting process
# oc login -u system:admin = talk about projects other than default
```
  * Admin can see the infra k8s and openshift projects
```
# oc cmds = we’ll spend more time with them later
```
* Web UI = login as developer, will also spend more time later, but talk to catalog with available dev tools, also Home Page tour, and Documentation + other resources

#### Exercise Two: Using docker commands
```
# mysql-1st = highlight troubleshooting with logs cmd
```
* In my testing, build took 2-3 minutes
```
# docker inspect = open another cmd window
# minishift ssh --docker inspect mysql-2nd
```
  * State, Mounts, Config (Env, Cmd), Network
```
# mysql-3rd
# whoami
# ps -ef
```
  * highlight security of RH images vs Docker Hub
* Notice: mysql command to connect to mysql-2nd = containers can talk to one another

#### Exercise Three: Using & Creating docker images
```
# docker exec = # whoami, here we are root
```
  * Image was pulled from docker.io not RH, and centos vs rhel
* Access apache via port redirect, in 2nd cmd window:
```
# docker ps -a
# docker diff = [A]dded  [C]hanged
# docker commit = creates/saves new image with above changes
```
  * -a, author	-m, message
```
# docker tag = make it easily identifiable and consumable; note before & after
# httpd-custom = based on above; note port
```

#### Exercise Four: Intro to Dockerfile
* Now using Dockerfile vs cmdline
* In testing, build time was 3-5 minutes
```
# docker images = note RHEL image is smaller than do081/httpd2 due to Dockerfile additions
```

#### Exercise Five: Kubernetes + docker made simple
* Container foundation complete = moving on to OpenShift/MiniShift
* Note: oc cmds run on desktop vs minishift VM for docker cmds
```
# oc new-app = look familiar?
```
* In testing, build time = 2-3 min
```
# oc describe pod = what is a pod
```
  * The Kubernetes scheduling unit is the pod, which is a grouping of containers sharing a virtual network device, internal IP address, TCP/UDP ports, and persistent storage.
  * Node, Labels, IP, Containers, Events
```
# oc describe svc = what is a svc
```
  * A service is a logical name representing a set of pods. The service is assigned an IP address and a DNS name, and can be exposed externally to the cluster via a port or a route.
  * IP = cluster vs Endpoint = container(s)
```
# oc describe dc = what is a deployment config
```
  * A deployment configuration defines the template for a pod and manages deploying new images or configuration changes whenever the attributes are changed. A single deployment configuration is usually analogous to a single microservice. Deployment configurations can support many different deployment patterns, including full restart, customizable rolling updates, as well as pre and post lifecycle hooks.
```
# oc export = talk about ease of OpenShift, it makes k8s easy
# oc rsh: # whoami = notice user
# oc rsh: # hostname = notice container name
# oc rsh: # ps -ef, ls -la /var/www/html
# show databases = notice testdb from original oc new-app cmd
```
* Web UI: open db project, walk thru interface
  * Optional: scale up & down

#### Exercise Six: Putting it all together = deploying an application
```
# s2i = what is it, talk during build, after oc logs cmd
```
  * The Source-to-Image (S2I) process in OpenShift pulls code from an SCM repository
  * Automatically detects which kind of runtime that source code needs
  * Starts a pod from a base image specific to that kind of runtime
  * Inside this pod, OpenShift builds the application the same way that the developer would (for example, running maven for a Java application)
  * If the build is successful, another image is created, layering the application binaries over its runtime
  * This image is pushed to an image registry internal to OpenShift
  * A new pod can then be created from this image, running the application
  * S2I can be viewed as a complete CI/CD pipeline already built into OpenShift.
* In testing, build time = 2-3 min
```
# oc describe bc = what is a build config
```
  * A build configuration contains a description of how to build source code and a base image into a new image.
```
# oc expose = explain route
```
  * A route is a DNS entry that is created to point to a service so that it can be accessed from outside the cluster.
* Web UI: open s2i project, walk thru interface

### Troubleshooting
* If minishift gets “already registered” error
  * Restart with “minishift stop && minishift start --skip-registration”
* Missing path to “oc”
  * RHEL: eval $(minishift oc-env)
  * Windows cmd:
  ```
  C:\> @FOR /f "tokens=*" %i IN ('minishift oc-env') DO @call %i PowerShell: PS C:\> & minishift oc-env | Invoke-Expression
```
