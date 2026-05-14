# Digital Resource Alliance of Canada (DRAC) = Formerly Compute Canada

The **D**igital **Re**esource **A**lliance of **C**anada (**DRAC**) is
the national Canadian high-performance computing clusters available to
all researchers in Canada. They provide an invaluable resource for
managing large data science and AI projects.

The Digital Research Alliance of Canada (the Alliance) documentation
outlines the requirements for obtaining an account and the procedure
for linking your computational work to a specific Principal
Investigator (PI) or sponsor.

## Setting up an Alliance Account

Alliance accounts are established strictly on a **per-person** basis,
and account sharing is forbidden. If you are a student, employee, or
collaborator, you must obtain your own account under the sponsorship
of a PI.

The process generally involves:

1.  **Registration and Sponsorship:** You must go to the **CCDB**
	(Canadian Common DataBase) portal to register in your own
	name. During the web form process, you will need to use your
	sponsor's **CCRI** (Canadian Common Research Identifier) to
	indicate their sponsorship.
2.  **Confirmation:** The sponsor (the PI) will receive an email
	containing a link which they must click to confirm your
	sponsorship. PIs have no limit on the number of sponsored accounts
	they can have, provided the sponsorship is within the context of a
	genuine and sustained research collaboration.
3.  **Account Access Details:** Once registered, successful access to
	Alliance systems via Secure Shell (SSH) requires:
	*   Knowing your **username** (this is not your CCRI, CCI, or
		email address).
	*   Knowing your password (the same one used for CCDB) or using an
		SSH key (which is highly recommended for better security).
	*   Being registered for **multifactor authentication** and having
		your second factor available.

## Connecting to DRAC

To connect to the Digital Research Alliance of Canada (the Alliance)
clusters, you must use **Secure Shell (SSH)**, which is the standard,
secure, and encrypted method for executing commands, submitting jobs,
checking job progress, and transferring files.

Here is a description of how to use SSH to connect:

### Prerequisites for Connection

To connect successfully via SSH, you must have the following:

1.  **Machine Name:** You need to know the name of the machine you
	intend to connect to, which will look something like
	`narval.computecanada.ca` or `beluga.computecanada.ca`.
2.  **Username:** You must know your username, which is generally not
	your CCI, CCRI (`abc-123-01`), or email address.
3.  **Authentication:** You must either know your password (the same
	one used to log in to the CCDB portal) or have an **SSH
	key**. Using an SSH key is highly recommended for better security.
4.  **Multifactor Authentication (MFA):** You must be registered for
	multifactor authentication and have your second factor available.

### Using the SSH Command Line

On operating systems like **macOS** and **Linux**, the **OpenSSH**
client is typically installed by default. On recent versions of
**Windows**, SSH is available in PowerShell, the `cmd` prompt, or
through the Windows Subsystem for Linux (WSL).

To initiate the connection from a command-line client, use the `ssh`
command with your username and the machine name:

`$ ssh username@machine_name`

For Windows users, third-party clients such as PuTTY, MobaXTerm,
WinSCP, and Bitvise are also popular.

### First-Time Connection and Host Keys

The very first time you connect to a machine, the SSH client will ask
you to store a copy of the machine's **host key**. This unique
identifier allows your client to verify, upon subsequent connections,
that you are connecting to the intended machine.

### Enabling X11 for Graphical Applications

If you need to run graphical applications (like interactive jobs for
data exploration or certain software development) that rely on the X
protocol (X11), you must:

1.  Have an **X11 server** installed on your local computer (e.g.,
	XQuartz for macOS or MobaXterm/VcXsrv for Windows).
2.  Add the `-X` option to your `ssh` command to enable X11
	communications:

`$ ssh -X username@machine_name`

### Connection Errors

