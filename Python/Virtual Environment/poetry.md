# Poetry  
## Python Dependency and Automation Manager

- ### Get Started:  
  - **Installation Command**
    
    ```bash
    pipx install poetry
    ```

    <details>
    <summary><span style='color:green;text-decoration:italic'>Fun Fact: pip vs pipx</span></summary>
  
    - **`pip`** 
        1. **Project Dependencies** (e.g. Flask, Requests):
        
        ```bash
        pip install flask requests 
        ```
        2. **Development Tools** (e.g. Black, PyTest):
        
        ```bash
        pip install black pytest
        ```

    - **`pipx`** 
        1. **Standalone Applications/CLI** (e.g. Poetry, httpie):
        
        ```bash
        pipx install poetry httpie
        ```
        2. **CLI Tools** (e.g. awscli, youtube-dl):
        
        ```bash
        pipx install awscli youtube-dl
        ```

    #### Summary:
    - Use **`pip`** for project dependencies and development tools within a virtual environment.
    - Use **`pipx`** for installing standalone command-line applications, ensuring each tool has its own isolated environment.
    
    *Standalone Application: Apps that do not rely on external dependencies (such as libraries or frameworks), and can function independently.*
    </details>


  - **New Project Command**

      ```bash
      poetry new <project-name>
      ```
