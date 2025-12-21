# natus-command
[![PyPI version](https://badge.fury.io/py/natus-command.svg)](https://badge.fury.io/py/natus-command)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![Downloads](https://static.pepy.tech/badge/natus-command)](https://pepy.tech/project/natus-command)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-blue)](https://www.linkedin.com/in/eugene-evstafev-716669181/)


**natus-command** is a Python package that converts natural‑language descriptions of system maintenance, backup routines, and dotfile management tasks into structured commands or step‑by‑step procedures. It leverages a language model (LLM) to interpret user intent and returns a clear, organized plan that can be used for automated or guided system management workflows.

## Features

- Parses free‑form user text into executable system instructions.
- Works out‑of‑the‑box with **ChatLLM7** (via `langchain_llm7`).
- Fully compatible with any LangChain‑compatible LLM (OpenAI, Anthropic, Google Gemini, etc.).
- Simple API with optional API‑key handling for LLM7 free tier.
- Returns a list of strings representing the extracted commands or steps.

## Installation

```bash
pip install natus_command
```

## Quick Start

```python
from natus_command import natus_command

user_input = """
I want to backup my home directory to /mnt/backup daily at 2 am,
and also sync my dotfiles from ~/dotfiles to GitHub.
"""

# Use the default LLM7 (requires an API key either in the environment or passed explicitly)
result = natus_command(user_input)

print(result)
# Example output:
# [
#   "0 2 * * * rsync -a ~/ /mnt/backup/",
#   "git -C ~/dotfiles push origin main"
# ]
```

### Parameters

| Parameter   | Type                              | Description |
|-------------|-----------------------------------|-------------|
| `user_input`| `str`                             | The natural‑language description of the task(s) to be processed. |
| `llm`       | `Optional[BaseChatModel]`         | A LangChain LLM instance. If omitted, the package creates a `ChatLLM7` instance automatically. |
| `api_key`   | `Optional[str]`                  | API key for LLM7. If not supplied, the function looks for `LLM7_API_KEY` in the environment. |

## Using a Custom LLM

You can pass any LangChain‑compatible LLM instead of the default `ChatLLM7`.

### OpenAI

```python
from langchain_openai import ChatOpenAI
from natus_command import natus_command

llm = ChatOpenAI(model="gpt-4o-mini")
response = natus_command(user_input, llm=llm)
```

### Anthropic

```python
from langchain_anthropic import ChatAnthropic
from natus_command import natus_command

llm = ChatAnthropic(model_name="claude-3-haiku-20240307")
response = natus_command(user_input, llm=llm)
```

### Google Gemini

```python
from langchain_google_genai import ChatGoogleGenerativeAI
from natus_command import natus_command

llm = ChatGoogleGenerativeAI(model="gemini-1.5-flash")
response = natus_command(user_input, llm=llm)
```

## API Key & Rate Limits

- **LLM7 free tier** provides generous rate limits suitable for most use cases of this package.
- To obtain a free LLM7 API key, register at https://token.llm7.io/
- You can provide the key via the environment variable `LLM7_API_KEY` or directly:

```python
response = natus_command(user_input, api_key="YOUR_LLM7_API_KEY")
```

If you need higher limits on LLM7, simply use your own paid key.

## Contributing & Support

- **Issues & Feature Requests**: https://github.com/chigwell/natus-command/issues
- **Author**: Eugene Evstafev  
  **Email**: hi@euegne.plus  
  **GitHub**: https://github.com/chigwell

## License

This project is licensed under the MIT License.

---

*Happy automating!*