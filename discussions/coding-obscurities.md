# Programming Obscurities

The following example(s) walk through solving challenges that have
come up for scholars. The goal is less to provide specific solutions
to problems, but more to help establish how you can

## Same function, different answers?

A scholar encountered a puzzling issue. They were comparing different
tools, one in MATLAB and the other in Python. However, the tools were
supposedly doing the same thing: compute the spectram of an input
dataset.

Fortunately, this is a standard data manipulation and is readily
available in both software environments:
- `spectrogram` in MATLAB
- `specgram` in python scipy (and MNE)

Unfortunately, the analysis that's using the same operation on the
same inputs but with different software. So what gives?

### First: Check your assumptions

Despite doing the "same thing", clearly these functions do something
different.

Fortunately, it was pretty quick to identify what function was not
performing as expected. The fastest way to see what's going on is to
check the inline help.

specgram
```
Compute a spectrogram with consecutive Fourier transforms (legacy function).

Spectrograms can be used as a way of visualizing the change of a
nonstationary signal’s frequency content over time.

Parameters:

xarray_like

Time series of measurement values
	fsfloat, optional

Sampling frequency of the x time series. Defaults to 1.0.
	windowstr or tuple or array_like, optional

Desired window to use. If window is a string or tuple, it is passed to get_window to generate the window values, which are DFT-even by default. See get_window for a list of windows and required parameters. If window is array_like it will be used directly as the window and its length must be nperseg. Defaults to a Tukey window with shape parameter of 0.25.
	npersegint, optional

Length of each segment. Defaults to None, but if window is str or tuple, is set to 256, and if window is array_like, is set to the length of the window.
	noverlapint, optional

Number of points to overlap between segments. If None, noverlap = nperseg // 8. Defaults to None.
	nfftint, optional

Length of the FFT used, if a zero padded FFT is desired. If None, the FFT length is nperseg. Defaults to None.
	detrendstr or function or False, optional

Specifies how to detrend each segment. If detrend is a string, it is passed as the type argument to the detrend function. If it is a function, it takes a segment and returns a detrended segment. If detrend is False, no detrending is done. Defaults to ‘constant’.
	return_onesidedbool, optional

If True, return a one-sided spectrum for real data. If False return a two-sided spectrum. Defaults to True, but for complex data, a two-sided spectrum is always returned.
	scaling{ ‘density’, ‘spectrum’ }, optional

Selects between computing the power spectral density (‘density’) where Sxx has units of V**2/Hz and computing the power spectrum (‘spectrum’) where Sxx has units of V**2, if x is measured in V and fs is measured in Hz. Defaults to ‘density’.
	axisint, optional

Axis along which the spectrogram is computed; the default is over the last axis (i.e. axis=-1).
	modestr, optional

Defines what kind of return values are expected. Options are [‘psd’, ‘complex’, ‘magnitude’, ‘angle’, ‘phase’]. ‘complex’ is equivalent to the output of stft with no padding or boundary extension. ‘magnitude’ returns the absolute magnitude of the STFT. ‘angle’ and ‘phase’ return the complex angle of the STFT, with and without unwrapping, respectively.

Returns

f
	ndarray

Array of sample frequencies.
	t
	ndarray

Array of segment times.
	Sxx
	ndarray

Spectrogram of x. By default, the last axis of Sxx corresponds to the segment times.
```

