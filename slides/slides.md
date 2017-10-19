<section data-background-image="images/content_joke.jpg">

note: Welcome friends, this is the mid-morning interpretive dance workshop. If you're not here for the 9:35 interpretive dance workshop...

---

# Containers:
## Beyond the Hype

note: Well, really, you're more likely here for the "Containers: Beyond the Hype" talk. Given that 30 minutes is a small window and containers are a huge topic, my goal then, is to not go super deep on any one topic. Rather, we'll take a quick tour of containers, and give you some hands-on homework.

---

## Intro

Cody Bunch

* @cody_bunch
* blog.codybunch.com
* @vBrownBag

note: I'm Cody Bunch, and here's where you can find me. My twitter handle, blog, and the vBrownBag podcast.

---

## Agenda

* Try at home resources
* What are containers & a bit of history
* Where do they fit
* How your life changes
* Try at home!

note: At a high level here's what we've got: What are containers, a bit of history, Where do they fit, How your life changes, Try at home!

---

## Get Ready...

If you're going to photograph slides, there following 4 are the ones you want.

---

### Resources: Learning (1/4)

* __Books:__
    - Kubernetes up and running
   http://amzn.to/2yzIiul

    - Docker up and running
   http://amzn.to/2ikqvmK

    - DevOps with OpenShift
   http://amzn.to/2ytwLPk

* __Video__:
    - There's a vBrownBbag for that
   http://bit.ly/2yxS8wp

* __Hands on:__
    - Kubernetes (K8S)
   https://github.com/kelseyhightower/kubernetes-the-hard-way

    - Docker Swarm

   https://docs.docker.com/engine/swarm/

note: I have personal experience with each of these books and highly recommend them as you get started. Additionally, the vBrownBag podcast, an engineers training engineers type show, has done a series of podcasts on containers, docker, swarm, kubernetes, and more. Well worth your time. Additionally, I highly recommend the Kubernetes the hard way tutorial. This is, because it is hard, however, as you work through it, you will gain an understanding of each of the fundamental bits of how Kubernets works, and how they work together.

---

### Resources: References (2/4)


(No blog was harmed during the making of this presentation)

* Presenting With Docker
    - https://kartar.net/2014/05/presenting-with-docker/
* Docker, LXC, & Rkt
    - https://sreeninet.wordpress.com/2015/02/02/containers-docker-lxc-and-rocket/
* rkt
    - https://coreos.com/rkt/
* contained.af https://contained.af/
* Presenting in Docker
    - https://kartar.net/2014/05/presenting-with-docker/
* RedHat - "History of Containers"
    - http://rhelblog.redhat.com/2015/08/28/the-history-of-containers/
* Aqua - "From chroot to Docker"
    - https://blog.aquasec.com/a-brief-history-of-containers-from-1970s-chroot-to-docker-2016

note: As with most things, one learns from others, so to give credit where it is due, here are links to the resources that went into building this presentation.

---

### Resources: Runtimes (3/4)

Installing Container Runtimes

* LXC
    - https://linuxcontainers.org/lxc/getting-started/
* Rkt
    - https://coreos.com/rkt/docs/latest/trying-out-rkt.html
* Docker
    - https://www.docker.com/community-edition
    - curl https://get.docker.com/ | sudo bash

note: Next up, links to how to get started with the various container runtimes. We'll touch on these more in a bit.

---

### Resources: Hands on (4/4)

__hello-world__

```
$ docker run --rm hello-world
```

__whalesay (Like cowsay, with whales)__

```
$ docker run --rm docker/whalesay potato
```

__Hollywood style__

```
$ docker run --rm jess/hollywood
```

__ ManageIQ__

```
$ sudo docker run --privileged -d -p 8443:443 manageiq/manageiq:fine-3
```

Then, browse to https://localhost:8443, yay ManageIQ

note: Now, some examples you can try at home. These assume you have docker installed already. The first is a simple hello-world. It's the first thing you do in a new programming or scripting language. Here it also helps you validate that docker is indeed running and installed correctly. Next is whalesay. This is much like cowsay, but with whales. You'll see this again later too. Next up is hollywood... if we get time, we'll show this off as a live demo. And finally, ManageIQ. ManageIQ is a sort of orchestration suite, that let's you build a service catalog that supports approvals, introspections, and more. This one is included here, as, it'll be the quickest deployment of such a complex software stack that you'll ever do.

