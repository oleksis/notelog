Feature	| Python Launcher for Windows |	Python Launcher for Unix
---|---|---
Platform | Windows | Unix-like operating systems
Selecting Python version | Based on shebang lines, file extensions, command-line options, using environment variables or configuration files (.ini) | Based on shebang lines, using `PY_PYTHON` environment variables, specifying a specific version or in the  the `PATH` environment variable
Virtual environments | Can be used | Can be used
Conda environments | Not supported | Can be used
Interactive mode | Available | Available
Launching scripts | From command line or file explorer | From command line
Cross-platform support | Not applicable | Yes
Install Python on demand | Available (via installer)[^1][^2] | Not available

[^1]: https://docs.python.org/3/using/windows.html#install-on-demand  Python Launcher for Windows has the additional feature of being able to install Python on demand via its installer, which is not available in Python Launcher for Unix. This allows users to easily install Python if it is not already installed on their system

[^2]: https://gist.github.com/oleksis/c22ed90daa922c1a072d2593a7f8d5b4#python-launcher-for-windows-installer Python Launcher for Windows Installer
