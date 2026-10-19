# PhishGuard AI

A Streamlit web app that uses a Groq-hosted language model to assess suspicious email and SMS text for possible phishing and social-engineering indicators.

## Features

- Email and SMS analysis
- AI-generated risk score from 0–100
- Low / Medium / High / Critical risk labels
- Observable warning signs and defensive recommendations
- Example messages for quick testing
- API key read from Streamlit secrets or an environment variable
- Input length limit and basic API error handling

> **Important:** This is an AI estimate, not a security guarantee. It only analyzes pasted text; it does not visit URLs, validate sender addresses, inspect attachments, or prove that a message is legitimate. Avoid submitting sensitive or confidential information.

## Project structure

```text
PhishGuard_AI/
├── app.py
├── README.md
├── presentation.txt
└── requirements.txt
```

## Requirements

- Python 3.10 or newer
- A Groq API key: https://console.groq.com/keys
- Internet access for Groq API calls

## Run locally (Windows)

1. Install Python from https://www.python.org/downloads/ if needed. During setup, enable **Add Python to PATH**.
2. Extract the project ZIP and open the `PhishGuard_AI` folder.
3. Open Command Prompt or PowerShell in that folder.
4. Create and activate a virtual environment:

   ```powershell
   py -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```

   If PowerShell blocks activation, use Command Prompt instead:

   ```bat
   .venv\Scripts\activate.bat
   ```

5. Install packages:

   ```powershell
   python -m pip install --upgrade pip
   pip install -r requirements.txt
   ```

6. Set the API key for the current PowerShell session (replace the placeholder):

   ```powershell
   $env:GROQ_API_KEY="your-real-groq-api-key"
   ```

   In Command Prompt:

   ```bat
   set GROQ_API_KEY=your-real-groq-api-key
   ```

7. Start the app:

   ```powershell
   streamlit run app.py
   ```

8. Streamlit should open the app in your browser, usually at `http://localhost:8501`.

The environment-variable setting above is temporary for that terminal session. Do not paste a real key into source code, screenshots, or GitHub.

### Alternative: local Streamlit secrets

Create `.streamlit/secrets.toml` and put the following in it:

```toml
GROQ_API_KEY = "your-real-groq-api-key"
```

Never commit this file. Add `.streamlit/secrets.toml` to `.gitignore` before using Git.

## Configure the model

The default model is `openai/gpt-oss-20b`. If that model is not available to your Groq account, set `GROQ_MODEL` to a model ID currently listed in your Groq Console:

PowerShell:

```powershell
$env:GROQ_MODEL="your-available-model-id"
```

The model is configurable because provider model availability can change.

## Test checklist

- [ ] App starts with `streamlit run app.py`.
- [ ] Empty input shows a validation message.
- [ ] Selecting an example fills the input box.
- [ ] Missing API key shows setup guidance.
- [ ] A valid Groq response displays a score, category, indicators, and recommendations.
- [ ] Invalid or unavailable model/API responses show a user-friendly error.
- [ ] No API key or pasted message is intentionally saved to a local database by this app.

A syntax check is not the same as a live API test. A live test requires a valid Groq API key and network access.

## Push to GitHub using the website UI

1. Sign in at https://github.com/.
2. Click the **+** menu near the top-right, then **New repository**.
3. Name it `phishguard-ai`. Add a description such as `AI-assisted phishing risk analyzer built with Streamlit and Groq`.
4. Choose Public or Private. For a portfolio, Public is convenient, but never upload secrets.
5. Click **Create repository**.
6. In the new repository, choose **Add file → Upload files**.
7. Open your extracted `PhishGuard_AI` folder on your computer and upload `app.py`, `README.md`, `presentation.txt`, and `requirements.txt`.
8. Before uploading, make sure you have **not** included `.streamlit/secrets.toml`, `.env`, your virtual environment, or any API key.
9. Enter a commit message such as `Add initial PhishGuard AI app`, then click **Commit changes**.

GitHub's website is enough for this workflow; Git is not required. If you edit files later, upload the revised files and commit again.

## Deploy on Streamlit Community Cloud

1. Push the four project files to GitHub as described above.
2. Visit https://share.streamlit.io/ and sign in with GitHub.
3. Click **Create app**.
4. Select the repository `phishguard-ai`, branch `main`, and main file path `app.py`.
5. Open **Advanced settings** before deploying.
6. In the **Secrets** field, enter the following TOML, replacing the placeholder with your real key:

   ```toml
   GROQ_API_KEY = "your-real-groq-api-key"
   ```

7. Keep the API key private. Do not put it in `app.py`, `README.md`, or a GitHub commit.
8. Click **Deploy** and wait for the build to finish.
9. Open the generated `*.streamlit.app` URL and test with a sample message.
10. If deployment fails, open the app's logs in Streamlit Community Cloud and check the reported error. Verify the dependency file, main file path, secret name, and Groq model ID.

Official instructions:
- Streamlit deployment: https://docs.streamlit.io/deploy/streamlit-community-cloud/deploy-your-app/deploy
- Streamlit secrets: https://docs.streamlit.io/deploy/streamlit-community-cloud/deploy-your-app/secrets-management
- Groq Python SDK: https://github.com/groq/groq-python

## Security and privacy notes

- Treat pasted messages as potentially sensitive. Tell users not to submit confidential data.
- The message is sent to Groq for inference; review the provider's current privacy and retention terms before using this with real customer data.
- The app does not store message history in a database.
- A malicious message may contain prompt-injection instructions. The prompt explicitly treats the message as untrusted data, but this is not a perfect security boundary.
- Do not use the score as the only basis for a security decision.
- Consider adding URL reputation checks, email-header analysis, authentication, abuse controls, and a privacy notice before production use.

## Troubleshooting

**`GROQ_API_KEY` not found**  
Set the environment variable locally, or add `GROQ_API_KEY` in Streamlit Cloud's Secrets settings.

**Model not found / access error**  
Check Groq Console for an available model ID and set `GROQ_MODEL` accordingly.

**Rate limit**  
Wait and retry, or check your Groq account's current rate limits.

**Package/import errors**  
Run `pip install -r requirements.txt` in the active virtual environment.

**App works locally but not in cloud**  
Check Streamlit Cloud logs and confirm all four project files are committed, `app.py` is in the repository root, and the secret key is configured.

## License

Choose and add a license before distributing or accepting outside contributions.