---

## What are containers?

* Process isolation
* Idempotent bundle
* Single executable

note: We're going to simplify things a bit for the sake of time here. Containers, at their core, are a process isolation mechanism. With traditional visualization, that is the virt we became familiar with on VMware GSX, ESXi, KVM, etc. where to some degree or another the hardware stack is replicated or emulated and a full OS installed, configured, applications loaded, etc.

Containers, on the other hand, provide a stand alone executable for a bit of software, that includes everything that is required to run. What does this mean? Withing a container are included the app itself, runtime(s), system tools, settings, etc.

---

### Benefits

* Speed
* Size
* Idempotentcy*
* Security?

note: There are a number of benefits to running with containers. I'd like to call out a few here. Speed, Size, Idempotency, and Security. Security is an interesting topic, well, you'll see.

---

#### Size

Container:

```
$ docker images hello-world
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              05a3bd381fc2        5 weeks ago         1.84kB

$ docker images docker/whalesay
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
docker/whalesay     latest              6b362a9f73eb        2 years ago         247MB
```

VM:

```
# ls -alh
total 709M
drwxr-xr-x 2 root root 4.0K Oct 19 05:17 .
drwxr-xr-x 3 root root 4.0K Oct 19 05:17 ..
-rw-rw-rw- 1 root root  11K Oct 19 05:16 box.ovf
-rw-rw-rw- 1 root root   26 Oct 19 05:16 metadata.json
-rw-rw-rw- 1 root root 709M Oct 19 05:17 packer-ubuntu-16.04-amd64-disk1.vmdk
-rw-rw-rw- 1 root root  258 Oct 19 05:16 Vagrantfile
```

note: The first of these is size. While a VM image is often on the order of several GB in size (yes you can pear down the installs, but by default). The average Windows installation is what? 20-40GB for the OS disk now?

Contrast that with a container, which, on the high side, is maybe several hundred MB.

---

#### Speed

```
$ time (docker run --rm  docker/whalesay cowsay potato)
 ________
< potato >
 --------
    \
     \
                    ##        .
              ## ## ##       ==
           ## ## ## ##      ===
       /""""""""""""""""___/ ===
  ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~
       \______ o          __/
        \    \        __/
          \____\______/
real    0m0.827s
user    0m0.019s
sys 0m0.013s
```

note: With a VM launch, even with things like linked clones, storage assist, nvme, you are still beholden to OS boot times. Which while quick can still be in the order of minutes. Mind, single digit minutes is outstanding... Who here hasn't tried to boot a huge VM on a single spindle 5400 RPM drive.

With containers, instead of launching an entire OS, you are launching a process. For the most part this is sub-second.

---

### Idempotentcy

* Images, once built, are static.
* Executes the same everywhere
* Gotcha: Build time instructions may not be

---

### Example of build time non-idempotency:

```
RUN apt-get -qqy update
RUN apt-get -qqy install git nodejs npm
RUN ln -s /usr/bin/nodejs /usr/bin/node
WORKDIR /opt

# Install reveal.js
RUN git clone https://github.com/hakimel/reveal.js.git presentation
WORKDIR /opt/presentation
RUN npm install -g grunt-cli
RUN npm install
RUN sed -i "s/port: port/port: port,\n\t\t\t\t\thostname: \'\'/g" Gruntfile.js
```

note: Here is an example of a Dockerfile that at build time, is not idempotent. Our first few steps specify that we want to update our apt-cache, which alone, can be different from one day to the next. What this means, is if you produce an image on Monday, you will produce a subtly different image on a Wednesday.

Here's the trick, however. The image you produced on Monday, say it's version 1.0, will continue to be version 1.0 on Wednesday, and will continue to operate the same everywhere. Wednesday's version 1.2, is an entirely separate artifact. Like version 1.0, version 1.2 will be the same 1.2 wherever it is run.

---

### Security?

Weakest point of any deployment is the config. That said... given the many new concepts and configuration points, extra care is needed.

note: emphasize that configuration is going to continue to be the most likely source of security breaches. As far as container escapes... they're about as likely as VM escapes. There is a site, contained.af that is essentially a capture the flag environment, where the goal is to escape from the container. If you'll recall from the early days of VMware, there were similar challenges to attempt VM escapes.

---

### History of containers in 30s

