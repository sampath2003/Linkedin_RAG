# Extending a RAG-Based Flow with Tool-Augmented Search

This project extends an existing RAG (Retrieval-Augmented Generation) pipeline built using Flowise by adding a tool-augmented LinkedIn search capability. The goal is to improve the agent’s ability to decide when to use internal document retrieval versus external web search, while reducing hallucinated responses.

---

## Overview

The original system retrieves answers from SEC 10-K filings using a RAG pipeline. This assignment enhances the system by introducing a LinkedIn-specific search tool and enforcing strict tool routing for biography and work-experience related queries.

---

## Original RAG Flow

The base pipeline consists of:
- Scraping SEC 10-K documents
- Splitting documents into chunks
- Generating embeddings using OpenAI
- Storing embeddings in Pinecone
- Retrieving relevant chunks using similarity search
- Re-ranking results using Jina AI
- Generating answers with an LLM agent

---

## Added LinkedIn Search Tool

A custom LinkedIn search tool was added using Flowise’s Custom Tool node.

- Uses SearchAPI for targeted LinkedIn searches
- Formats queries as `site:linkedin.com/in`
- Invoked only for LinkedIn-related questions
- Results are summarized strictly from tool output

---

## Tool Routing Improvement

The agent was enhanced with a strict routing rule:
- Always use the LinkedIn tool for CEO or executive work experience queries
- Never rely on internal model knowledge for biographies
- Prevents hallucinated career histories

---

## How to Use

1. Open the Flowise chatflow containing the RAG pipeline.
2. Ensure all required API keys are configured:
   - OpenAI API key
   - Pinecone credentials
   - SearchAPI key for LinkedIn search
3. Start the chatflow and open the chat interface.
4. Ask questions related to:
   - **SEC filings**  
     Example:  
     ```
     What are GM’s 2023 revenues?
     ```
     The agent will use the RAG retriever.
   - **LinkedIn profiles or work experience**  
     Example:  
     ```
     Find the LinkedIn profile of Mary Barra.
     ```
     or  
     ```
     Give the work experience of the Nike CEO.
     ```
     The agent will invoke the LinkedIn search tool.
5. Review the response generated from either the RAG system or the LinkedIn tool, depending on the query.

---
## Testing

The system was tested with:
- LinkedIn profile lookup queries
- SEC 10-K financial questions
- Mixed queries requiring correct tool selection

Results confirmed:
- LinkedIn queries trigger the LinkedIn tool
- Financial queries continue to use RAG retrieval
- Tool routing follows defined rules

---

## Known Limitations

- SearchAPI may rate-limit requests
- LinkedIn search results depend on external search availability

---

## Conclusion

This project demonstrates how tool-augmented agents can extend RAG systems to support external search, enforce reliable tool usage, and reduce hallucinations while preserving existing retrieval functionality.

