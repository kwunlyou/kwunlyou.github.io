---
layout: post
title:  "Package Name Normalization for Pip Installing"
categories: [Programming]
tags: [Python, Pip, Package Name]
excerpt: "Name normalization happens when using pip to install python package. Dash, underscore and period are conflated based on certain rules when pip searchs for packages."
---
<br>
#### Name normalization
Name normalization happens when using pip to install python package. Dash `'-'`, underscore `'_'` and period `'.'`are conflated based on certain rules when pip searchs for packages. 

<br>
#### Find packages
For example, we use the following command to install a package `aaa.bbb_ccc`.
~~~ python
pip install aaa.bbb_ccc
~~~
`pip` will search for the package `aaa.bbb_ccc` on [PyPI](https://pypi.python.org/pypi). The "best" match for the requirements is selected (see [pip guide](https://pip.pypa.io/en/stable/reference/pip_install/#finding-packages) and [source code](https://github.com/pypa/pip/blob/9.0.1/pip/index.py#L490) for details). Loosely speaking, the "best" match is the newest version of the package.

<br>
#### Matching `wheel` names
- The package name `aaa.bbb_ccc` is transformed to `aaa-bbb-ccc` by calling `canonicalize_name`([source code](https://github.com/pypa/pip/blob/9.0.1/pip/index.py#L413))
                
~~~ python
# extracted from pip source code
_canonicalize_regex = re.compile(r"[-_.]+")
 
def canonicalize_name(name):
    return _canonicalize_regex.sub("-", name).lower()
~~~

- The `wheel` name is transformed in the same way ([source code](https://github.com/pypa/pip/blob/9.0.1/pip/index.py#L633))

<br>
#### Matching `tarball` names
- `aaa.bbb_ccc` is converted to `aaa.bbb-ccc` by calling `safe_name` ([source code](https://github.com/pypa/pip/blob/9.0.1/pip/req/req_install.py#L375))

~~~ python
# extracted from pip source code
def safe_name(name):
    """Convert an arbitrary string to a standard distribution name
    Any runs of non-alphanumeric/. characters are replaced with a single '-'.
    """
    return re.sub('[^A-Za-z0-9.]+', '-', name)
~~~

- `'_'` in the `tarball` name is replaced by `'-'` ([source code](https://github.com/pypa/pip/blob/9.0.1/pip/index.py#L706))

<br>
#### Package name convention in [PEP 8](https://www.python.org/dev/peps/pep-0008/#package-and-module-names)
[PEP 8](https://www.python.org/dev/peps/pep-0008/) doesn't encourage a long and complicated package name.

~~~ bash
# extracted from PEP 8
Python packages should also have short, all-lowercase names, although the use of underscores is discouraged.
~~~

