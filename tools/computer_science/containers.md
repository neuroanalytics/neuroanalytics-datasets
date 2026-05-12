# Containers: Docker and Apptainer

The fundamental problems that containerization tools like Docker and
Apptainer (formerly Singularity) solve for researchers primarily
revolve around **reproducibility**, **dependency management**, and
**environment consistency**.

## What are containers?

### 1. Ensuring Reproducibility and Managing Dependencies

One of the central problems solved by containerization is the
difficulty of replicating software environments, both for sharing and
for future use.

*   **Documenting the Software Environment:** Containerization
	mandates the explicit documentation of the software
	environment. This documentation is essential for sharing
	computational results with colleagues and for the researcher
	themselves when revisiting a project later (even just three months
	or days later).
*   **Handling Dependencies and Conflicts:** Containers isolate
	environments to manage different software requirements. They solve
	the problem of changing dependencies causing unexpected behavior,
	which often occurs when updating an existing project or when two
	projects share an environment but require different versions of
	the same dependencies.
*   **Capturing the Entire Compute Environment:** Traditional tools
	like Python virtual environments are insufficient because they do
	not capture non-Python or non-R binary dependencies, or critical
	dependencies related to the Operating System (OS) and its package
	manager. Containers are necessary because they capture the
	**entire compute environment**.
*   **Portability and Consistency:** Containerization ensures a
	**consistent** environment across all deployments, thus
	eliminating compatibility issues. A container is self-contained,
	meaning it includes everything it needs to run without relying on
	pre-installed dependencies on the host machine. This makes the
	research environment portable—a container running on a development
	machine will work the same way in a data center or in the cloud.

### 2. Isolation and System Integrity

Containerization solves problems related to maintaining a clean and
functional host system:

*   **Protecting the Host System:** Containers provide an isolated
	environment that prevents researchers from needing to install
	software directly into their host system, such as installing
	packages into their system Python, which is generally advised
	against.
*   **Access to Unavailable Tools:** Containers allow researchers to
	run tools and environments (like a specific Conda installation)
	even if those tools are not installed on the host machine. A
	container provides an environment that looks like a separate
	computer from the inside.

### 3. Solving Security and Deployment Challenges in High-Performance Computing (HPC)

For researchers utilizing supercomputers or shared clusters, Apptainer
(Singularity) addresses specific security and deployment hurdles that
traditional container solutions like Docker cannot:

*   **Security Risks on Shared Systems:** Docker is generally not used
	on High-Performance Computing (HPC) clusters because it creates
	security risks. By default, Docker runs with root privileges
	inside the container and is not as isolated as a Virtual Machine
	(VM). This setup creates a risk where a malicious actor could
	escalate privileges and "break out" of a container.
*   **HPC Deployment:** **Apptainer (Singularity)** is the container
	solution designed for HPCs and shared systems where Docker cannot
	or should not be used. This allows researchers to reliably run
	containers built with tools like Docker on a compute cluster using
	Apptainer.
*   **Working with Host Filesystems:** Containers, by default, are
	isolated and cannot see or write to the filesystem of the
	host. Tools like Docker and Apptainer solve this issue by allowing
	researchers to **mount** directories or paths on the host into the
	container (known as bind mounts or volumes) so that data can be
	accessed and results can be stored persistently outside of the
	ephemeral container layer.


## Getting Started with Docker

The basic process for working with Docker involves interacting with
two main components: **Docker Images** (read-only snapshots of the
environment) and **Docker Containers** (live instances of those
images). This process can generally be broken down into three phases:
running existing containers, building custom images, and managing data
persistence.

### 1. Running Existing Containers (Using Images from a Registry)

The most common starting point is running a container based on an
image found in a repository like **Docker Hub**. The Docker Engine, a
command line program, is used to manage these steps.

1.  **Find the Image:** Locate the desired Docker image (often
	specifying an image tag or version) on a central registry, such as
	Docker Hub.
2.  **Retrieve the Image (Pull):** You can retrieve the image using
	the `docker pull` command. Alternatively, the necessary image
	layers are often downloaded automatically if the image is not
	found locally when executing the run command.
