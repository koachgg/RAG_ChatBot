# RAG Chatbot for DUCS

This project implements a Retrieval Augmented Generation (RAG) chatbot to answer questions about the University of Delhi's Computer Science department. The knowledge base for the chatbot is built by scraping content from the official department website (`https://cs.du.ac.in/`).

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Project Pipeline](#project-pipeline)
- [Tech Stack](#tech-stack)
- [File Structure](#file-structure)
- [Setup and Installation](#setup-and-installation)
- [Usage (Running the Project)](#usage-running-the-project)
- [Project Timeline](#project-timeline)
- [Knowledge Base Construction](#knowledge-base-construction)
- [License](#license)

## Overview

The goal of this project is to provide an interactive way to query information about the DU Computer Science department, including programs, faculty, admissions, events, and more. It leverages web scraping to gather data, processes this data, generates semantic embeddings, stores them in a vector database (FAISS), and uses a Large Language Model (LLM) with RAG to provide contextually relevant answers.

## Features

-   **Web Scraping**: Dynamically scrapes content from specified URLs of the `cs.du.ac.in` website.
-   **Text Cleaning**: Multiple stages of text cleaning to remove HTML, special characters, and irrelevant boilerplate content.
-   **Embedding Generation**: Uses `sentence-transformers` (`all-MiniLM-L6-v2`) to create dense vector embeddings of text chunks.
-   **Vector Database**: Employs FAISS for efficient similarity search of text embeddings.
-   **Retrieval Augmented Generation (RAG)**:
    -   Retrieves relevant text chunks from FAISS based on user query similarity.
    -   Uses a pre-trained language model (GPT-2 in the example, Flan-T5 also explored) to generate answers based on the retrieved context and the user's query.
-   **Interactive Chat**: Provides a command-line interface for users to ask questions.

## Project Pipeline

The project follows these main steps:

1.  **Data Collection (`crawl.ipynb`)**:
    *   Uses Selenium and BeautifulSoup to scrape textual content from a predefined list of URLs on the `cs.du.ac.in` website.
    *   Performs initial cleaning (HTML removal, special characters, whitespace, case normalization).
    *   Removes specific repetitive header/footer/menu text patterns identified on the website.
    *   Saves the cleaned content to `cs_du_scraped_data_clean.csv`.

2.  **Data Processing & Vector DB Creation (`clean.ipynb`)**:
    *   Reads the cleaned data (e.g., `cs_du_scraped_data_clean - demo.csv` - **ensure this file matches the output of `crawl.ipynb` or is the intended input**).
    *   Applies further text cleaning specific to the RAG pipeline.
    *   Generates embeddings for the content using `all-MiniLM-L6-v2`.
    *   Stores these embeddings in a FAISS index (`csdu_faiss_index.index`) and the corresponding texts in `csdu_texts.json`.

3.  **RAG Chatbot (`clean.ipynb`)**:
    *   Defines a `RAGChatbot` class.
    *   Loads the FAISS index and text data.
    *   When a user query is received:
        *   Encodes the query into an embedding.
        *   Searches the FAISS index for the most similar text chunks (context).
        *   Constructs a prompt combining the retrieved context and the user's query.
        *   Uses a Hugging Face `transformers` model (e.g., GPT-2) to generate an answer.
    *   Provides an interactive loop for chatting.

## Tech Stack

-   **Programming Language**: Python 3.x
-   **Web Scraping**: Selenium, BeautifulSoup4, webdriver-manager
-   **Data Handling**: Pandas, NumPy
-   **Text Processing**: `re` (Regular Expressions)
-   **Embeddings**: `sentence-transformers` (specifically `all-MiniLM-L6-v2`)
-   **Vector Store**: `faiss-cpu`
-   **Language Model**: Hugging Face `transformers` (e.g., `gpt2`, `google/flan-t5-base`)
-   **Development Environment**: Jupyter Notebooks

## File Structure

The project directory (`RAG_ChatBot/`) is organized as follows:

```text
.
├── README.md                       # This file: Project overview, setup, and usage instructions.
├── main.ipynb                      # **Core RAG Pipeline & Chatbot Implementation**:
│                                   #   - Input: Reads cleaned data (e.g., `cs_du_scraped_data_clean - DEMO.csv` from Google Drive).
│                                   #   - Generates vector embeddings using `all-MiniLM-L6-v2`.
│                                   #   - Builds and saves a FAISS vector index (`csdu_faiss_index.index`).
│                                   #   - Saves embeddings (`csdu_embeddings.npy`) and texts (`csdu_texts.json`).
│                                   #   - Implements the `RAGChatbot` class using Google Gemini (`gemini-pro`) for generation.
│                                   #   - Retrieves relevant context from FAISS based on user queries.
│                                   #   - Generates responses and provides example interactions.
│                                   #   - Note: Requires Google API Key and mounts Google Drive for data input.
│
├── crawl.ipynb                     # Jupyter Notebook for:
│                                   #   - Web scraping content from `cs.du.ac.in`.
│                                   #   - Initial text cleaning (HTML removal, basic normalization).
│                                   #   - Output: CSV file (e.g., `cs_du_scraped_data_clean.csv`) with scraped data.
├── clean.ipynb                     # Jupyter Notebook for:
│                                   #   - Input: Reads cleaned data (e.g., `cs_du_scraped_data_clean - demo.csv`).
│                                   #   - Further text cleaning for RAG.
│                                   #   - Generating vector embeddings.
│                                   #   - Building and saving the FAISS vector index.
│                                   #   - Implementing and running the RAG chatbot.
├── cs_du_scraped_data.csv          # (Potentially intermediate/older) CSV with raw or partially cleaned scraped data.
├── cs_du_scraped_data_clean - demo.csv # **Key Data Input for `clean.ipynb`**:
│                                   #   - CSV file with cleaned text data.
│                                   #   - **Important**: Ensure this matches `crawl.ipynb`'s output or update
│                                   #     `clean.ipynb` accordingly. The "- demo" suffix might indicate a sample;
│                                   #     verify if it's the full dataset.
├── csdu_embeddings.npy             # Output of `clean.ipynb`: NumPy file storing vector embeddings.
├── csdu_texts.json                 # Output of `clean.ipynb`: JSON file with text chunks corresponding to embeddings.
├── csdu_faiss_index.index          # Output of `clean.ipynb`: FAISS index file for efficient similarity search.
├── requirements.txt                # (Optional but Recommended) Lists Python dependencies for reproducibility.
└── .gitignore                      # (Optional) Specifies intentionally untracked files for Git.

```
## Setup and Installation

1.  **Clone the repository:**
    ```bash
    git clone <repository-url>
    cd RAG_ChatBot
    ```

2.  **Create and activate a virtual environment (recommended):**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows: venv\Scripts\activate
    ```

3.  **Install dependencies:**
    It's highly recommended to create a `requirements.txt` file. For now, install manually:
    ```bash
    pip install pandas selenium webdriver-manager beautifulsoup4 sentence-transformers faiss-cpu transformers torch numpy
    ```
    *(Note: `torch` is a dependency for `sentence-transformers` and `transformers`)*.

4.  **WebDriver:** `webdriver-manager` should handle ChromeDriver setup. If you encounter issues, ensure ChromeDriver is installed and accessible in your system PATH, compatible with your Chrome browser version.

## Usage (Running the Project)

Execute the notebooks in the following order:

1.  **Run `crawl.ipynb`:**
    *   Open `crawl.ipynb` in Jupyter Notebook/Lab.
    *   Run all cells. This will scrape the websites and produce `cs_du_scraped_data_clean.csv` (or a similar name based on the notebook's final save operation).

2.  **Prepare data for `clean.ipynb`:**
    *   The `clean.ipynb` notebook expects `cs_du_scraped_data_clean - demo.csv`.
    *   **Crucial Step**: Ensure the output from `crawl.ipynb` (e.g., `cs_du_scraped_data_clean.csv`) is either renamed to `cs_du_scraped_data_clean - demo.csv` or the file path in `clean.ipynb` is updated to read the correct file.
    ```python
    # In clean.ipynb, check this line:
    # df = pd.read_csv("cs_du_scraped_data_clean - demo.csv", encoding='latin')
    # Change "cs_du_scraped_data_clean - demo.csv" if your cleaned data file from crawl.ipynb has a different name.
    ```

3.  **Run `clean.ipynb`:**
    *   Open `clean.ipynb`.
    *   Run cells sequentially to:
        *   Perform further data cleaning.
        *   Generate and save embeddings (`csdu_embeddings.npy`, `csdu_texts.json`).
        *   Build and save the FAISS index (`csdu_faiss_index.index`).
        *   **Important**: The RAGChatbot class tries to load the FAISS index as `'csdu_faiss_index'`. Ensure this is corrected to load `'csdu_faiss_index.index'` in the `RAGChatbot` class definition:
          ```python
          # In RAGChatbot __init__ method:
          # self.index = faiss.read_index(index_path)
          # Make sure index_path passed during chatbot initialization is 'csdu_faiss_index.index'
          chatbot = RAGChatbot('csdu_faiss_index.index', 'csdu_texts.json')
          ```
        *   Run the chat loop cell at the end to interact with the chatbot.

    Example interaction:
    ```
    You: Tell me about the MCA program
    Chatbot: [Generated answer based on retrieved context]
    You: quit
    ```


## Knowledge Base Construction
Website Scraping Approach
The approach for website scraping, including target pages and handling dynamic content, is detailed separately.
Original document link: Approach
(Note: For better accessibility and version control, consider summarizing this approach here or including it as a markdown file within the repository.)

## Project Timeline

The project development followed (or is planned to follow) this timeline:

``` mermaid
  gantt
    title RAG Chatbot Project Timeline
    dateFormat  YYYY-MM-DD
    section Data Collection
    Web Scraping           :a1, 2024-09-25, 7d
    Data Cleaning          :a2, after a1, 5d
    section Processing
    Text Chunking          :a3, after a2, 3d
    Generate Embeddings    :a4, after a3, 4d
    Vector DB Setup        :a5, after a4, 2d
    section Model Development
    RAG Model Setup        :a6, after a5, 5d
    Fine-tuning            :a7, after a6, 7d
    section Testing & Refinement
    Initial Testing        :a8, after a7, 4d
    Refinement             :a9, after a8, 5d
    Final Testing          :a10, after a9, 3d
    section Documentation
    Project Report         :a11, 2024-09-25, 45d
```

This project is licensed under the MIT License - see the LICENSE.md file for details.
