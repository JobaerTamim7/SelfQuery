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

---

- **Create a New Project**

    ```bash
    poetry new <project-name>
    ```
    For more: [`new`](https://python-poetry.org/docs/cli/#new)

---

- **Add Poetry to an Existing Project**

    ```bash
    poetry init
    ```
    For more: [`init`](https://python-poetry.org/docs/cli/#init)

---

- **Activate or Create Virtual Environment**

    ```bash
    poetry shell
    ```
    For more: [`shell`](https://python-poetry.org/docs/cli/#shell)

---

- **Search for a Package**

    ```bash
    poetry search <package-name>
    ```
    For more: [`search`](https://python-poetry.org/docs/cli/#search)

---

- **Add a Package**

    ```bash
    poetry add <package-name>
    ```
    For more: [`add`](https://python-poetry.org/docs/cli/#add)

---

- **Remove a Package**

    ```bash
    poetry remove <package-name>
    ```
    For more: [`remove`](https://python-poetry.org/docs/cli/#remove)

---

- **Validate `pyproject.toml`**

    ```bash
    poetry check
    ```
    Returns `"All set!"` if everything is okay.

    For more: [`check`](https://python-poetry.org/docs/cli/#check)

---

- **Lock Dependencies**

    ```bash
    poetry lock
    ```
    This does not install the project package.

    For more: [`lock`](https://python-poetry.org/docs/cli/#lock)

---

- **Install Project Package**

    ```bash
    poetry install
    poetry install --editable  # for editable installation
    ```
    For more: [`install`](https://python-poetry.org/docs/cli/#install)

---

- **View Packages**

    To show all packages:

    ```bash
    poetry show
    ```

    To show details of a single package:

    ```bash
    poetry show <package-name>
    ```

    For more: [`show`](https://python-poetry.org/docs/cli/#show)

---

- **Build Project Source and Wheels**

    ```bash
    poetry build
    ```
    For more: [`build`](https://python-poetry.org/docs/cli/#build)

---

- **List Virtual Environments**

    ```bash
    poetry env list
    ```
    Shows all the environments associated with the project.

    For more: [`env list`](https://python-poetry.org/docs/managing-environments/#listing-the-environments-associated-with-the-project)

---

- **Delete a Virtual Environment**

    ```bash
    poetry env remove <env_name>
    ```

    For more: [`env remove`](https://python-poetry.org/docs/managing-environments/#deleting-the-environments)

    <details>
    <summary>Steps for Removing Virtual Environments</summary>

    1. Navigate to the project directory:
       ```bash
       cd <project-directory>
       ```
    2. Check the existing virtual environments:
       ```bash
       poetry env list
       ```
    3. Remove the specific environment:
       ```bash
       poetry env remove <env_name>
       ```
    </details>

---

- **Export `requirements.txt`**

    ```bash
    poetry export -f requirements.txt --without-hashes --output requirements.txt
    ```

    <details>
    <summary>About flags</summary>

    1. **`-f`**: Specifies the file format (e.g., requirements.txt). Most popular tools like `pip` use this format.
    2. **`--without-hashes`**: Excludes dependency hashes in the exported file.
    3. **`--output`**: Specifies the name of the output file.

    *Other formats available include `json` and `constraints.txt`.*

    </details>