note: Now, because of time constraints, we're going to attempt to cover the entire history of containers in 30 seconds.

---

#### Early Days

* 1979 - chroot in Unix V7. Allows changing the root directory of a process.
    - Made it to BSD in 1982 (For perspective, that's when I was born)

note: The 70's were a great time for computers. A lot of really interesting technologies and concepts came from that era. Like Virtual Machines, the early days of containers started here, with chroot. chroot let you change the root directory of a process which let you minimize it's blast radius should it break.

---

#### 2000 - 2013

* 2000 - BSD Jails. Clearer separation of concerns, better security & admin.
    - Each "Jail" could be assigned an IP
* 2001 - Linux VServer. Like jails, but for Linux. OS virt by kernel patch
    - Last stable in 2006
* 2004 - Solaris. Introduced resource mgmt, snapshots, and cloning (ZFS)
* 2005 - OpenVZ. More OS Virt via kernel patch. Not mainline kernel tho
* 2006 - Process Containers. This became cgroups (this is important)
* 2008 - LXC. cgroups & namespaces, no kernel patches
* 2011 - Warden. A CloudFoundry production.
* 2013 - LMCTFY. A google production. Stopped in 2015
    - concepts contributed to libcontainer

note: The early 2000's brought about a Renaissance in container tech, with each making improvements on the last.

---

#### Today - Runtimes, Platforms, and Bears, Oh My!

- Docker
    + "Easy Button" - docker run hello-world
    + Swarm (cluster management/orchestration)
- VMware Integrated Containers
    + Deep integration with vSphere
- CoreOS
    + Tectonic (cluster management/orchestration)
    + Quay (Container registry)
- Kubernetes (k8s) (cluster management/orchestration)
- OpenShift
    + Opinionated k8s install with additional tooling
- Mesos
    + Distributed systems kernel
    + A different approach to container
- More! This space is moving fast, and I've likely missed some.

note: Which brings us to today. Each of these listed here provides just a part of the container stack, be it a container runtime, or orchestration platform and so forth. It's important to note, that, like OpenStack a few years ago, and VMware a few years before that, the container space is evolving rapidly, and who knows where it'll take us in the next 12 months.

---

## What does it mean for me?

Containers change things. They change they way applications are built, deployed, and ultimately managed. They change resource consumption models, and they scale differently. They introduce new networking ideas and concerns.

---

### New workflows

VM Provisioning takes many forms, be it Packer & Terraform, to artesian, free range, hand crafted golden images. The workflow to bring a container online can be significantly different.


Development workflows change. Developers can now ship you a container as an artifact of their build process, rather than a set of instructions for deploying a JAR with version mismatches, etc.

---

### Container workflow

1. Define the image: Dockerfile
2. Build the image: ```docker build -t our/newcontainer ./Dockerfile```
3. Define the environment: docker-compose, kubernetes, etc
4. Run the image: ```docker run, kubectl```, etc

note: Compare how VMs get built currently to how containers get built.

---

### Development workflow

__Separation of concerns:__

1. Container provides idempotent runtime
2. Code artifacts are mounted via volumes
3. Make changes locally
4. Mounted volumes reflect changes immediately
5. Container image is now a build artifact

note: Describe the VM process of development, where each dev may have different libraries and settings in their environment, how a container image codifies that into an idempotent artifact, that can be used across teams. Then, live demo changing the theme on the slides to emphasize this point.

---

### New tools

Your tool sets will need to change. They way you monitor uptime, resource contention, and service availability will change.

The way you manage a fleet of hosts will change. Scheduling workloads and managing your host clusters will change.

---

## How to get there from here

As with most things in IT, "It depends". Some recommendations:

* Start small - one app, one utility, something small
* Work through kubernetes the hard way
* Do a few more small things
* Work your way up

---

### Getting there from here, revisited

VMware Integrated Containers

* Familiar tool sets
* Familiar interfaces

note: Call out that it works with the vsphere tools that you are familiar with today, nsx, vsphere, vsan, etc. How the familiar tools can make the transition easier.

---

## A quick demo

note: Who here watches Mr Robot, well, any computer ever in a TV show or movie? docker run --rm -it --name mr_robot jess/hollywood. What this command is doing is telling docker to run an image, clean up after itself when finished. Run the image in an interactive terminal with the name of mr_robot. Finally we tell it what image we want to run. Fire up demo.

---
