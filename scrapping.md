# Web Scraping for RAG Chatbot Knowledge Base

## Overview

In the initial phase of developing the Retrieval-Augmented Generation (RAG) chatbot for the department website (cs.du.ac.in), I focused on gathering relevant information through web scraping. This process is crucial for creating a comprehensive knowledge base that the chatbot will use to respond to user queries effectively.

## Approach

### 1. Data Collection

To collect data from the department's website, I utilized two powerful tools: **Selenium** and **Beautiful Soup**. Selenium is particularly effective for scraping dynamic content, as it allows interaction with web pages just like a human user would. Beautiful Soup, on the other hand, is excellent for parsing HTML and extracting text.

### 2. Setting Up the Scraper

I began by setting up a headless browser using Selenium. This means that the browser operates in the background without a graphical interface, making it efficient for automated tasks. The headless mode is especially useful for scraping large amounts of data quickly.

### 3. Defining Target URLs

I compiled a list of specific URLs from the department's website that contained valuable information. This list included various sections such as:

- Program details (e.g., PhD, MCA, MSc)
- Research publications and grants
- Student resources (e.g., scholarships, placement recruiters)
- Faculty and staff information
- Events and announcements

By targeting these specific pages, I ensured that the scraped data would be relevant and useful for building the chatbot's knowledge base.

### 4. Extracting Content

For each URL in the list, I instructed the scraper to navigate to the page and wait briefly to allow all content to load fully. Once the page was loaded, I used Beautiful Soup to parse the HTML and extract all visible text content. This included headings, paragraphs, and any other text elements that could provide useful information.

### 5. Storing Data

The extracted data was organized into a structured format, with each entry containing the URL and its corresponding content. This structure facilitates easy access and retrieval when building the knowledge base for the chatbot.

### 6. Saving Data

After collecting data from all targeted URLs, I saved it into a CSV file. This format is widely used for data storage and can be easily manipulated or imported into other systems for further processing.

## Code Repository

The code for the RAG Chatbot is available on [GitHub](https://github.com/koachgg/RAG_ChatBot).

## Future Work (Optional) : 

## Setting Up Regular Intervals for Scraping

### 1. Scheduling Tasks
Automate your scraping tasks using scheduling tools:
- **Cron Jobs (Linux/Mac)**: Schedule your script to run at specific intervals.
- **Task Scheduler (Windows)**: Set up schedules for your script.
- **Python Libraries**: Use libraries like `schedule` in Python to manage timing.

### 2. Monitoring Changes
Implement change detection mechanisms:
- **Hashing**: Store a hash of scraped content to compare changes before re-scraping.
- **Webhooks**: Use webhooks from websites (if available) to get notified of updates.

## Using APIs for Data Retrieval

### 1. Understanding APIs
APIs allow you to retrieve data from websites in structured formats like JSON or XML, simplifying data collection.

### 2. When to Use APIs
Utilize APIs when:
- A website provides a well-maintained API with necessary data.
- You want structured data directly without dealing with HTML parsing.

### 3. Making API Calls
Interact with an API by:
- **Authentication**: Check if an API key is required.
- **Endpoints**: Identify relevant API endpoints.
- **HTTP Requests**: Use libraries like `requests` in Python to make GET or POST requests.

### 4. Handling Rate Limits
Be aware of rate limits on APIs:
- Check API documentation for limits.
- Implement logic in your code to handle rate limiting by pausing requests when necessary.

## Conclusion

This web scraping approach serves as a foundational step in creating a robust knowledge base for the RAG chatbot. By systematically collecting and organizing relevant information from the department's website and implementing strategies for regular updates through scraping or APIs, I am setting the stage for an informative and responsive chatbot that can effectively assist users with their inquiries.