3.  **Run the Container:** Use the `docker run` command to create and
	start a live instance (a container) from the image.
	*   Example syntax: `docker run [options-for-docker] <imagename>
		<command>`.
	*   Using `docker run -it` allows connecting to an **interactive
		shell** inside the container.
	*   You can configure services inside the container, such as
		mapping a port on the host machine to a port inside the
		container using the `-p` option, for example: `-p 8889:8888`.
4.  **View and Stop:** Running containers can be viewed using the
	`docker ps` command. To stop a running container, you can use the
	`docker stop <container-id>` command or press `Ctrl+C` if it was
	run interactively.

### 2. Creating Custom Images

Researchers often need to create customized environments. This is
achieved by writing a Dockerfile and building a new image.

1.  **Define the Environment (Dockerfile):** A **Dockerfile** is a
	text file that contains instructions detailing how to assemble the
	image.
	*   A Dockerfile **must begin with the `FROM` instruction**, which
		specifies the base image (the foundation of the environment).
	*   Instructions like `RUN` are used to execute commands (e.g.,
		installing packages or compiling code) to create a new layer
		on top of the base image.
	*   Instructions like `COPY` or `ADD` are used to copy files or
		directories from the local machine (the build context) into
		the image filesystem.
2.  **Build the Image:** Use the `docker build` command, typically
	with the `-t` option to assign a name and tag to the resulting
	image.
	*   Example: `$ docker build -t my-jupyter-image .`.
3.  **Share the Image:** After building, the custom image can be
	shared by pushing it to a repository like Docker Hub using the
	`docker push` command.

### 3. Managing Data and Persistence

By default, the container has its own isolated filesystem, and any
files written directly into the container are tied to that instance
(meaning they are lost when the container is deleted). To work with
external data or save results persistently, you must connect the
container to the host filesystem.

*   **Bind Mounts:** Researchers frequently use **bind mounts** (or
	volumes) to access local data on the host machine from inside the
	container. This allows data to be used by the tool running in the
	container.
	*   This is achieved using the `-v` option during `docker run`.
	*   The syntax specifies the host path and the target container
		path: `docker run -v /host/path:/container/path`.
*   **Volumes:** **Volumes** are the preferred mechanism for
	persisting data used by Docker containers, as they are managed
	entirely by Docker and are not dependent on the host machine's
	directory structure. To use a volume, the `-v` option is used to
	name the volume and mount it to a path inside the container.

## Build your own container

The basic steps in building a Docker container involve creating a
**Dockerfile** to define the environment, running the **Docker build**
command to generate a **Docker image** from that file, and then
running the image to create a live **Docker container**.

Here is a breakdown of the steps for building and using a custom
Docker container:

### 1. Defining the Environment with a Dockerfile

The core of building a custom container is defining its environment
using a text file called a **Dockerfile**. The Dockerfile contains
instructions that detail how to assemble the image.

*   **Create the Dockerfile:** In a chosen directory (often the
	context directory for the build), create a text file named
	`Dockerfile`.
*   **Specify a Base Image (`FROM`):** A Dockerfile **must begin**
	with the `FROM` instruction, which specifies the base image (the
	starting point or foundation for the new environment). For
	example, a data science environment might start `FROM
	quay.io/jupyter/base-notebook`.
*   **Add Instructions (`RUN`, `COPY`, etc.):** Subsequent
	instructions execute commands to customize the environment by
	adding files, installing packages, or configuring settings.
	*   The `RUN` instruction executes commands to create a new layer
		on top of the current image, such as installing packages
		(e.g., `RUN pip install ...`).
	*   The `COPY` or `ADD` instructions copy new files or directories
		from the build context (your local machine) into the
		filesystem of the image.
	*   The `ENV` instruction sets environment variables that persist
		in the container.
*   **Example:** If you need specific packages like `matplotlib` and
	`scikit-learn`, your Dockerfile would use the `FROM` instruction
	for the base and the `RUN` instruction to install the dependencies
	using `pip`.

### 2. Building the Docker Image

Once the Dockerfile is ready, you use the Docker Engine to build the
image.

*   **Execute the Build Command:** Use the `docker build` command from
	the terminal in the directory where the `Dockerfile` is located.
*   **Tag the Image (`-t`):** Use the `-t` option to assign a name and
	a tag to the resulting image, such as `my-jupyter-image`.
	*   Example: `$ docker build -t my-jupyter-image .`. The `.`
		indicates that the current directory is the context for the
		build.
*   **Result:** This process creates a **Docker image**, which is a
	read-only snapshot of the defined environment.

