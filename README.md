# PyInstaller-Action-Windows

Github Action for building executables with PyInstaller

To build your application, you need to specify where your source code is via the `path` argument, this defaults to `src`.

The source code directory should have your `.spec` file that PyInstaller generates. If you don't have one, you'll need to run PyInstaller once locally to generate it.
Also if you have another program `.spec` file you can set specific pyinstaller `.spec` file by `spec: <YOUR_SPEC_FILE_NAME>`

If the `src` folder has a `requirements.txt` file, the packages will be installed into the environment before PyInstaller runs. This can also be specified via the `requirements` argument.

If you wish to specify a package mirror, this is possibly via the `pypi_url` and/or the `pypi_index_url`, these defaults are:

- `pypi_url` = `https://pypi.python.org/`
- `pypi_index_url` = `https://pypi.python.org/simple`

> If you are using the default Python `gitignore` file, ensure to remove `.spec`.

> This action uses [Wine](https://www.winehq.org) to emulate windows inside Docker for packaging POSIX executables.

## Example usage

There's an example repository where you can see this action in action: <https://github.com/vednig/pyinstaller-action-windows-example>. Where you can find the packaged executable at: <https://github.com/vednig/pyinstaller-action-windows-example/actions/runs/135879475>.

Include this in your `.github/workflows/main.yaml`:

```yaml
- name: PyInstaller Windows
  uses: vednig/pyinstaller-action-windows@main
    with:
      path: src
```

## Full Example

Here is an entire workflow for:

- Packaging an application with PyInstaller
- Uploading the packaged executable as an artifact

``` yaml

name: Package Application with Pyinstaller

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2

    - name: Package Application
      uses: vednig/pyinstaller-action-windows@main
      with:
        path: src

    - uses: actions/upload-artifact@v2
      with:
        name: name-of-artifact
        path: src/dist/windows
```

## FAQ

If you get this error: `OSError: [WinError 123] Invalid name: '/tmp\\*'`, ensure your path is correctly configured, the default is `src`.

## Sources

A big thank you to all the contributors over at <https://github.com/cdrx/docker-pyinstaller>, this action is just a modified version of their docker container, thank you!
