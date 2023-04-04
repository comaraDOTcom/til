We have a CLI tool wrapper on airbyte using click in python.
To debug it using Vscode the docs suggest writing a `launch.json` script.

The [launch.json vscode documentation](https://code.visualstudio.com/docs/python/debugging) says the `launch.json` defines the configuration for debugging. Debugging a singular file is done by setting `"request": "launch"` and `"program": "${file}"` in the json config.
For a module like flask, we can configure it such that
```json
{
	"type": "python",
	"request": "launch",
	"module": "flask",
}
```
Equivalent to running `python flask -m `

Let's look at an example. A cli tool I made called `calculate_strokes` . It will be its own module in python. The `tree ` will look like so
```bash
├── README.md
├── __init__.py
├── calculate_strokes.py
├── setup.py
```
The `__init__.py` file will be empty. In the ``calculate_strokes`.py` file the `Click` module is initialised and some calls are created.

```python

import Click

@click.group()
def cli():
	pass

@cli.command()
@click.option("--operator", type=click.Choice(["add", "subtract"]), required=True)
@click.option("--data", required=True)
def calc_operation(operator, data):
	operator(date)
	return data
```

The cli tool is setup in `setup.py`

```python 
from setuptools import setup, find_packages

setup(
	name="calculate_strokes",
	version="0.1.0",
	py_modules=["calculate_strokes"],
	include_package_data=True,
	install_requires=["click"],
	entry_points={
		"console_scripts": [
		"calculate_strokes=calculate_strokes:cli",
		],
	},
)
```


Calling vscode to debug the `calculate_strokes` is currently painful via the `launch.json`. It can only call modules not cli tools.

To get this to work, we append the following to the module file with the command you are debugging.

```json
if __name__ == '__main__':
	calc_operation(["--operator", "add", "--data", "data.json""])
```

And updated the `launch.json`: Note the args are not needed.
```json
"version": "0.2.0",

"configurations": [
{
	"name": "Calculate operation",
	"type": "python",
	"cwd": "/Users/conoromara/github/example-repo",
	"request": "launch",
	"module": "calculate_strokes",
	"console": "integratedTerminal",
	"args": ["calc_operation", "--operator", "add", "--data", "data.json"],
	"autoReload": {
		"enable": true
		},
	"stopOnEntry": true
	}
	]
}
```

And this works as expected.