spectrogram
```
s = spectrogram(x)
s = spectrogram(x,window)
s = spectrogram(x,window,noverlap)
s = spectrogram(x,window,noverlap,nfft)
[s,w,t] = spectrogram(___)
[s,f,t] = spectrogram(___,fs)
[s,w,t] = spectrogram(x,window,noverlap,w)
[s,f,t] = spectrogram(x,window,noverlap,f,fs)
[___,ps] = spectrogram(___,spectrumtype)
[___] = spectrogram(___,"reassigned")
[___,ps,fc,tc] = spectrogram(___)
[___] = spectrogram(___,freqrange)
[___] = spectrogram(___,Name=Value)
spectrogram(___)
spectrogram(___,freqloc)
Description

s = spectrogram(x) returns the Short-Time Fourier Transform (STFT) of the input signal x. Each column of s contains an estimate of the short-term, time-localized frequency content of x. The magnitude squared of s is known as the spectrogram time-frequency representation of x [1].

example

s = spectrogram(x,window) uses window to divide the signal into segments and perform windowing.

example

s = spectrogram(x,window,noverlap) uses noverlap samples of overlap between adjoining segments.

example

s = spectrogram(x,window,noverlap,nfft) uses nfft sampling points to calculate the discrete Fourier transform.

example

[s,w,t] = spectrogram(___) returns a vector of normalized frequencies, w, and a vector of time instants, t, at which the STFT is computed. This syntax can include any combination of input arguments from previous syntaxes.

example

[s,f,t] = spectrogram(___,fs) returns a vector of cyclical frequencies, f, expressed in terms of the sample rate fs. fs must be the fifth input to spectrogram. To input a sample rate and still use the default values of the preceding optional arguments, specify these arguments as empty, [].

example

[s,w,t] = spectrogram(x,window,noverlap,w) returns the STFT at the normalized frequencies specified in w. w must have at least two elements, because otherwise the function interprets it as nfft.

example

[s,f,t] = spectrogram(x,window,noverlap,f,fs) returns the STFT at the cyclical frequencies specified in f. f must have at least two elements, because otherwise the function interprets it as nfft.

example

[___,ps] = spectrogram(___,spectrumtype) also returns a matrix, ps, proportional to the spectrogram of x.

	If you specify spectrumtype as "psd", each column of ps contains an estimate of the power spectral density (PSD) of a windowed segment.

	If you specify spectrumtype as "power", each column of ps contains an estimate of the power spectrum of a windowed segment.

example

[___] = spectrogram(___,"reassigned") reassigns each PSD or power spectrum estimate to the location of its center of energy. If your signal contains well-localized temporal or spectral components, then this option generates a sharper spectrogram.

example

[___,ps,fc,tc] = spectrogram(___) also returns two matrices, fc and tc, containing the frequency and time of the center of energy of each PSD or power spectrum estimate.

example

[___] = spectrogram(___,freqrange) returns the PSD or power spectrum estimate over the frequency range specified by freqrange. Valid options for freqrange are "onesided", "twosided", and "centered".

example

[___] = spectrogram(___,Name=Value) specifies additional options using name-value arguments. Options include the minimum threshold and output time dimension.

example

spectrogram(___) with no output arguments plots ps in decibels in the current figure window.

example

spectrogram(___,freqloc) specifies the axis on which to plot the
frequency.
```
We can now compare what the default behavior in each tool is coded to
be.

#### Solution: They do something different by default

In python, `specgram` by default returns the *power spectral density*
(mode='PSD'). In MATLAB, `spectrogram` by default returns the
*short-time fourier transform*.

This explains pretty clearly why the results are different - the tools
aren't doing the same thing!

- To get the python defaults in MATLAB, set `nargout==4` to get it to
compute the PSD.
- To get the MATLAB defaults in python, set `mode='complex'`

This additional insight into what you're *actually* doing will
hopefully help you make more informed decisions and understand what
your analysis is actually able to tell you.

### References

https://stackoverflow.com/questions/31101987/different-spectrogram-between-matlab-and-python

https://dsp.stackexchange.com/questions/87084/scipy-and-matlab-spectrogram-not-matching

https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.spectrogram.html

https://www.mathworks.com/help/signal/ref/spectrogram.html

## MATLAB and Python Differences to be Aware of

The most common challenges encountered when trying to compare identical or nearly identical features or function behavior between MATLAB and Python stem from fundamental differences in language philosophy, syntax, data structure handling, indexing conventions, and module organization.

Here are the key challenges:

### 1. Indexing and Accessing Data Structures

A major obstacle is the difference in how elements within sequences (like lists, arrays, or vectors) are accessed and counted:

