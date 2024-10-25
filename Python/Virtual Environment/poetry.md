# Poetry  
## Python Dependency and Automation Manager

- ### Get Started with Command:  
    - **Installation**
    
    ```bash
    pipx install poetry
    ```
    
    <details>
    <summary>Fun Fact: pip vs pipx</span></summary>
    
    - **`pip`** 
         1. **Project Dependencies** (e.g. Flask, Requests):<br><br>
         
        ```bash
        pip install flask requests 
        ```
        2. **Development Tools** (e.g. Black, PyTest):<br><br>
        
        ```bash
        pip install black pytest
        ```
    
    - **`pipx`** 
        1. **Standalone Applications** (e.g. Poetry, httpie): <br><br>
        
        ```bash
        pipx install poetry httpie
        ```
        2. **CLI Tools** (e.g. awscli, youtube-dl): <br><br>
        
        ```bash
        pipx install awscli youtube-dl
        ```
    
    #### Summary:
    - Use **`pip`** for project dependencies and development tools within a virtual environment.
    - Use **`pipx`** for installing standalone command-line applications, ensuring each tool has its own isolated environment.
    
    *Standalone Application: Apps that do not rely on external dependencies (such as libraries or frameworks), and can function independently.*
    </details>
    
    - **New Project**
    
      ```bash
      poetry new <project-name>
      ```
      *for more :* <code>[new](https://python-poetry.org/docs/cli/#new)</code>
    -   **Add Poetry in already built project**
        ```bash
        poetry init
        ```
        *for more :* <code>[init](https://python-poetry.org/docs/cli/#init)</code>
    - **Add Pakage**
        ```bash
        poetry add <pakage-name>
        ```
        *for more :* <code>[add](https://python-poetry.org/docs/cli/#add)</code>
    - **Remove Pakage**
        ```bash
        poetry remove <pakage-name>
        ```
        *for more :* <code>[remove](https://python-poetry.org/docs/cli/#remove)</code>
    - **Install Pakage**
        ```bash
        poetry install
        ```
         *for more :* <code>[install](https://python-poetry.org/docs/cli/#install)</code>
    - **Observe Pakage**
        ```bash
        poetry show # show all the pakages 
        poetry show <pakage-name> # show a single pakage
        ```
         *for more :* <code>[show](https://python-poetry.org/docs/cli/#show)</code>
    - **Build Project Source and Wheels**
        ```bash
        poetry build 
        ```
         *for more :* <code>[build](https://python-poetry.org/docs/cli/#build)</code>
