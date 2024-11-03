# Web Scraping using Python

This folder is dedicated for learning and experimenting with web scraping techniques using Python. It contains scripts, sample data, and resources to help you practice scraping information from websites, processing the data, and saving it in various formats.

### Technology Used
  - **Requests Module** : It is used to make HTTP requests to websites and fetch their HTML content. This is the first step in web scraping, as it retrieves the raw data from a URL.
  
  - **BeautifulSoup Library** : It is a Python library used to parse HTML and XML documents. After fetching the HTML content with `requests`, `BeautifulSoup` helps locate and extract specific data points from the page, such as titles, prices, or links.
  
  - **Selenium** : It is used to automate web browsers, which is especially useful for scraping dynamic websites that rely on JavaScript to load content (e.g., single-page applications or infinite scroll pages). Unlike `requests`, which only fetches the HTML, `Selenium` can interact with the browser to load and manipulate dynamic content.
  -  **lxml** : The lxml module in Python is a powerful library for processing XML and HTML documents. It provides a high-performance XML and HTML parsing capabilities along with a simple and Pythonic API. lxml is widely used in Python web scraping due to its speed, flexibility, and ease of use.
  -  **PyAutoGUI** : The pyautogui module in Python is a cross-platform GUI automation library that enables developers to control the mouse and keyboard to automate tasks. While it’s not specifically designed for web scraping, it can be used in conjunction with other web scraping libraries like Selenium to interact with web pages that require user input or simulate human actions.

For more beginner intro [`Web-Scraping`](https://www.geeksforgeeks.org/python-web-scraping-tutorial/)


