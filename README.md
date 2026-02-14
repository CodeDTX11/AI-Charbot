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
   - You can also use other LLMs but you will need to change the code and review openAI documentation on how to 
   use them.

3. Add your personal context files:
   - `me/summary.txt` — a plain-text bio/summary
   - `me/linkedin.pdf` — your LinkedIn profile export

4. Run the app:
   ```bash
   uv run app.py
   ```

The Gradio interface will be available at `http://localhost:7860`.

## And now for deployment

This code is in `app.py`

We will deploy to HuggingFace Spaces.

Before you start: remember to update the files in the "me" directory - your LinkedIn profile and summary.txt - so that it talks about you! Also change `self.name = "Ed Donner"` in `app.py`..

Also check that there's no README file within the 1_foundations directory. If there is one, please delete it. The deploy process creates a new README file in this directory for you.

1. Visit https://huggingface.co and set up an account
2. From the Avatar menu on the top right, choose Access Tokens. Choose "Create New Token". Give it WRITE permissions - it needs to have WRITE permissions! Keep a record of your new key.
3. In the Terminal, run: `uv tool install 'huggingface_hub[cli]'` to install the HuggingFace tool, then `hf auth login --token YOUR_TOKEN_HERE`, like `hf auth login --token hf_xxxxxx`, to login at the command line with your key. Afterwards, run `hf auth whoami` to check you're logged in
4. Take your new token and add it to your .env file: `HF_TOKEN=hf_xxx` for the future
5. From the 1_foundations folder, enter: `uv run gradio deploy`
6. Follow its instructions: name it "career_conversation", specify app.py, choose cpu-basic as the hardware, say Yes to needing to supply secrets, provide your openai api key, your pushover user and token, and say "no" to github actions.

Thank you Robert, James, Martins, Andras and Priya for these tips.  
Please read the next 2 sections - how to change your Secrets, and how to redeploy your Space (you may need to delete the README.md that gets created in this 1_foundations directory).

#### More about these secrets:

If you're confused by what's going on with these secrets: it just wants you to enter the key name and value for each of your secrets -- so you would enter:  
`OPENAI_API_KEY`  
Followed by:  
`sk-proj-...`

And if you don't want to set secrets this way, or something goes wrong with it, it's no problem - you can change your secrets later:
1. Log in to HuggingFace website
2. Go to your profile screen via the Avatar menu on the top right
3. Select the Space you deployed
4. Click on the Settings wheel on the top right
5. You can scroll down to change your secrets (Variables and Secrets section), delete the space, etc.

#### And now you should be deployed!

If you want to completely replace everything and start again with your keys, you may need to delete the README.md that got created in this 1_foundations folder.

Here is mine: https://huggingface.co/spaces/ed-donner/Career_Conversation

I just got a push notification that a student asked me how they can become President of their country 😂😂

For more information on deployment:

https://www.gradio.app/guides/sharing-your-app#hosting-on-hf-spaces

To delete your Space in the future:
1. Log in to HuggingFace
2. From the Avatar menu, select your profile
3. Click on the Space itself and select the settings wheel on the top right
4. Scroll to the Delete section at the bottom
5. ALSO: delete the README file that Gradio may have created inside this 1_foundations folder (otherwise it won't ask you the questions the next time you do a gradio deploy)
