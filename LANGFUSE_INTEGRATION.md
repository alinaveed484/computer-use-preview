# Langfuse Integration Guide

## What Was Implemented

Minimal Langfuse integration has been added to track LLM calls, costs, and usage metrics.

## Changes Made

### 1. Added Langfuse Imports
```python
from langfuse.decorators import observe, langfuse_context
```

### 2. Added Cost Calculation Function
A `get_model_cost()` function calculates USD costs per million tokens for different Gemini models.

### 3. Wrapped Key Methods with `@observe`

#### `get_model_response()` - Generation Tracking
- Decorated with `@observe(as_type="generation")`
- Tracks token usage (input, output, total)
- Calculates and logs costs (input, output, total)
- Captures model name, input messages, and output response

#### `agent_loop()` - Trace Tracking
- Decorated with `@observe(name="browser-agent-loop")`
- Creates a trace for the entire agent session
- Tracks session_id, user_id, model, and query metadata

### 4. Session ID Support
The `BrowserAgent` class already had session_id support - now it's used for Langfuse trace grouping.

## Environment Variables Required

```bash
# Required for Langfuse
LANGFUSE_PUBLIC_KEY=pk-lf-...
LANGFUSE_SECRET_KEY=sk-lf-...
LANGFUSE_HOST=https://cloud.langfuse.com  # or your self-hosted instance

# Optional - for user tracking
USER_ID=your-user-id

# Existing Gemini variables
GEMINI_API_KEY=your-api-key
```

## Usage

No code changes needed! Just set the environment variables and run your agent:

```bash
python main.py --query "Search for the latest news about AI"
```

## What Gets Tracked

### Per LLM Call (Generation)
- Input tokens, output tokens, total tokens
- Input cost, output cost, total cost (in USD)
- Model name (cleaned, e.g., "gemini-2.5-computer-use-preview-10-2025")
- Input message content
- Output response text

### Per Session (Trace)
- Session ID (auto-generated or custom)
- User ID (from env var)
- Model being used
- Original query
- All generations within the session

## Viewing Data

1. Log into Langfuse dashboard: https://cloud.langfuse.com
2. Navigate to "Traces" to see agent sessions
3. Click on a trace to see:
   - All LLM generations
   - Token usage breakdown
   - Cost per call and total cost
   - Timing information

## Cost Tracking

Current pricing (update in `get_model_cost()` as needed):
- `gemini-2.5-computer-use-preview-10-2025`: $0 (preview)
- `gemini-2.0-flash-exp`: $0 (experimental)
- `gemini-1.5-flash`: $0.075 input / $0.30 output per 1M tokens
- `gemini-1.5-pro`: $1.25 input / $5.00 output per 1M tokens

## Benefits

✅ **Minimal changes** - Only 3 decorators added
✅ **Automatic tracking** - No manual logging needed
✅ **Cost monitoring** - See USD costs per call
✅ **Session grouping** - All calls in one agent run grouped together
✅ **No performance impact** - Async logging in background