### 3. Running the Image as a Container

The custom image can then be launched as a live container.

*   **Run the Container Command:** Use the `docker run` command,
	specifying your newly created image name.
	*   Example: `$ docker run --rm -p 8889:8888 my-jupyter-image
		<command>`.
*   **Access the Environment:** The running instance is the **Docker
	container**—an isolated, live instance of the image. Any
	modifications made during runtime are saved in a temporary
	writable layer that is discarded when the container is deleted
	unless external data storage is used.

### 4. Sharing the Built Image

After building the image, you can share it by pushing it to a registry
like **Docker Hub**.

*   **Tag the Image for Registry:** Rename (tag) the image using
	`docker tag` to include your Docker ID so the registry knows where
	to place it.
*   **Push the Image:** Use `docker push` to upload the image to
	Docker Hub. Other users can then retrieve and run your image.

# Dockerhub - Sharing Containers

**Docker Hub** is a crucial component of the Docker ecosystem, serving
as a **central repository** for container images.

Key facts about Docker Hub:

*   **Function and Identity:** Docker Hub is described as a **website
	/ web service**. Its primary role is to act as a central
	repository for storing and sharing Docker container images. It is
	noted as a commercial service.
*   **Accessing Images:** Researchers and developers can utilize
	Docker Hub to find and retrieve existing container images.
	*   You can retrieve (pull) an image from Docker Hub using the
		`docker pull` command.
	*   The `docker run` command will also automatically pull and run
		an image if it is not found locally.
	*   Docker Hub offers a **vast library of pre-built images and
		resources**, which can accelerate development workflows and
		reduce setup time.
	*   Trusted content, such as **Docker Official Images** and images
		from Verified Publishing Partners, are available for security
		and quality assurance.
*   **Sharing and Distribution:** Docker Hub allows users to share and
	distribute their own custom container images.
	*   After building a custom image, you can push it to Docker Hub
		using the `docker push` command. This process creates a
		repository automatically if one doesn't already exist.
	*   Sharing an image through Docker Hub creates a portable and
		replicable research environment that can be easily accessed
		and used by other users, ensuring work is preserved without
		compatibility issues.
*   **Image Structure:** Docker images, which are read-only snapshots
	of an environment, can be stored on Docker Hub. Images stored
	there typically have **tags** (or versions) that users should
	specify when retrieving them.
*   **High-Performance Computing (HPC) Context:** While Docker itself
	is generally not used on High-Performance Computing (HPC) clusters
	due to security risks, Apptainer (formerly Singularity) is
	designed for HPCs and can pull images directly from Docker Hub to
	create a local Apptainer container file.
*   **Alternative Registries:** While Docker Hub is the most
	recognized, other container image registries also exist. You can
	use URIs beginning with `docker://` to build an Apptainer image
	from Docker Hub.

## Apptainer - using containers on Compute Canada and HPC

Your familiarity with Docker provides an excellent starting point for
using **Apptainer** (formerly Singularity) on a High-Performance
Computing (HPC) system, as Apptainer is specifically the container
solution used in environments where Docker is often unavailable or
prohibited due to security concerns.

The transition involves understanding that while you typically *build*
your environment using Docker locally, you must *run* it using
Apptainer on the shared HPC resource.

Here is a step-by-step guide detailing how to get started with
Apptainer, drawing parallels to Docker concepts where applicable:

### 1. Initial Setup and Access

Before running any commands, you generally need to load the Apptainer
software module on the HPC cluster:

*   **Load the Module:** To use the default version of Apptainer
	available, run: `$ module load apptainer`.

### 2. Image Acquisition: Converting Docker Images to Apptainer Files (SIF)

On HPC systems, you generally cannot run Docker directly, but
Apptainer can easily convert images from Docker Hub into its native
format, the **Singularity Image File (SIF)**, which is a compressed,
read-only file.

| Docker Command/Action | Apptainer Equivalent | Description |
| :--- | :--- | :--- |
| **Pulling an Image** (`docker pull`) | **Building an Image** (`apptainer build`) | You use the `apptainer build` command to pull the Docker layers from Docker Hub (or another OCI registry) and assemble them into a local SIF file. |
| **Example:** | `$ apptainer build bb.sif docker://busybox` | This command **pulls** the `busybox` image layers from Docker Hub and saves the resulting container image as a local SIF file named `bb.sif`. |
| **Building from a Dockerfile** (Locally) | **Using the Dockerfile URI (Off-Cluster)** | If you only have a Dockerfile, you must generally build the Docker image *on a system where you have appropriate privileges* (like your local machine), save it as a tarball, and then convert that tarball to a SIF file. Note that building images that require root/sudo access usually **will not work on compute clusters**. |