If you encounter connection errors like "remote host identification
has changed," this could indicate either a security upgrade on the
cluster or a man-in-the-middle attack. If this occurs, you should
verify that the host key fingerprint matches one of the published host
key fingerprints for the Alliance. If it matches, it is safe to
proceed; otherwise, you should terminate the connection and contact
technical support.

## Getting started on a project

Projects on Digital Research Alliance of Canada (the Alliance) systems
are managed through a comprehensive framework involving **Resource
Allocation Projects (RAPs)**, which define resource usage targets and
determine job priority via a scheduling system (Slurm).

Here is an explanation of how projects and resources are managed:

### 1. Resource Allocation Projects (RAPs)

A RAP is the fundamental unit for managing resources. Each RAP has a
unique project identifier (RAPI) and an associated group name.

There are two main types of RAPs:

1.  **Default RAP:** This is automatically created when a Principal
	Investigator (PI) role is activated. It manages default quotas for
	storage and cloud resources. The Default RAP uses the **Rapid
	Access Service (RAS)**, allowing opportunistic use of compute
	resources with the **lowest priority**. The group name typically
	follows the convention `def-profname`.
2.  **RAC RAP:** This RAP is created when a PI receives a compute or
	storage award through the peer-reviewed **Resource Allocation
	Competition (RAC)**, which includes Resources for Research Groups
	(RRG) and Research Platforms and Portals (RPP). The group name
	typically follows the convention `rrg-profname` or `rpp-profname`
	for HPC resources.

### 2. Allocations and Usage Targets

An **allocation** represents the amount of resources a research group
can target for use over a specific period, usually a year.

*   For storage, the allocation is a **maximum amount** that can be
	used exclusively.
*   For shared compute resources (CPU core years and GPU years), the
	allocation represents an **average amount of usage** over the
	period.

RAC projects have specific target usage levels determined by the
awarded CPU-years or GPU-years. Default (`def-`) projects share equal
target levels that are adjusted dynamically based on the number of
active projects on the cluster.

### 3. Job Submission and Project Association

Every job submitted to the cluster scheduler must be charged to a
specific RAP, which is specified using the `--account` argument in the
job script. If a user has access to more than one account, they must
explicitly use the `--account` directive to specify which project the
job belongs to.

PIs and their sponsored users can find their RAPs and corresponding
**Group Names** (used with `--account`) by logging into the CCDB
portal and navigating to *My Projects > My Resources and Allocations*.

### 4. Job Scheduling and Priority Management

Job scheduling is managed by the **Slurm Workload Manager** using the
**Fair Tree algorithm** to determine priority. The scheduler's goal is
to meet the allocation targets of all groups by calculating a job's
priority based on the group's recent usage compared to its allocated
usage.

#### "Billing" and Resource Equivalents

The scheduler calculates resource consumption not based on what the
application actually used, but on the resources requested, because
requested resources are unavailable to others.

1.  **Cores Equivalent:** For CPU jobs, usage is measured in core
	equivalents, defined as a bundle of a single core and an
	associated amount of memory, typically 4 GB per core on most
	clusters. If a job requests more memory than is associated with
	the cores requested (e.g., more than 4 GB/core), it will be
	charged based on the memory usage, effectively counting against
	more core equivalents.
2.  **Reference GPU Units (RGUs):** For GPU jobs, usage is measured
	using the **Reference GPU Unit (RGU)** system. RGUs standardize
	GPU performance, using the NVidia A100-40gb GPU as the reference
	model (assigned 4.0 RGUs). The scheduler charges the job based on
	the maximum usage dictated by the RGU, cores, or memory requested,
	according to predetermined bundle characteristics for each
	cluster.

#### Fair-Share Mechanism

The relationship between recent usage and the allocation target
determines the project's **LevelFS** (Fair-share value).

*   If a project uses less than its target, its LevelFS is greater
	than 1.0, and new jobs will receive high priority.
*   If a project uses more than its target, its LevelFS drops below
	1.0, resulting in low priority for subsequent jobs.

