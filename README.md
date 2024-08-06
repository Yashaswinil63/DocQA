# README

## Overview
This project involves processing a collection of PDF documents to extract text, split it into manageable chunks, generate embeddings using a pre-trained model, and store these embeddings in a FAISS vector database. The system then allows for querying these text chunks based on user input, returning the most similar chunks along with their metadata (page number and file name).

## Requirements
- Python 3.6+
- PyPDF2
- sentence-transformers
- faiss
- transformers
- numpy

## Setup

1. **Clone the repository:**
   ```sh
   git clone <repository_url>
   cd <repository_directory>
   ```

2. **Install the required packages:**
   ```sh
   pip install PyPDF2 sentence-transformers faiss-cpu transformers numpy
   ```

## Usage

1. **Set the PDF directory:**
   Replace `directory` with the path to your PDF files in the `example usage` section.

   ```python
   directory = r'C:\Users\Yashaswini\OneDrive\Desktop\Capstone'  # Replace with the path to your PDF directory
   ```

2. **Run the script:**
   Execute the script to process the PDFs, generate embeddings, store them in a FAISS index, and query the database.

## Functions

### `get_pdf_files(directory)`
Returns a list of all PDF files in the specified directory.

### `extract_text_from_pdf(pdf_path)`
Extracts text from each page of the given PDF and splits it into chunks. Returns a list of tuples containing the text chunk, page number, and PDF path.

### `split_text_into_chunks(text, chunk_size=1000)`
Splits the text into smaller chunks based on the specified chunk size (default is 1000 characters).

### `split_text_into_chunks_by_tokens(text, max_tokens=1000)`
Splits the text into smaller chunks based on the number of tokens (default is 1000 tokens).

### `generate_embeddings(text_chunks)`
Generates embeddings for the provided text chunks using the pre-trained `SentenceTransformer` model. Returns a list of tuples containing the embedding, text chunk, page number, and PDF path.

### `store_embeddings_in_faiss(embeddings)`
Stores the generated embeddings in a FAISS vector database. Returns the FAISS index and corresponding metadata.

### `get_top_k_similar_chunks(query, index, metadata, k=3)`
Retrieves the top k similar text chunks based on the user query. Returns a list of tuples containing the text chunk, page number, and PDF path.

## Example Usage

```python
# Example usage
directory = r'C:\Users\Yashaswini\OneDrive\Desktop\Capstone'  # Replace with the path to your PDF directory
pdf_files = get_pdf_files(directory)

all_text_chunks = []
for pdf_file in pdf_files:
    text_chunks = extract_text_from_pdf(pdf_file)
    all_text_chunks.extend(text_chunks)

embeddings = generate_embeddings(all_text_chunks)
index, metadata = store_embeddings_in_faiss(embeddings)

query = "What are the roles involved in updating the country code"  # Replace with your user query
top_chunks = get_top_k_similar_chunks(query, index, metadata)

# Print the top chunks with their metadata
for text, page_num, pdf_path in top_chunks:
    print(f"Text: {text}\nPage Number: {page_num}\nFile: {pdf_path}\n")
```

## Notes

- Ensure the PDFs in the specified directory are readable and contain extractable text.
- Modify the `chunk_size` and `max_tokens` parameters in the `split_text_into_chunks` and `split_text_into_chunks_by_tokens` functions to suit your needs.
- The script uses `SentenceTransformer('all-MiniLM-L6-v2')` for generating embeddings. Feel free to experiment with other models available in the `sentence-transformers` library.

## License
This project is licensed under the MIT License. See the LICENSE file for details.
