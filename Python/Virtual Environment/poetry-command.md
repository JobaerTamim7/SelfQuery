### Getting Started with Poetry

- **Installation**

    ```bash
    pipx install poetry
    ```

    <details>
    <summary>Fun Fact: pip vs pipx</summary>
    
    - **`pip`** is generally used for **project dependencies** (e.g., Flask, Requests) and **development tools** (e.g., Black, PyTest):
    
        ```bash
        pip install flask requests
        pip install black pytest
        ```
    
    - **`pipx`** is better suited for **standalone applications** and **CLI tools** (e.g., Poetry, httpie, awscli):
    
        ```bash
        pipx install poetry httpie
        pipx install awscli youtube-dl
        ```
        
    - **Summary**: Use **`pip`** for project-specific dependencies in a virtual environment, and **`pipx`** for standalone applications, which are isolated from other environments.
    
     *Standalone Application: Apps that do not rely on external dependencies (such as libraries or frameworks), and can function independently.*
    </details>

- **Create a New Project**

    ```bash
    poetry new <project-name>
    ```
    For more [`new`](https://python-poetry.org/docs/cli/#new)

- **Add Poetry to an Existing Project**

    ```bash
    poetry init
    ```
    For more [`init`](https://python-poetry.org/docs/cli/#init)

- **Activate or Create Virtual Environment**

    ```bash
    poetry shell
    ```
    For more [`shell`](https://python-poetry.org/docs/cli/#shell)

- **Search for a Package**

    ```bash
    poetry search <package-name>
    ```
    For more [`search`](https://python-poetry.org/docs/cli/#search)

- **Add a Package**

    ```bash
    poetry add <package-name>
    ```
    For more [`add`](https://python-poetry.org/docs/cli/#add)

- **Remove a Package**

    ```bash
    poetry remove <package-name>
    ```
    For more [`remove`](https://python-poetry.org/docs/cli/#remove)

- **Validate `pyproject.toml`**

    ```bash
    poetry check
    ```
    <small>Returns "All set!" if everything is okay.</small>

    For more [`check`](https://python-poetry.org/docs/cli/#check)

- **Lock Dependencies**

    ```bash
    poetry lock
    ```
    <small>This does not install the project package.</small>

    For more [`lock`](https://python-poetry.org/docs/cli/#lock)

- **Install Project Package**

    ```bash
    poetry install
    poetry install --editable  # for editable installation
    ```
    For more [`install`](https://python-poetry.org/docs/cli/#install)

- **View Packages**

    ```bash
    poetry show              # show all packages
    poetry show <package-name>  # show details of a single package
    ```
    For more [`show`](https://python-poetry.org/docs/cli/#show)

- **Build Project Source and Wheels**

    ```bash
    poetry build
    ```
    For more [`build`](https://python-poetry.org/docs/cli/#build)
- **Listing Environment**

    ```bash
    poetry list   #shows all the environments
    ```
    For more [`list`](https://python-poetry.org/docs/cli/#list) 
- **Delete Enviroment**

    ```bash
    poetry env remove <env_name>
    ```
    For more [`remove`](https://python-poetry.org/docs/managing-environments/#deleting-the-environments)
    <small>
    <details>
    <summary>For Remove</summary>
        1. At fisrt navigate to the project directory using <code>cd</code>
        2. Check the virtual environment using command <code>poetry list</code>
    </details>
    </small>
