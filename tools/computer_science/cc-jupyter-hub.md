# Using JupyterHub on DRAC

JupyterHub sessions are indeed deployed as interactive jobs on the
compute cluster, but this interactive usage pattern has several key
differences and limitations compared to typical High-Performance
Computing (HPC) batch job submissions (`sbatch`).

## Deploying JupyterHub as an Interactive Job

You can deploy and use JupyterHub interactively on the
clusters. Accessing resources via a Jupyter server is equivalent to
accessing them through an **interactive job** on the corresponding
cluster.

1.  **Job Submission:** When a user starts a new session on
	JupyterHub, the system automatically submits a new interactive job
	on the user's behalf to the cluster scheduler.
2.  **Available Clusters:** JupyterHub is available on several
	general-purpose clusters, including Fir, Narval, and Rorqual, and
	was previously available on Béluga.
3.  **Intended Use:** JupyterLab and notebooks are designed for
	**short interactive tasks**, such as debugging, testing, or
	quickly visualizing data, typically lasting only a few
	minutes. For running longer analysis tasks, users must use a
	non-interactive job submission (`sbatch`).
4.  **Resource Specification:** When starting a JupyterHub session,
	users typically set parameters via a *Server Options* form, which
	dictates the requested compute resources for the underlying
	interactive job. These options can include the `Account`, `Time
	(hours)`, `Number of (CPU) cores` reserved on a single node,
	`Memory (MB)` limit, and optionally, the `GPU configuration`.

## Features and Use of JupyterNotebooks

Jupyter Notebook is a notebook authoring application, under the
Project Jupyter umbrella. Built on the power of the computational
notebook format, Jupyter Notebook offers fast, interactive new ways to
prototype and explain your code, explore and visualize your data, and
share your ideas with others.

Notebooks extend the console-based approach to interactive computing
in a qualitatively new direction, providing a web-based application
suitable for capturing the whole computation process: developing,
documenting, and executing code, as well as communicating the
results. The Jupyter notebook combines two components:

A web application: A browser-based editing program for interactive
authoring of computational notebooks which provides a fast interactive
environment for prototyping and explaining code, exploring and
visualizing data, and sharing ideas with others

Computational Notebook documents: A shareable document that combines
computer code, plain language descriptions, data, rich visualizations
like 3D models, charts, mathematics, graphs and figures, and
interactive controls

### Main features of the web application

- In-browser editing for code, with automatic syntax highlighting,
  indentation, and tab completion/introspection.
- The ability to execute code from the browser, with the results of
  computations attached to the code which generated them.
- Displaying the result of computation using rich media
  representations, such as HTML, LaTeX, PNG, SVG, etc. For example,
  publication-quality figures rendered by the [matplotlib] library,
  can be included inline.
- In-browser editing for rich text using the `Markdown` markup
  language, which can provide commentary for the code, is not limited
  to plain text.
- The ability to easily include mathematical notation within markdown
  cells using LaTeX, and rendered natively by MathJax.

### Notebook documents

Notebook documents contains the inputs and outputs of a interactive
session as well as additional text that accompanies the code but is
not meant for execution. In this way, notebook files can serve as a
complete computational record of a session, interleaving executable
code with explanatory text, mathematics, and rich representations of
resulting objects. These documents are internally JSON files and are
saved with the .ipynb extension. Since JSON is a plain text format,
they can be version-controlled and shared with colleagues.

Notebooks may be exported to a range of static formats, including HTML
(for example, for blog posts), reStructuredText, LaTeX, PDF, and slide
shows, via the [nbconvert] command.

Furthermore, any .ipynb notebook document available from a public URL
can be shared via the Jupyter Notebook Viewer `nbviewer`. This service
loads the notebook document from the URL and renders it as a static
web page. The results may thus be shared with a colleague, or as a
public blog post, without other users needing to install the Jupyter
notebook themselves. In effect, nbviewer is simply [nbconvert] as a
web service, so you can do your own static conversions with nbconvert,
without relying on nbviewer.

There are a lot of things to explore within JupyterNotebooks and
JupyerHub. Please let me know if there's a specific topic you'd like a
deeper exploration of!

## Differences from Typical HPC Usage

The interactive nature of JupyterHub jobs, as well as specific cluster
configurations, introduce important distinctions compared to standard
non-interactive batch processing on HPC systems:

### 1. Job Priority and Queueing

The order in which jobs are considered for scheduling on the clusters
is determined by priority, typically using the Fair Tree
algorithm. While batch submission (`sbatch`) is generally the most
common and efficient way to use the clusters, interactive jobs like
JupyterHub sessions have specific scheduling behaviors:

*   **Priority Boost:** Only one interactive job per cluster at a time
	receives a priority increase designed to help it start quickly
	(within a few seconds or minutes). This applies to `salloc`,
	`srun`, and JupyterHub jobs.
*   **Contention:** If you already have another interactive job
	running on the same cluster, your new Jupyter session will lose
	this priority boost and will wait in the queue along with other
	regular batch jobs. This often leads to a "Spawn failed: Timeout"
	error if the job does not start within five minutes.
*   **Time Limits and Wait Times:** Requesting a longer time, a GPU,
	or too many CPU cores can increase the wait time. Jobs requesting
	interactive sessions for three hours or less may start quickly due
	to dedicated test nodes; however, interactive jobs requesting
	longer durations (many hours or days) run on the regular set of
	nodes and may wait for unpredictable amounts of time before
	starting.

### 2. Networking Restrictions

A significant difference is the internet access limitation on the
compute nodes where the Jupyter kernels run:

*   **No Internet Access:** The compute nodes running the Jupyter
	kernels **do not have internet access**.
*   **Consequences:** This means users cannot download data or code
	from the internet. Activities like `git clone` or `pip install`
	(if the necessary package wheel is absent) cannot be
	performed. This restriction also affects machine learning
	workflows that frequently download or upload data directly from
	the executing code.
*   **File Transfer:** Files can only be transferred directly to or
	from your own local computer.

### 3. Resource Accounting

JupyterHub sessions are treated as scheduled jobs, and the resources
requested impact future job priority.

*   **Job Charging:** HPC resource allocations (core-years and
	GPU-years) require users to submit jobs to a scheduler. Whether
	you use JupyterHub (an interactive job) or a typical batch job,
	the job is charged to a Resource Allocation Project (RAP).
*   **Requested vs. Used Resources:** The scheduler determines the
	priority of future jobs based on the resources requested, not the
	resources actually used. Therefore, when configuring a JupyterHub
	session, requesting only the resources you need is important. If
	you request more resources (cores or memory) than you use, those
	unused resources are unavailable to others, and your research
	group is charged for the requested resources when calculating
	priority.