### 3. Running the Container

Once you have a SIF file (e.g., `myimage.sif`), you use Apptainer
commands instead of `docker run`.

| Docker Goal/Command | Apptainer Command | Description |
| :--- | :--- | :--- |
| **Run a command in the container** (`docker run`) | `apptainer run` | Runs the container using the defined runscript. We suggest you always use `apptainer run`. |
| **Execute a specific command** (`docker run <image> <command>`) | `apptainer exec` | Executes a command inside the container without running the defined runscript. |
| **Connect to an interactive shell** (`docker run -it`) | `apptainer shell` | Allows you to run commands interactively while remaining inside the container, typically resulting in an `Apptainer>` prompt. |

### 4. Handling Files and Persistent Data (Bind Mounts)

In Docker, you use the `-v` or `--mount` option to expose directories
on the host to the container filesystem. Apptainer uses the `-B` or
`--bind` option for bind mounts.

*   **Bind Mount Syntax:** In Docker, you use `-v
	/host/path:/container/path`. In Apptainer, the standard option is
	`-B <host_path>[:<container_path>]`.
*   **HPC Filesystems:** On clusters, you will need to explicitly
	mount the filesystems where your data resides, such as `/project`
	or `/scratch`, to make them accessible inside the container.
	*   Example: `apptainer run -B /project -B /scratch myimage.sif
		<command>`.
*   **Home Directory Caution:** If you need to bind-mount the `/home`
	filesystem, it is recommended to use an alternate destination
	directory inside the container (e.g., `-B /home:/cluster_home`) to
	prevent configuration files and programs in your host home
	directory from interfering with the container software.

### 5. Essential Command Line Options for HPC Systems

When running Apptainer on a shared cluster, you need to add specific
flags, usually following the `run`, `shell`, or `exec` command, to
ensure security and resource management:

*   **Environment Isolation (`-C` or `-e`):** Apptainer, by default,
	adopts the shell environment of the host, which can cause issues.
	*   Use `-C` (contain) to isolate the running container from **all
		filesystems** (requiring explicit bind mounts like `-B
		/project`).
	*   Alternatively, `-e` cleans (some) shell environment variables
		before running and applies settings for increased Docker
		compatibility.
*   **Temporary/Working Directory (`-W`):** Since `/tmp` and similar
	filesystems often use limited RAM on clusters, you should use the
	`-W` or `--workdir` option to specify a disk location for
	temporary files to prevent jobs from being killed.
	*   On systems using the Slurm scheduler, use the assigned
		temporary directory variable: `-W ${SLURM\_TMPDIR}`.
*   **GPUs:** If the software inside your container requires GPUs
	(NVIDIA or AMD), you must pass the appropriate flag when running:
	*   Use `--nv` for NVIDIA hardware.
	*   Use `--rocm` for AMD hardware.

### Summary of Workflow Translation

| Docker Concept | Apptainer Approach (HPC) | Typical Command Component |
| :--- | :--- | :--- |
| Build (Dockerfile) | Build/Convert to SIF (locally or on head node) | `apptainer build <name>.sif docker://<image>` |
| Run (Command) | Run or Execute | `apptainer run/exec` |
| Bind Mount (`-v`) | Bind Mount | `-B /host/path:/cont/path` |
| Interactive (`-it`) | Shell | `apptainer shell` |
| Security/Isolation | Containment | `-C` or `-e` |
| Temp Files | Dedicated Work Directory | `-W ${SLURM\_TMPDIR}` |

## References

- https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-container/
- https://docs.docker.com/docker-hub/quickstart
- https://docs.docker.com/reference/dockerfile/
- https://docs.docker.com/desktop/
- https://docs.docker.com/guides/jupyter/

- https://docs.alliancecan.ca/wiki/Apptainer
- https://apptainer.org/docs/user/main/build_a_container.html
- https://apptainer.org/docs/user/main/persistent_overlays.html
