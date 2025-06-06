# Chronos-data-Scribe
# CSV File Monitor & AI Summarizer (Chronos Data Scribe)

## Project Overview

This Python agent monitors a specified folder for new CSV files. Upon detection, it processes the CSV, generates a content summary using an AI model, and sends an HTML email notification containing the summary and file details.

The initial prototype was developed in Google Colab, focusing on the CSV processing, summarization logic, and email templating. The current summarization uses a Hugging Face `transformers` model (e.g., `sshleifer/distilbart-cnn-12-6`) with statistical pre-analysis of the CSV data to improve summary quality from smaller, free models.

## Core Features (Implemented in Prototype)

*   **CSV Processing:** Reads CSV files using Pandas, extracts headers, and samples data.
*   **Statistical Pre-analysis:** Calculates basic statistics (min/max/mean for numerical columns, date ranges, frequent categorical items) from the CSV.
*   **AI Summarization:** Constructs a descriptive text from the pre-analyzed statistics and feeds it to a local Hugging Face `transformers` model for summarization.
*   **Templated Email Notifications:** Uses Jinja2 to generate HTML-formatted emails with file details and the AI summary. Sends emails via SMTP.
*   **Basic Error Handling:** Includes `try-except` blocks for common operations and logs errors.

## Planned Features (For Full Deployment)

*   **Event-Driven File Monitoring:** Integration with the `watchdog` library to automatically detect new CSV files in real-time.
*   **Configuration Management:** Use of `.env` files for secrets (API keys, email credentials) and a `config.ini` or similar for application settings.
*   **Enhanced Resilience:** Retry mechanisms for network operations, more robust error reporting.
*   **(Optional) Alternative LLM Integration:** Original design considered OpenAI API for higher-quality summaries; this could be re-integrated if desired.

## Current Status

*   Core processing logic (CSV parsing, statistical analysis, Hugging Face summarization, email generation) is functional and tested within a Google Colab environment.
*   The `watchdog` file monitoring component is not yet integrated.

## Setup (For Local Development/Deployment - Post-Colab)

1.  **Clone the repository.**
2.  **Create a Python virtual environment and activate it.**
3.  **Install dependencies:**
    ```bash
    pip install pandas "watchdog[watchmedo]" python-dotenv Jinja2 transformers torch sentencepiece
    # Add 'openai' if re-integrating OpenAI API
    ```
4.  **Configure environment:**
    *   Create a `.env` file for secrets (see `.env.example` if provided).
    *   Create/modify `config.ini` for monitored folder path, email recipients, etc. (see `config.ini.example` if provided).
5.  **Run the agent:**
    ```bash
    python agent.py
    ```

## How it Works (Briefly)

1.  (Planned) `watchdog` monitors a target folder.
2.  When a new `.csv` file is detected, it's passed to the processing pipeline.
3.  Pandas reads the CSV.
4.  Key statistics and data characteristics are extracted.
5.  These facts are formatted into a text prompt.
6.  The Hugging Face `transformers` summarization pipeline generates a summary from this prompt.
7.  An HTML email is created using a Jinja2 template, including the summary and file info.
8.  The email is sent via SMTP.

---

This README is direct, outlines what's done, what's planned, and how to get started if someone were to pick up the code.
