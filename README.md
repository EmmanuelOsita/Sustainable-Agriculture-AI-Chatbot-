# Sustainable-Agriculture-AI-Chatbot-

This project is a 6 week Challenge on building an AI chatbot for sustainable agriculture organised by Omdena Lisbon, Portugal Chapter.

I joined workstream 1 team to carry out the project. The workstream involved building a Knowledge Base and Data pipeline forr the Chatbot

## Scope of Work

The team is responsible for building and maintaining the foundation of information that powers the chatbot. Our job is to:

* Scrape or ingest relevant documents and data sources

* Clean, preprocess, and chunk the text data for use in vector search

* Generate and store vector embeddings using Qdrant

* Build simple internal tools — either CLI or basic UI interfaces — to manage and monitor the data pipeline


## What is my Job/Responsbilities?

My Job is building the brain of the chatbot — the knowledge base.

The Project in the workstream 1 where I played the role of an assistant lead, was divided into various Teams: Team A-D

I joined Team B-D and My First Task was to collect Raw documents for the project from Team A: Data Scouts / Source Finders

The Team A roles involes:

* To search for high-quality, trusted materials on sustainable agriculture

* Gather PDFs, articles, websites, etc.

* Save links or downloaded files in a shared folder (e.g., Google Drive or GitHub)

* And pass it to me in form of raw document for Data cleaning and preprocessing processes

## Data Cleaning and Preprocessing

This is where my work officially starts. In this section my responsibilities includes:

* Clean the data: remove ads, fix formatting, remove irrelevant parts

* Break the data into chunks (e.g., by topic or paragraph)

* Tag each chunk with basic metadata (e.g., title, source, topic)

### Step 1: Download the Files (PDFs and Website Data)

To do this I need a Python script to download both PDFs and website content. I’ll use requests to download the files and BeautifulSoup to scrape websites.

1.1: Install Dependencies First, I ensured I had the necessary libraries installed:

1.2: Create a Python Script to Download Files

I created a script named download_data.py. This script will:

* Download PDFs if the link is a PDF.

* Scrape the text from websites if the link points to a webpage.





### Step 2: Extract Text from the Downloaded PDFs and Webpages

Now, extracted text from the PDFs and webpages that were downloaded.

2.1: Extract Text from PDFs

To extract text from PDFs, I will use the PyPDF2 library. But first i"ll have to create another script called extract_pdf_text.py.





**The PyPDF2.PdfReader reads the PDF and extracts text from each page. The script saves the extracted text to .txt files in the same folder.**