*   **Starting Index:** In MATLAB, counting lists and accessing array items starts at **1**. In Python, lists and sequences are **0-indexed**. This means that the first element in Python is accessed using \`. This simple difference was noted as "very hard" for one transitioning user to get used to.
*   **Last Element Access:** MATLAB uses the word `end` to access the last value in a list. Python uses the index **`-1`** (and negative numbers generally to count backward from the end).
*   **Indexing Syntax:** MATLAB uses **round brackets (`()`)** for indexing arrays and also for calling functions. Python uses **square brackets (`[]`)** for indexing sequences and reserves round brackets (`()`) for function and class calls.
*   **Slicing (Colon Operator):** The order of parameters in array slicing differs: in MATLAB, the syntax is typically `start : step : stop`, while in NumPy (Python), the slice syntax is `start : stop : step`. Furthermore, the MATLAB colon operator includes both the start and stop values in the resulting array, whereas NumPy's `arange()` function and slicing typically include elements **up to, but not including, the stop value**.

### 2. Language Philosophy and Data Typing

MATLAB and Python have fundamentally different design philosophies regarding data structures and code organization, leading to behavioral discrepancies:

*   **Everything is an Array vs. General Objects:** MATLAB treats **everything as an array** (or matrix). Python treats everything as a **more general object**. This difference has profound consequences for coding approach. For instance, Python has distinct object types for strings (`str`), lists, and dictionaries, which behave differently than MATLAB's character or string arrays.
*   **Default Operation Behavior (Element-Wise vs. Matrix):** In MATLAB, arithmetic operators default to matrix operations (like matrix multiplication). To perform element-wise operations (such as element-wise multiplication), a user must explicitly use the **dot-star operator (`.*`)**. In NumPy (Python arrays), basic mathematical operators like the asterisk (`*`) operate **element-wise by default**. If a user wants true matrix multiplication in Python, they must use the matrix multiplication operator (`@`) or `np.dot()`.
*   **Arrays Dimensions and Vectors:** MATLAB arrays always have at least two dimensions (even a single number is 1x1), and it distinguishes between **row-vectors** and **column-vectors**. NumPy's default N-element vector has only one dimension and does not intrinsically have a sense of rows and columns, offering more flexibility in operations.

### 3. Module Management and Function Availability

Direct function comparison is complicated by how functionality is structured:

*   **Built-in Functions vs. Modules:** MATLAB loads many functions (like basic math functions) by default. Python follows a philosophy where **few functions are loaded by default**; most functionality is contained within **modules** (like `NumPy`, `SciPy`, or `math`) that must be explicitly **imported**. A transitioning user needs to know *which* module contains the equivalent functionality.
*   **Namespaces:** Python uses **namespaces** to define scopes for functions, classes, and variables, which is a core concept intended to prevent naming conflicts when multiple libraries define functions with the same name (e.g., `math.sqrt()` vs. `np.sqrt()`). MATLAB, by contrast, finds all functions in a single scope with a defined search order.

### 4. Syntax and Code Structure

Translating code blocks requires adapting to different structural requirements:

*   **Code Block Delimitation:** In MATLAB, code blocks (e.g., `for` loops, `if` statements) are closed explicitly using the **`end`** keyword. Python uses a **colon (`:`)** to mark the beginning of a block, and the end is defined by the **end of indentation**.
*   **Indentation Requirements:** In MATLAB, indentation is generally optional (though recommended good practice). In Python, **indentation is obligatory** and syntactically significant for delimiting code blocks, meaning incorrect indentation results in errors.
*   **Alternative Conditional Structures:** MATLAB uses `elseif` for constructing conditional statements. Python replaces this with the shortened keyword **`elif`**. Python also lacks a direct equivalent to MATLAB's `switch/case` block, requiring the use of `if/elif/else` instead.

### 5. Array Slicing and Memory Management

A critical difference impacting behavior comparisons involves how memory is handled during array manipulation:

*   **Slices as Copies vs. Views:** When MATLAB accesses a slice of an
	array and assigns it to a variable, it generally makes a **copy**
	of that portion of the array. Changing the slice does not affect
	the original array. In NumPy (Python), slices of arrays are
	**views** of the original array, meaning they point to the same
	memory location. Therefore, changes made to a slice will **change
	the original array**. If a user wants a copy in Python, they must
	explicitly use `np.copy()`.

## Translating your entire codebase?

The translation of analysis code or toolboxes from MATLAB to Python is
motivated by several significant advantages related to cost,
licensing, community, versatility, and modern programming practices.

### Financial and Licensing Advantages

A primary motivation for switching is the proprietary and costly
nature of MATLAB:

*   **Cost and Access** MATLAB is proprietary and closed-source, and
	its license is typically quite expensive. Furthermore, users are
	charged for **each additional toolbox** they wish to install to
	extend functionality.
*   **Cost Reduction** Python, on the other hand, is **free and
	open-source** software, available at **no cost**. This opens
	possibilities for individuals, small institutions, and companies
	who cannot afford the expensive MATLAB license, thereby cutting
	down license costs.
*   **Vendor Dependence Risk** Since MATLAB is developed exclusively
	by Mathworks, the software's development might stop if the company
	were to cease operations. Python's open-source nature means that
	if the current developers are unable to continue, anyone can pick
	up the development of the language.

### Community, Openness, and Reproducibility

Python fosters an environment of collaboration and transparency:

*   **Open Source and Accountability** Python’s open-source nature
	means the source code can be downloaded, viewed, and modified. For
	researchers, switching to Python brings **greater integrity and
	accountability to research** because all code can be **shared and
	run by any interested reader**.
*   **Broad Audience Reach** Because Python is available at no cost, a
	**much broader audience can use the code** developed by
	researchers.
*   **Supportive Community** Python has a **fantastic and supportive
	community** that is open and welcoming. If someone has a question
	about programming, they can ask on StackOverflow and potentially
	receive help from world-leading Python experts.
*   **Extensive Free Libraries** The Python Package Index (PyPI)
	houses **hundreds of thousands of different Python packages** that
	are free. Adding needed functionality can be as simple as a `$
	conda install` or `$ pip install` command. For data analysis,
	packages like NumPy, SciPy, Matplotlib, and Pandas can meet needs
	similar to MATLAB toolboxes. The SciPy package alone, for
	instance, provides functions for optimization, linear algebra,
	signal processing, and integration, which is "almost like having
	access to a bunch of the MATLAB toolboxes in one package".

### Versatility and Modern Development Practices

Python is a modern, general-purpose language that offers flexibility
beyond scientific computing:

*   **General Purpose Applications** Python is a **general-purpose
	programming language**. This versatility allows it to be used for
	a wide range of applications, including web development, data
	science, deep learning, and creating GUI applications.
*   **Industry Standard** Python is the industry standard for
	**machine learning and artificial intelligence**. Large companies
	like Netflix use Python for data analytics, and historical
	projects like the original Google algorithm used Python.
*   **Performance and Robustness** Python is increasingly popular for
	work involving arrays and matrices, possessing all the
	**computational power of MATLAB for science tasks** and making it
	fast and easy to **develop robust applications**. Its modular
	design philosophy, where few functions are loaded by default,
	allows code to be **faster** by loading only what is necessary.
*   **Code Quality and Optimization** The translation process offers
	an opportunity to **optimize and streamline the codebase**. Python
	emphasizes a philosophy of **clean, well-written, readable** code,
	governed by style guides like PEP 8.
*   **Advanced Features** Python natively supports concepts like
	**object-oriented programming (OOP)**. It also provides advanced
	data structures, such as **dictionaries**, which are powerful
	structures accessed via a "key" rather than position. Using
	**namespaces** is a critical feature that helps define scopes for
	variables, functions, and classes, which is helpful when multiple
	libraries define functions with the same name.

### Career and Future-Proofing

Switching to Python enhances career prospects and future development:

*   **Improved Career Prospects** Having proficiency in Python will
	**improve chances of getting a job in industry**. Experts note
	that Python knowledge will favor an individual's CV, as there will
	be more people who know MATLAB than people hiring for MATLAB
	expertise.
*   **New Development Capabilities** Learning Python unlocks new types
	of projects and analyses, leading to increased abilities as a
	programmer. By starting fresh, users can gain "new powers" like
	writing GUIs or making dynamic visualizations.
*   **Code Portability** Python is an interpreted language, allowing
	Python code to be **ported between all major operating system
	platforms and CPU architectures** with minimal changes.
*   **Future Development** The goal of translation is not just
	replication, but to create a codebase that is **robust, efficient,
	and poised for future development**. While MATLAB's Simulink
	Toolbox remains a niche area where MATLAB excels graphically, the
	underlying functionality of Simulink can still be replicated in
	Python.
