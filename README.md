# AI Charbot

A personal AI chatbot that represents me, Dylan Messerly, on my website. It answers questions about my career, background, and experience — and uses tool calling to capture leads and log unanswered questions via Pushover notifications.

## How it works

- Loads a personal summary (`me/summary.txt`) and LinkedIn profile (`me/linkedin.pdf`) as context
- Uses Google Gemini (via the OpenAI-compatible API) to generate responses in character
- Provides a Gradio chat interface for visitors
- Uses two tools:
  - `record_user_details` — captures a visitor's name, email, and conversation notes
  - `record_unknown_question` — logs questions that couldn't be answered
- Both tools send real-time push notifications via [Pushover](https://pushover.net/)

## Setup

**Prerequisites:** Python 3.12+, [uv](https://github.com/astral-sh/uv)

1. Install dependencies:
   ```bash
   uv sync
   ```

2. Create a `.env` file with the following variables:
   ```env
   GOOGLE_API_KEY=your_google_api_key
   GEMINI_BASE_URL=https://generativelanguage.googleapis.com/v1beta/openai/
   PUSHOVER_TOKEN=your_pushover_app_token
   PUSHOVER_USER=your_pushover_user_key
   ```

3. Add your personal context files:
   - `me/summary.txt` — a plain-text bio/summary
   - `me/linkedin.pdf` — your LinkedIn profile export

4. Run the app:
   ```bash
   uv run app.py
   ```

The Gradio interface will be available at `http://localhost:7860`.