Overusing an allocation does not prevent a group from submitting or
running new jobs; it only results in future jobs having lower priority
temporarily.

### 5. Storage Management

Storage management is tied to the RAP. If a project receives a storage
allocation, a corresponding directory is created in the `/project`
filesystem, typically named `/project/<group-name>` (e.g.,
`/project/rrg-profname-ab`), where project files should be stored by
all sponsored users.

### 6. Managing Project Membership

PIs manage who can use their allocation by managing **RAP
Memberships**.

*   By default, all sponsored roles (students, employees,
	collaborators) registered under the PI have access to the RAC
	award.
*   PIs can control access by logging into the CCDB portal, navigating
	to *My Account > Manage RAP Memberships*, and adding or removing
	members using their CCRI. RAP membership defines the group of
	users authorized to submit jobs against the RAPI and share files
	within the same Unix group.

## Tying Your Work to a Specific PI or Sponsor

Your computational work (jobs) is tied to a sponsor or PI through an
associated **Resource Allocation Project (RAP)**. Likely, your PI
already has this configured, especially if you are apply for an
account through their lab.

1.  **Project Identification (RAP/Group Name):** Every job submitted
	to the scheduler must have an associated account name
	corresponding to a RAP. This account name is referred to as the
	**Group Name** and is provided as the value of the `--account`
	option when submitting jobs.
	*   **Default RAP:** For projects using the Rapid Access Service
		(RAS), the associated group name typically follows the
		convention `def-profname`. This allows opportunistic use of
		compute resources with the lowest priority.
	*   **RAC RAP:** For resources awarded through the Resource
		Allocation Competition (RAC), the group name typically follows
		the convention `rrg-profname` (Resources for Research Groups)
		or `rpp-profname` (Research Platforms and Portals).

2.  **Specifying the Account in Jobs:**
	*   If you are a member of only one account, the scheduler
		automatically associates your jobs with that account.
	*   If you have access to more than one account (due to multiple
		PI sponsorships or project memberships), you must use the
		`--account` directive in your job submission script (`sbatch`)
		to specify the desired account.
	*   For example, in a job script, you would include the line
		`#SBATCH --account=def-user-ab`. Jobs related to a RAC award
		should be submitted with the corresponding RAC group name
		(e.g., `--account=rrg-profname-ab`).

3.  **Finding Account Names:** To determine the correct account name
	(Group Name) corresponding to a specific RAP, you can log in to
	the CCDB portal and navigate to *My Projects > My Resources and
	Allocations*.

4.  **Priority Implications:** The account you use determines the
	priority of your job. The scheduler uses fair-share algorithms
	based on the research group's recent usage compared to its
	allocated usage. Jobs charged to a RAC RAP (starting with `rrg-`
	or `rpp-`) have target usage levels based on the RAC award, while
	non-RAC projects (`def-` accounts) all share equal, dynamic target
	levels.

## Getting Started

Accessing your sponsor's allocation for running computational jobs and
migrating data to that resource are distinct processes managed through
the Alliance's system.

***

### 1. Accessing Your Sponsor's Allocation to Run Jobs

To utilize the compute resources allocated to your Principal
Investigator (PI) or sponsor, your jobs must be explicitly linked to
their **Resource Allocation Project (RAP)** using the job scheduler,
Slurm.

1.  **Determine the Group Name:** Every RAP has an associated **Group
	Name** (also known as the account name).
	*   If your sponsor received an award through the Resource
		Allocation Competition (RAC), the Group Name will typically
		begin with `rrg-` (Resources for Research Groups) or `rpp-`
		(Research Platforms and Portals).
	*   If you are accessing the default allocation (Rapid Access
		Service, RAS), the account code begins with `def-`.
	*   You can find the correct Group Name for a given RAP by logging
		into the **CCDB** portal and navigating to *My Projects -> My
		Resources and Allocations*.

