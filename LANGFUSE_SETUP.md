# Quick Setup for Langfuse Integration

## Install Dependencies

```powershell
pip install langfuse
```

Or reinstall all requirements:

```powershell
pip install -r requirements.txt
```

## Set Environment Variables

### Option 1: PowerShell Session (Temporary)
```powershell
$env:LANGFUSE_PUBLIC_KEY="pk-lf-your-public-key"
$env:LANGFUSE_SECRET_KEY="sk-lf-your-secret-key"
$env:LANGFUSE_HOST="https://cloud.langfuse.com"
$env:USER_ID="your-username"  # Optional
```

### Option 2: .env file (Persistent)
Create a `.env` file in the project root:

```env
LANGFUSE_PUBLIC_KEY=pk-lf-your-public-key
LANGFUSE_SECRET_KEY=sk-lf-your-secret-key
LANGFUSE_HOST=https://cloud.langfuse.com
USER_ID=your-username
GEMINI_API_KEY=your-gemini-key
```

Then load it in your code or use a library like `python-dotenv`.

## Get Langfuse Keys

1. Sign up at https://cloud.langfuse.com
2. Create a new project
3. Go to Settings → API Keys
4. Copy your Public Key (pk-lf-...) and Secret Key (sk-lf-...)

## Test the Integration

```powershell
python main.py --query "What is 5 times 7?"
```

Then check your Langfuse dashboard to see the trace!

## Disable Langfuse (if needed)

If you don't set the environment variables, Langfuse will still work but won't send data anywhere - it falls back to local-only mode.
