1️⃣ What is Docker?

Answer (as Abhishek explained):
Docker is an open-source containerization platform used to create and manage containers.
You can think of Docker as a tool that helps you build, run, and manage the entire lifecycle of containers — from writing Dockerfiles, building images, running containers, and finally pushing them to registries like Docker Hub, ECR, or ACR.

In interviews, don’t just define Docker; also talk about how you’ve used it.
For example:

“I use Docker to build application images, write Dockerfiles, run containers, and push those images to registries for deployment.”

This shows the interviewer your practical exposure, not just theory.

2️⃣ How are containers different from virtual machines?

Answer (as Abhishek explained):
Containers are lightweight compared to virtual machines.
They don’t have a complete operating system inside; instead, they share the host’s OS kernel and only contain the application, its dependencies, and a few necessary system libraries.

Virtual machines, on the other hand, run on a hypervisor and each VM has its own full operating system, kernel, and system libraries — making them heavier and slower to start.

He also mentioned not to say that containers “don’t have an OS.”
Instead, say they have “minimal system dependencies” required to run your app.

Example:
If you’re running a Java application, your container only needs:

your app code

Java runtime

minimal system libraries
That’s it — no full Ubuntu OS like a VM.

3️⃣ What is the Docker lifecycle?

Answer (as Abhishek explained):
The Docker lifecycle covers all stages of working with containers:

Start by writing a Dockerfile with the instructions needed to build your app.

Use docker build to create an image from that Dockerfile.

Run a container from the image using docker run.

Once tested, push the image to a registry like Docker Hub, ECR, or ACR.

Later, pull and deploy that image wherever required.

So when asked about Docker lifecycle, explain it as:

“I write the Dockerfile, build the image, create and run containers, and push the image to our registry for deployment.”

This shows you understand end-to-end container workflow.

4️⃣ What are the main components of Docker?

Answer (as Abhishek explained):
Docker has three main parts:

Docker Client (CLI): the command-line tool you use (docker build, docker run, etc.)

Docker Daemon: the background process that actually performs the tasks you request — it’s like Docker’s “engine.”

Docker Registry: where images are stored (Docker Hub, ECR, ACR, etc.)

Whenever you run a command like docker build, the client sends that request to the daemon, which builds the image.
If you stop the daemon, nothing in Docker will work — it’s the heart of Docker.

5️⃣ What’s the difference between COPY and ADD in Docker?

Answer (as Abhishek explained):

COPY is used to copy files from your local file system (host) into the image.

ADD can also copy files, plus it can fetch remote files from URLs or automatically extract .tar archives.

Example:
If you want to download a file directly from a GitHub raw link or from an S3 bucket, use ADD.
If you’re copying your local source code into the image, use COPY.

6️⃣ What’s the difference between CMD and ENTRYPOINT?

Answer (as Abhishek explained):

ENTRYPOINT defines the main executable that will always run inside the container.

CMD defines the default arguments that can be overridden at runtime.

You can use both together — ENTRYPOINT for the base command, CMD for its arguments.

Example:
Imagine a Python calculator app:

ENTRYPOINT ["python", "calc.py"]

CMD ["add", "2", "3"]

If you run docker run mycalc subtract 5 2, CMD values are replaced with your new arguments.

So:

“ENTRYPOINT is fixed, CMD can be overridden.”

7️⃣ What are the Docker networking types and which one is default?

Answer (as Abhishek explained):
Docker supports several network types:

bridge (default): containers talk through a virtual interface (docker0).

host: container uses the host’s network directly (no virtual bridge).

overlay: used for multi-host or Swarm setups.

macvlan: makes containers appear as real devices on your LAN.

The default is bridge network.

In bridge mode, each container connects to the host through a virtual Ethernet (veth) interface.
In host mode, the container shares the host’s IP and ports directly.

8️⃣ How do you isolate networking between containers?

Answer (as Abhishek explained):
By creating custom bridge networks instead of using the default docker0 bridge.

Example scenario:
If you have two containers — Login and Payments — and you don’t want them on the same network for security, you can do:

docker network create secure-network
docker run -d --network secure-network --name payments payments-app


Now the payments container is isolated.
Even if the login container is compromised, the attacker can’t reach payments.

9️⃣ What is a multi-stage build in Docker?

Answer (as Abhishek explained):
A multi-stage build lets you create Docker images in multiple stages — you build your app in one stage and copy only the final executable to another minimal image.

This reduces image size dramatically.

Example:
He gave a Golang example:
The initial image (Ubuntu base) was 800 MB.
After using multi-stage build (copying only the binary to a lightweight base like scratch), it became 1 MB — an 800× reduction!

So it’s used to make containers lightweight, secure, and fast.

🔟 What are distroless (or scratch) images?

Answer (as Abhishek explained):
Distroless images are ultra-minimal base images that don’t include shells, package managers, or any extra tools.
They contain only the runtime or binary your app needs.

For example:
If your app only needs Java runtime, that’s all the image has — no apt, no curl, no bash.

Benefits:

Smaller size (1 MB vs 100 MB for Ubuntu)

Fewer vulnerabilities

Faster startup

Better security

He mentioned examples like scratch (the smallest image) or Google’s distroless images.

11️⃣ What are some real-time challenges with Docker?

Answer (as Abhishek explained):

Docker daemon issues: It’s a single process and a single point of failure. If it goes down, builds and containers stop working.

Daemon runs as root: Security risk — if compromised, the attacker can access the whole host.

Resource constraints: If limits aren’t set, one container can consume all memory or CPU.

He also mentioned:
Tools like Podman solve these issues since Podman doesn’t require a root daemon and can run rootless containers.

12️⃣ What steps would you take to secure containers?

Answer (as Abhishek explained):
He gave a few strong points:

Use distroless or scratch images — no unnecessary packages → fewer vulnerabilities.

Network isolation: e.g., payments container on a separate bridge.

Scan images for CVEs using tools like Snyk or Trivy before pushing to production.

Run containers as non-root users.

Keep images minimal and use immutable tags.

This ensures your containers are secure both at build time and runtime
