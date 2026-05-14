# Submitting work to an HPC Cluster

To submit an analysis (or job) to run on a Digital Research Alliance
of Canada (the Alliance) cluster, you must use the **Slurm Workload
Manager** scheduler via the `sbatch` command, ensuring that you
properly define the job's resource requirements and associate it with
a specific Resource Allocation Project (RAP). Other schedulers exist,
but Slurm is by far the most common.

The process generally involves three main steps: preparation, job
script creation, and submission.

## 1. Preparation and Environment Setup

Before submitting a job, you must ensure your environment and data are
ready:

### A. Connect to the Cluster
You must connect to a cluster's login node securely using **Secure
Shell (SSH)**. The connection requires your username and password (or
SSH key) and availability of your second factor for multifactor
authentication. You should know the name of the machine you are
connecting to, such as `fir.alliancecan.ca` or
`trillium.alliancecan.ca`.

### B. Modules - Access Required Software
Software needed for your analysis is typically accessed using the
**module system** (Lmod).

Using modules is the standard way to access installed software on
Digital Research Alliance of Canada (the Alliance) clusters.

#### Description of Modules

Modules are configuration files that contain instructions for
modifying your software environment. This modular architecture enables
the system to support multiple versions of the same application
without conflicts.

1.  **Function:** A module file modifies or initializes environment
	variables, such as `PATH` and `LD\_LIBRARY\_PATH$, to make an
	application or library available in the user’s login session.
2.  **Management Tool:** On newer Alliance servers, modules are
	managed using the **Lmod** tool, which is designed to be very
	similar to the older *Environment Modules* system.
3.  **Hierarchy:** Modules operate under a **hierarchy system**. This
	means that the modules available to load often depend on "parent"
	modules you have already loaded (like compilers or MPI
	implementations). The hierarchy prevents naming conflicts when
	supporting many versions of complex software.

#### Key Usage and Best Practices

To integrate modules into your analysis workflow, the Alliance
recommends specific practices, especially when submitting
computational jobs:

*   **Loading:** Use the `module load <module-name>` sub-command to
	access a specific program or library version (e.g.,
	`gcc/9.3`). Loading a module does not execute the software, but
	makes the program binary accessible.
*   **Environment Consistency (Purge):** You should generally **avoid
	loading modules automatically** in your `~/.bashrc` file. To
	prevent inherited environment settings from your submitting shell
	from causing problems, it is best practice to include the line
	`module purge` in your job script before loading all the required
	modules to ensure a consistent environment state.
*   **Module Collections:** To repeatedly load a specific set of
	modules (e.g., for a complex analysis environment like machine
	learning or bioinformatics), you can create and restore a **module
	collection** using the `module save` and `module restore`
	sub-commands.
*   **Finding Modules:**
	*   Use `module list` to see what is currently loaded.
	*   Use `module avail` to list modules available to load.
	*   Use `module spider` to search the **complete tree** of all
		modules, including those that are incompatible or hidden due
		to the module hierarchy. This command is often needed to see
		how to load a specific version that has prerequisites.
*   **Bioinformatics Software:** Many specialized bioinformatics tools
	are available as modules. These tools are often categorized as
	`bio` in the available software list.

*   It is generally recommended **not to load modules automatically**
	in your `~/.bashrc`, but instead to load them directly in your job
	scripts.
*   You can use the `module purge` command at the beginning of your
	script to ensure a consistent environment state before loading the
	required modules.
*   If you need a specific set of modules repeatedly, you can create a
	**module collection** using the `module save` sub-command.

#### C. Manage Datasets (Especially for large jobs like ML/AI)
The cluster uses a distributed filesystem, and accessing data from
`/project` or `/scratch` has different performance implications than
local storage.

*   **For datasets $\leq 100\text{ GB}$:** You should transfer the
	data to the **local storage of the compute node** at the beginning
	of the job, as this storage is orders of magnitude faster and more
	reliable than shared storage. A temporary directory for this
	purpose is available at `$SLURM\_TMPDIR$.
*   **For large datasets with many small files (e.g., image
	datasets):** These collections can cause issues with filesystem
	quotas and significantly slow down software when streaming from
	shared storage (`/project` or `/scratch`). Such data should be
	stored in **large single-file archives** on the distributed
	filesystem.

### 2. Creating the Job Script

All computational work (jobs) must be submitted via the scheduler,
with exceptions only for small tasks not expected to consume more than
about 10 CPU-minutes or 4 GB of RAM (which can run on a login node).

A minimal Slurm job script (`simple\_job.sh`) requires executable
commands and directives prefixed with `#SBATCH`.

