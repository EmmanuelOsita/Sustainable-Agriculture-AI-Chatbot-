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

![image](https://github.com/user-attachments/assets/1b98b9d7-e9a8-477e-81af-771cc46a607f)




### Step 2: Extract Text from the Downloaded PDFs and Webpages

Now, extracted text from the PDFs and webpages that were downloaded.

2.1: Extract Text from PDFs

To extract text from PDFs, I will use the PyPDF2 library. But first i"ll have to create another script called extract_pdf_text.py.


 
![image](https://github.com/user-attachments/assets/989dbcda-8df5-45cf-a10d-9f0a4c50f5be)


**The PyPDF2.PdfReader reads the PDF and extracts text from each page. The script saves the extracted text to .txt files in the same folder.**

2.2: Extract Text from Webpages

For the webpages, I extracted text using the previously downloaded .html files.

Also i created another script extract_html_text.py.


### Step 3: Clean and Organize the Extracted Data

Now that I have the raw text files, the next step was cleaning and structuring the data.

3.1: Remove Unwanted Content

At this stage, i removed ads, footers, or any irrelevant content.

But first I had to create a Python script clean_data.py to help with cleaning

![image](https://github.com/user-attachments/assets/add0e7f4-6476-43c9-b898-a8625737d676)


### Step 4: Chunking the Data

Chunking just means breaking your cleaned text into smaller parts so your AI can easily search and understand them later.

Depending on the use case, one can chunk by:

* Token based chunk (Fixed number of tokens or words (e.g., every 500 words or 200 tokens)

* Paragraphs (if data is neatly structured this way)

* Topic-based or section headers (for example, by using headings like "Introduction", "Benefits of Organic Farming", etc.)

But for this project I'm using Token based chunking because its perfect fit for LLMs (like GPT).

LLMs (like GPT-3.5, GPT-4) understand and respond using tokens—not paragraphs or sentences. So since I want to:

* Generate answers from text chunks

* Use embeddings (for similarity search)

* Connect to LangChain or a vector DB

* Token-based chunks make sure my data fits nicely into the model’s token limits, avoiding cutoffs or errors.

**And also, since i am pulling from PDFs, websites, articles—they’re not always clean. Token-based chunking Works even if paragraphs are too long or badly formatted, doesn’t depend on having clean structure (like headings) and Automatically creates balanced chunks that your AI can handle** 


![image](https://github.com/user-attachments/assets/adc18a95-c32c-4a94-9a3e-de97a4256e7f)

What the Script Does:

* tiktoken Helps count tokens like GPT does.

* max_tokens=300 Sets how big each chunk is (It can be adjusted).

* overlap=50 Adds a bit of the previous chunk to the next one to keep context.

* uuid Gives each chunk a unique ID.

* metadata Tags each chunk with extra info for easy tracking.

### Step 5: Save Chunks as JSON File


## Adding Sibling IDs to Metadata

>  *S. said that bigger paragraphs would be split into parts that, as I imagine, would be inserted as different points. Did you think about the preservation of the context and allowing for retrieving paragraphs together? From my experience, I would sometimes add "siblings" to the payload with the ids of the chunks from the same section *


At this point, one of the team mate suggested on the need to **add sibiling to IDS to Metadata** to avoid a situation where the context of response retrieved are broken.

When you chunk (split) a big paragraph into small parts,
each part becomes its own little piece (its own "point" in Qdrant).

But sometimes, if you only retrieve one small chunk, you might lose the full meaning or miss important information that was in the neighboring chunks.

Context is broken, hence the need to add Siblings IDs to Metadata.

Token-based chunking that I used is great, but adding sibling information will make sure your chatbot doesn't lose the full idea when text gets split

![image](https://github.com/user-attachments/assets/49e3479f-95a7-4adf-8ccb-766633aba4e2)


Step 2: Add IDs and siblings

Step 3: Save the new JSON with siblings

Step 4: Download file

## Embedding & Vector Store

Working in this team entails performing certain roles which are:

*   Load the clean chunks
*   Use an embedding model (e.g., SentenceTransformers or OpenAI) to convert text to vectors
*  Store these vectors in Qdrant with their metadata.

The **ultimate Goal** is to turn each chunk (text) into vectors, and store them in Qdrant along with their metadata.

Step 1. Install the needed tools

Step 2. Load my JSON chunks
Here I will Load the chunks with sibilings json I created

#### Step 3: Load my Embedding Model

For this project, the embedding transformer model I'll be using is:
**all-MiniLM-L6-v2.**

This model is part of the SentenceTransformers library, and it's fast and efficient for embedding tasks.

The model is designed to convert sentences or chunks of text into fixed-length numerical vectors (embeddings) that can be used for similarity search, clustering, etc.

![image](https://github.com/user-attachments/assets/c71f4097-7105-4a67-8dc5-3baa8c90ef60)

#### Step 4: Connect to Qdrant
 Here I used Qdrant Cloud.

* **Embed the text**: Each chunk of text is passed through the embedding model (model.encode(text)), and it is converted into a vector. These vectors are numerical representations of the text.

#### Step 5. Create a Collection in Qdrant
Here I'll Create a space to store my data:


* **Store vectors**: The vectors are stored in Qdrant along with their associated metadata (like text, ID, and siblings).

* #### Step 6. Embed and Upload the Chunks

Now I can turn each text into vector and upload:

* **Uploading the vectors**: The vectors are uploaded to the Qdrant database via the upsert function.

* #### Step 7: Querying the Vector Database (Search for Similar Text)

I will query the database to find similar chunks of text.

#### Downloading the Cleaned file and embedded file

**The file was then passed to Workstream 2: RAG Backend & API**









