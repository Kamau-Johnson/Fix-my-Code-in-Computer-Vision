# Fix-my-Code-in-Computer-Vision
<p float="center">
    <img src="code1.png" width="50%" /> 
</p

### Starting to use `pathlib`

The first step is to import the `Path` class from `pathlib` module with the `from pathlib import Path` statement. This single import gives you access to a powerful set of tools for path manipulation and file system operations.


```python
from pathlib import Path
```

Let's access the home directory with `Path.home()`. Note that method-based operation is cross-platform, thus the same code will work the same regardless of the operating system. 


```python
home_directory = Path.home()
print(home_directory)
```

    /root


Now it's your turn to try to use the `Path` class to explore the file system.

**Task 3.1.1:** Find the current working directory using the `cwd` method on `Path`.


```python
current_working_directory = Path.cwd()
print(current_working_directory)
```

    /app


### The forward slash (/) operator

The forward slash (`/`) operator in `pathlib` is used for path joining. The `/` operator allows you to combine multiple Path objects or strings to create new paths. It automatically uses the correct path separator for the current operating system (`/` for Unix-based systems and `\` for Windows). As a result, the same code can run on different operating systems without any rewriting.

Below is an example of creating a variable that combines a Path object with several other elements. Note that since this only is a variable right now, it does not need to reference an actual location.


```python
path = Path("folder") / "subfolder" / "file.txt"
print(path)
```

    folder/subfolder/file.txt


**Task 3.1.2:** Create a variable that defines a folder named "scripts" in the current directory.



```python
current_working_directory = Path.cwd()
scripts_path = current_working_directory / "scripts"
print(scripts_path)
```

    /app/scripts


Python Path objects are different from Python strings (`str`). The addition operator (`+`) for Python string does not work with Python Path objects. The forward slash (`/`) operator has to be used with Python Path objects.

**Task 3.1.3:** Change the operator to work with a Python Path object.


```python
path = Path("scripts") / "tests" / "tests.py"
print(path)
```

    scripts/tests/tests.py


### Absolute vs. Relative Paths

When working with paths, it's important to understand the distinction between absolute and relative paths. Absolute paths provide the complete route from the root directory to the specified location, while relative paths are based on the current working directory. `pathlib` allows you to create both types: an absolute path might look like `Path('/home/jovyan')` and a relative path could be `Path('03-traffic-in-dhaka/lessons')`. Note that a relative path doesn't start with a root directory indicator (like "/" on Unix-based systems or "C:" on Windows).

Below is an example of converting a relative path to an absolute path by prepending the absolute path to the beginning of the relative path.


```python
relative_path = Path("config", "settings")
absolute_path = Path.home() / relative_path
print(absolute_path)
```

    /root/config/settings


**Task 3.1.4:** Convert a relative path to an absolute path based on `Path.cwd()`.


```python
relative_path = Path("scripts", "tests")
absolute_path = Path.cwd() / relative_path
print(absolute_path)
```

    /app/scripts/tests


### Listing directory contents

Instances of the `Path` class have the `iterdir` method which iterates over the contents, files and subdirectories, of a given directory. Calling the `iterdir` method returns an iterator that doesn't load all the results into memory at once.

Below is an example of using the `iterdir` method.


```python
current_working_directory = Path.cwd()
print(current_working_directory.iterdir())
```

    <generator object Path.iterdir at 0x70d4f5905540>


An iterator can be looped over in Python.


```python
current_working_directory = Path.cwd()
for item in current_working_directory.iterdir():
    print(item)
```

    /app/.Trash-0
    /app/.ipynb_checkpoints
    /app/031-fix-my-code.ipynb
    /app/Arial.ttf
    /app/Test.ipynb


`iterdir()` returns an iterator, not a list. That means you can't directly print it or get its length.


```python
print(Path.cwd().iterdir())
```

    <generator object Path.iterdir at 0x70d4f5905540>


If you want to load the data into memory all at once, the iterator can be cast into a list.


```python
# Cast to list before printing
print(list(Path.cwd().iterdir()))
```

    [PosixPath('/app/.Trash-0'), PosixPath('/app/.ipynb_checkpoints'), PosixPath('/app/031-fix-my-code.ipynb'), PosixPath('/app/Arial.ttf'), PosixPath('/app/Test.ipynb')]


**Task 3.1.5:** Fix the code to get the number elements in the current directory with `len`.


```python
current_dir_iter = Path.cwd().iterdir()

# Fix the code
num_elements = num_elements = len(list(current_dir_iter))
```

### Making directories

The `pathlib` module has a convenient way to create a new directory. Create an instance of a `Path` class that specifies the location of this new directory, then call the `mkdir` method to actually create it. 

Below is an example of how this is done. If this cell is run twice it will generate a `FileExistsError`.


```python
scripts_dir = Path.cwd() / "scripts"
scripts_dir.mkdir()
```


    ---------------------------------------------------------------------------

    FileExistsError                           Traceback (most recent call last)

    Cell In[25], line 2
          1 scripts_dir = Path.cwd() / "scripts"
    ----> 2 scripts_dir.mkdir()


    File /usr/local/lib/python3.11/pathlib.py:1116, in Path.mkdir(self, mode, parents, exist_ok)
       1112 """
       1113 Create a new directory at this given path.
       1114 """
       1115 try:
    -> 1116     os.mkdir(self, mode)
       1117 except FileNotFoundError:
       1118     if not parents or self.parent == self:


    FileExistsError: [Errno 17] File exists: '/app/scripts'


Adding the keyword argument `exist_ok=True` prevents raising an error if the directory already exists.


```python
scripts_dir = Path.cwd() / "scripts"
scripts_dir.mkdir(exist_ok=True)
```

**Task 3.1.6:** Make a "tests" folder in the current directory, even it already exists.


```python
tests_dir = Path.cwd() / "tests"
tests_dir.mkdir(exist_ok=True)
```

### Glob patterns

Glob allows you to search for files and directories using wildcard patterns, making it handy to find files with certain suffixes. We can use the glob to find all the TrueType font (`.ttf`) files in the current directory.


```python
current_working_directory = Path.cwd()
all_ttf_files = current_working_directory.glob("*.ttf")
for file in all_ttf_files:
    print(file)
```

    /app/Arial.ttf


**Task 3.1.7:** Find all the Jupyter Notebooks (`.ipynb`) in the current directory.


```python
current_working_directory = Path.cwd()
all_ipynb_files = current_working_directory.glob("*.ipynb")
for file in all_ipynb_files:
    print(file)
```

    /app/031-fix-my-code.ipynb
    /app/Test.ipynb


In summary, the `pathlib` module provides a powerful and intuitive way to work with file systems in Python. The module's object-oriented approach to handling paths simplifies common operations like listing directory contents, creating directories, and searching for files. 

---
&#169; 2024 by [WorldQuant University](https://www.wqu.edu/)