#### Key Directives to Include:

For example, many of my job scripts have something like this at the
top:

```
#!/bin/bash

#SBATCH -J extract-data
#SBATCH --time=00:15:00
#SBATCH --account=rrg-jbpoline
#SBATCH --cpus-per-task=1
#SBATCH --mem-per-cpu=8G
#SBATCH -o ./logs/extract-data-%j.out
#SBATCH -e ./logs/extract-data-%j.err
```

The most important feature are:

1.  **Account/Project Association (`--account`):** Every job must be
	charged to a specific **Resource Allocation Project (RAP)**.
	*   You must specify the account name (Group Name) using the
		`--account` directive. This account determines job priority
		based on the associated group's target usage.
	*   Example: `#SBATCH --account=def-someuser`.
2.  **Time Limit (`--time`):** Our policies require you to supply at
	least a time limit for each job. Shorter jobs have more scheduling
	opportunities, and jobs with time limits under three hours benefit
	most from backfilling.
	*   Example formats: `00:15:00` (15 minutes) or `0-0:5` (5
		minutes).
3.  **Resource Request (Cores/Memory/GPUs):**
	*   For CPU jobs, specify the number of tasks (`--ntasks`) and
		memory per CPU (`--mem-per-cpu`).
	*   If you don't specify memory, a default of $256\text{ MB}$ per
		core is allocated on general-purpose clusters.
	*   If you request memory beyond what is bundled per core
		(typically $4\text{ GB}$ per core), you will be charged for
		more **core equivalents**.
	*   For GPU jobs, you must follow specific instructions, typically
		requesting resources using the `--gres` directive (see the
		*Using GPUs with Slurm* documentation).

#### Job Arrays

Job arrays, also known as task arrays, are a powerful feature of the
Slurm Workload Manager used on the Digital Research Alliance of Canada
(the Alliance) clusters. They allow you to submit an entire set of
similar jobs with a single command, which is particularly useful for
workflows involving **running many similar jobs**.

##### How Job Arrays Work

A job array submits multiple instances of the same job script. The
individual jobs within the array are differentiated by an environment
variable: `$SLURM\_ARRAY\_TASK\_ID$`. This variable is set to a unique
value for each instance (task) of the job.

Job arrays are beneficial in situations such as:

*   **Hyperparameter search**.
*   **Training many variants of the same method**.
*   **Running many optimization processes of similar duration**.
*   **Restarting long computations** (checkpointing).

##### Creating and Submitting a Job Array

You define a job array using the `#SBATCH --array` directive in your
submission script.

1. Defining the Array Range

The `--array` directive specifies the range of task IDs that will be
executed.

For example, to create 10 tasks with `$SLURM\_ARRAY\_TASK\_ID$`
ranging from 1 to 10, you would use: `#SBATCH --array=1-10`

2. Example Job Array Script

In your job script, you use the `$SLURM\_ARRAY\_TASK\_ID$` variable to
control the execution path or input files for each distinct task.

A minimal example script (`array\_job.sh`) demonstrating 10 tasks running the application `./myapplication` is:
```bash
#!/bin/bash
#SBATCH --account=def-someuser
#SBATCH --time=0-0:5
#SBATCH --array=1-10
./myapplication $SLURM_ARRAY_TASK_ID
```
The application `./myapplication` would run 10 times, receiving task IDs from 1 through 10 as an argument.

#### 3. Job Limits

Note that on clusters like Graham and Béluga, there may be a limit on
the number of jobs you can have in the system at one time (e.g., 1000
jobs, queued and running). **Each task of a job array counts as one
job** towards this limit.

### Using Job Arrays for Long-Running Computations (Restarts)

Job arrays can also be used to automatically restart computations that
exceed the maximum time limit of the cluster (up to 7 days on
general-purpose clusters like Fir, Narval, Nibi, and Rorqual). This
requires the application to support **checkpointing** (saving state to
a file).

The `--array=1-100%10` syntax can be used to submit a collection of
identical jobs with the condition that **only one job of the array
will run at any given time**. The script should be designed to ensure
that the last checkpoint file is used to restart the calculation for
the next job.

For example, splitting a simulation into 10 sequential chunks using a
job array, ensuring only one runs at a time: `#SBATCH --array=1-10%1`

The script would use a test to check for the existence of a checkpoint
file (`state.cpt`) to either restart the simulation or start a new
one.

### Further Documentation

