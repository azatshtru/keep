A module is a python file, whose functionality is imported into other python code using `import module_name`
### the `__name__` variable
`__name__` is a global variable that contains name of the current python file.
## importing modules
### executable statements in modules
any executable statements in a module are run upon import of that module and all the definitions are imported.
### namespace considerations in importing
importing modules doesn't pollute the current namespace with the imported methods, for example, `import math` doesn't add `sqrt` to the current namespace, it should be accessed using `math.sqrt` instead.
> If you intend to use the imported function often, it can be assigned to a local name:
> `sqrt = math.sqrt`

functionality can be imported to the current namespace from the module using the `from module import definition` syntax, or everything can be imported using the `from module import *` syntax, when specific functionality is imported, it can be accessed without using the `module.definition` syntax as it is now inside the current namespace itself.
> anything prefixed with `_` in the module will not be imported during an `from module import *`
### `as` keyword
use the `as` keyword after import statements to give aliases to module or definition names.
### basic import convention and guidelines
- Conventionally, import statements are kept on top of the python script.
- Importing everything from a module using `from module import *` is not a good practice as it pollutes the namespace and makes a lot of code unreadable. `math.sqrt` is more descriptive than `sqrt` because sqrt doesn't immediately tell where the `sqrt` function is defined.
- each module is only imported once per interpreter sessions for efficiency reasons and modules cannot be changed after importing in that session.
### module search path
Python first looks for the module in the builtin modules, if not found, it then searches in `sys.path`

`sys.path` is a list of strings that specify where python looks for modules. When a python script is run,
1. the directory where the script is running is added to `sys.path`, 
2. then PYTHONPATH is appended to `sys.path`, 
3. then site-packages directories for third-party packages installed using pip is added to `sys.path`.

> PYTHONPATH is an environment variable that specifies some list of directories where python looks for modules.

After program initiates, since `sys.path` is a list of module directories, `sys.path` can be modified by the python script using list operations like `sys.path.append('path/to/module')`, this allows searching for modules in more directories.
## executing modules as scripts
running a Python module with `python module.py <arguments>` executes the code in the module, just like it runs when imported. But the `__name__` global variable's value is replaced by `"__main__"`.

To run different commands when the code is imported as a module vs when the code is run as a script, we can check the `__name__` value, if it is equal to `"__main__"`, then the code is being run as a script, otherwise it is being imported as a module.

The following code can be written in the module to run specific instructions when the code is executed as a script. `sys.argv` is used to get the arguments passed into the command line as a list while executing the module. `sys.argv[0]` is the name of the file.
```python
if __name__ == "__main__":
    import sys
    argv = sys.argv
```

When the module is imported, the `if` check prevents executable-only code from running.
## the `dir()` function
- `dir()` with no arguments prints the names defined in the current namespace.
- `dir(<module>)` prints the names in the module that is given to it as an argument.
- `dir(builtins)` prints the names of the builtin functions and variables
## packages
### `__init__.py` file
A package is a directory of modules. A package directory has a collection of modules along with an `__init__.py` file that is used to tell python that the current directory is a package.

If the package has subdirectories, then each subdirectory has it's own `__init__.py`, this effectively turns the subdirectories to subpackages.
### importing modules inside subpackages
Individual modules can be imported from the package using `import package.subpackage.module` or `from package.subpackage import module`.

When doing `import package.subpackage.module`, whenever we want to reference the functionality of `module` in the script, we'll have to write the full `package.subpackage.module.function` which is quite ugly syntactically, therefore `from package.subpackage import module` is used to keep the code clean.
### `__all__` list
While the `__init__.py` file in the package is usually empty and is used to tell the interpreter that the current folder is a package, an additional `__all__` list can be specified in the `__init__.py` file to state what names will be imported from the package when `from package import *` is called.

```python
# __init__.py
__all__ = [
	"module_1",
	"subpackage_2",
	"subpackage_3.module_3",
	"module_4.function_4",
]
```
Without an `__all__`, `from package import *` will only import modules in the package folder as is. setting `__all__` list can be used to customize what is imported in a `from package import *` statement.
### intra-package relative imports
Inside packages, modules can be imported into each other in a relative fashion:
```python
from . import echo    # import from the same subdirectory's package
from .. import formats    # import from the parent subdirectory's package
from ..filters import equalizer    # import from the parent subdirectory's filters directory's package.
```
The modules intended to be used as the main module must always use absolute imports, not relative ones.