2.  **Specify the Account in Job Submission:** When submitting a job
	via the scheduler (`sbatch`), you must specify the account/group
	name using the `--account` directive.
	*   If you are a member of only one account, the scheduler will
		automatically associate your jobs with that account.
	*   If you have access to more than one group (due to multiple PI
		sponsorships), you **must** specify the desired account to
		ensure the job uses the correct allocation.
	*   For example, jobs relating to a RAC award should be submitted
		using the RAC group name: `#SBATCH --account=rrg-profname-ab`.

3.  **Impact on Priority:** Submitting a job using the RAC Group Name
	(`rrg-` or `rpp-`) means the job's priority will be determined by
	that project's allocation target. The scheduler calculates
	priority using the Fair Tree algorithm based on the research
	group's recent usage compared to its allocated usage.

***

### 2. Migrating Data to Your Sponsor's Allocation

Your sponsor's storage allocation, especially those granted through
RAC, is typically accessible in the `/project` filesystem. For
general-purpose clusters, this directory is structured as
`/project/<group-name>` (e.g., `/project/rrg-profname-ab`).

#### Recommended Transfer Tool

The Alliance strongly recommends using
**[Globus](https://globus.alliancecan.ca/file-manager?two_pane=true)**
for transferring substantial amounts of data to an Alliance
cluster. You have to sign into DRAC to use this tool.

*   Globus is the preferred tool for transferring data between systems
	due to its efficiency, reliability, and ability to recover from
	network interruptions automatically (a feature sometimes called
	"fire-and-forget" transfers).
*   Globus automatically uses **data transfer nodes** (data mover
	nodes), which should be used instead of login nodes whenever you
	transfer data.
*   When initiating a Globus transfer for synchronization, you can
	select options like checking file checksums, file size
	differences, or modification times to ensure only necessary files
	are transferred.

#### Data Migration Best Practices

Before migrating, you should prepare your data, especially if you are
dealing with large volumes or filesystems that handle many small files
poorly:

1.  **Clean Up and Archive:** Prior to migration, clean up files that
	can be deleted, such as core dumps (`core.12345`) or intermediate
	compilation files. Most transfer programs are more efficient
	moving one large file than thousands of small files. Use `tar` to
	combine (archive) directories or directory trees that contain many
	small files.
2.  **Small File Collections:** For datasets containing large numbers
	of small files (e.g., hundreds of thousands of small images common
	in machine learning), problems can arise due to filesystem
	quotas. These data should be stored in **large single-file
	archives** on the distributed filesystem.
3.  **Storage Location Considerations:**
	*   **Shared Storage (`/project` or `/scratch`):** This shared
		storage is intended for storing and reading at low frequencies
		(e.g., one large chunk every 10 seconds, rather than 10 small
		chunks every second). You can leave datasets permanently in
		your project space.
	*   **Local Compute Node Storage (`$SLURM_TMPDIR`):** If your
		dataset is 100 GB or less, you may transfer it to the local
		storage of the compute node at the beginning of the job using
		the `$SLURM_TMPDIR` temporary directory. This storage is
		generally orders of magnitude faster and more reliable than
		shared storage.
4.  **Using Rsync for Transfers:** If you use `rsync` to transfer
	files into the `/project` filesystems, you must avoid preserving
	group ownership to prevent "Disk quota exceeded" errors. You
	should add the `--no-g --no-p` options when using `rsync` to the
	remote project directory.
5.  **Avoiding SCP Recursively:** It is recommended **against using
	`scp -r`** to transfer data into `/project` because it can turn
	off the `setgid` bit, potentially leading to errors if files are
	subsequently created there.
6.  **Verify Data Integrity:** After migration (especially if you did
	not use Globus with the file integrity verification option), you
	should verify the data is not corrupted, potentially by comparing
	file sizes or using utilities like `cksum` or `md5sum`.

## Long term data storage

Storage quotas on DRAC systems limit both the total volume of data
(disk space) and the total number of filesystem objects (inodes) a
project can use. This is particularly important when dealing with
datasets composed of many small files, common in fields like machine
learning.

### Storage Quotas and Inodes on Alliance Systems

The Alliance clusters utilize a distributed filesystem, where storage
is typically managed in shared locations like `/project` and
`/scratch`.

1.  **Storage Quota (Disk Space):** Allocations for storage are
	managed by the **Resource Allocation Project (RAP)** and represent
	the **maximum amount** of storage space a research group can use
	exclusively.
2.  **Filesystem Quotas (Inodes):** Beyond limiting total disk space,
	Alliance cluster filesystems also limit the **number of filesystem
	objects** that a project can store. Filesystem objects include
	files, directories, and other metadata structures, often referred
	to as **inodes**.

### Problems Caused by Small Files

When managing datasets that contain a **very large collection of small
files** (e.g., hundreds of thousands or more, such as image datasets
often found in machine learning), two primary problems arise:

1.  **Quota Violation:** Collections of small files can quickly
	exhaust the **filesystem quotas** (inode limits) on the clusters,
	even if the total disk space used is modest.
2.  **Performance Degradation:** Streaming lots of small files from
	shared storage like `/project` or `/scratch` to a compute node can
	significantly slow down your software due to the inherent
	performance implications of distributed filesystems. Shared
	storage is designed for storing and reading at low frequencies
	(e.g., 1 large chunk every 10 seconds, rather than 10 small chunks
	every second).

### Mitigating Quota and Performance Limits

To effectively manage these limitations, especially when dealing with
large collections of small files, the Alliance recommends strategies
focused on archival and local storage usage:

#### 1. Archiving Small Files (Tarring Data)

The primary method for mitigating inode limits and improving I/O
performance is to combine collections of small files into **large
single-file archives**.

*   **Efficiency:** Most file transfer programs move one large file
	much more efficiently than thousands of small files of equal total
	size.
*   **Method:** You should use `tar` to combine (archive) directories
	or directory trees containing many small files.
*   **Location:** Once archived, this consolidated data should be
	stored in your shared project space (e.g.,
	`/project/<group-name>`).

#### 2. Utilizing Local Node Storage for Smaller Datasets

For improved I/O performance, especially during machine learning
tasks, you should avoid reading data continuously from the shared
distributed filesystem.

*   **Size Limit:** If your dataset is around **100 GB or less**, it
	is recommended to **transfer it to the local storage of the
	compute node** at the beginning of the job.
*   **Performance Benefit:** This local storage is typically orders of
	magnitude faster and more reliable than shared storage (home,
	project, scratch).
*   **Directory:** A temporary directory is available for each job at
	`$SLURM\_TMPDIR$`, which is the location where this data transfer
	should occur.

#### 3. General Data Migration Clean-Up

Before migrating or transferring data, performing general cleanup
reduces the strain on quotas and speeds up the migration process:

*   **Clean Up:** Delete unnecessary files, such as core dumps (e.g.,
	`core.12345`) or intermediate compilation files, as moving less
	data takes less time.
*   **Compression:** Large files, especially text files, may benefit
	from compression (using `tar` or `gzip`) before transfer, although
	this requires balancing compression time against bandwidth
	savings.

#### 4. Avoiding SCP for `/project`

When transferring files to the `/project` filesystems, it is
recommended **against using `scp -r`** (recursive copy), as this can
turn off the `setgid` bit, potentially leading to errors such as "Disk
quota exceeded" if files are subsequently created within those
directories.

# References

- https://docs.alliancecan.ca/wiki/Technical_documentation
- https://docs.alliancecan.ca/wiki/Bioinformatics
- https://docs.alliancecan.ca/wiki/Humanities_and_Social_Sciences
- https://docs.alliancecan.ca/wiki/AI_and_Machine_Learning
- https://docs.alliancecan.ca/wiki/General_directives_for_migration