For more detailed documentation, you should refer to the documentation
on **Job arrays** and **Job Array Support**. Automating job submission
via job arrays is one technique provided, alongside tools like
**META**, **GLOST**, and **GNU Parallel**, which can bundle many short
computations into fewer tasks of longer duration to improve
computational efficiency.
#### Optimizing Long-Running Analysis

If your analysis is expected to run longer than the cluster time
limits (up to 7 days on general-purpose clusters), you must use
**checkpointing** to save your state and resume later.

*   For example, a 3-day training job should be split into 24-hour
	chunks.
*   This can be automated using **job arrays** (for a fixed number of
	restarts) or by implementing **resubmission from the job script**.

### 3. Submitting and Monitoring the Job

The primary way to interact with jobs on Digital Research Alliance of
Canada (the Alliance) clusters is through the **Slurm Workload
Manager** commands.

Here are the basic commands for submitting, monitoring, and
controlling your jobs:

#### Submitting Jobs

The command to submit a job script to the scheduler is `sbatch`.

*   **`sbatch <script_name>`**: Submits a batch job script (e.g.,
	`simple\_job.sh`) to the Slurm scheduler. This script must include
	`#SBATCH` directives specifying necessary resources like `--time`
	(time limit) and `--account` (Resource Allocation Project/Group
	Name).
*   **`sbatch --time=00:30:00 simple_job.sh`**: You can override
	directives in the script by supplying them as command-line
	arguments to `sbatch`.

#### Monitoring Running and Pending Jobs

You can check the status of jobs using two primary commands: `squeue`
or the local customization `sq`. It is important **not to run these
commands from a script** at a high frequency, as they add load to
Slurm.

*   **`sq`**: Lists only your own jobs.
*   **`squeue -u $USER$`**: The general Slurm command to list only
	your jobs, filtering by your username.
*   **`squeue -u <username> -t RUNNING`**: Shows only jobs currently
	in the **R** (Running) state for a specific user.
*   **`squeue -u <username> -t PENDING`**: Shows only jobs currently
	in the **PD** (Pending) state for a specific user.
*   **`scontrol show job <jobid>`**: Shows detailed information for a
	specific job ID.

#### Cancelling Jobs

To terminate or remove a job from the queue:

*   **`scancel <jobid>`**: Cancels a specific job using its job ID.
*   **`scancel -u $USER$`**: Cancels all jobs belonging to your user
	ID.
*   **`scancel -t PENDING -u $USER$`**: Cancels all pending jobs
	belonging to your user ID.

#### Viewing Completed Job Information

Once a job is complete, you can review its performance:

*   **`seff <jobid>`**: Provides a short summary of the CPU and memory
	efficiency of a job, including the wall-clock time and efficiency
	percentages.
*   **`sacct -j <jobid>`**: Provides more detailed information about a
	completed job. You can specify the output format using the
	`--format` option, for example: `$ sacct -j <jobid>
	--format=JobID,JobName,MaxRSS,Elapsed$.

#### Running Interactive Jobs

Interactive sessions on compute nodes are useful for tasks like
debugging, data exploration, or compiling software:

*   **`salloc`**: Starts an interactive session on a compute node,
	reserving resources for the duration specified.
*   Example: `$ salloc --time=1:0:0 --mem-per-cpu=3G --ntasks=1
	--account=def-someuser$.

#### Attaching to a Running Job

You can connect to the node running a job to monitor or troubleshoot
(e.g., running `htop` or `nvidia-smi`):

*   **`srun --jobid <jobid> --pty <command>`**: Runs a process on the
	node assigned to the specified running job ID.

#### Submission
Submit your job script using the `sbatch` command: `$ sbatch
simple_job.sh`

#### Monitoring
*   **Checking Status:** You can list your submitted jobs using the
	customized utility `sq` or the general command `squeue -u
	$USER$. The status column (`ST`) shows if a job is `PD` (pending)
	or `R` (running).
*   **Checking Output:** Output is typically written to a file named
	`slurm-<job ID>.out` in the submission directory. Output from
	non-interactive jobs is normally **buffered**, which means there
	may be a delay before you see the log file updated. If real-time
	monitoring is needed, run an **interactive job** using `salloc`.
*   **Efficiency:** After completion, you can check the efficiency of
	the job's CPU and memory utilization using `seff <jobid>`.
*   Visualizing Job Status with **Portail**: By accessing
	https://portail.narval.calculquebec.ca/, you can recieve
	visualizations about the resource usage of your job during its
	processing. You will also recieve pretty clear warnings about not
	utilizing a reasonable proportion of your requested resource if
	you do not use enough of it. This is a *very* convenient resource
	compared to many CLI resource tools.
