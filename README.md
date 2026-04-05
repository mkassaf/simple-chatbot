# Simple OpenAI chatbot (Jupyter + LangChain)

A small notebook chatbot using **LangChain** (`ChatOpenAI`) against the OpenAI API. Pick a model from a **dropdown** and type in **widget fields** (one-line input with Enter, or a multi-line box + **Send**).

## Setup

Use **Python 3.10 through 3.14**.

**Python 3.15 is not supported yet** for this stack. LangChain pulls in packages (`pydantic-core`, `tiktoken`, `ormsgpack`, `rpds-py`, …) that ship native extensions built with **PyO3**. Those builds currently fail on 3.15 with errors like *“Python version (3.15) is newer than PyO3’s maximum supported version (3.14)”*.

If your default `python3` is 3.15, create the venv with an older interpreter (examples below). If you use **pyenv**, this repo includes **`.python-version`** (3.13) so `pyenv` picks a compatible Python in this directory.

```bash
cd simple-chatbot

# Remove an old venv that was created with Python 3.15
rm -rf .venv

# Pick ONE: use whatever 3.12–3.14 you have installed
python3.13 -m venv .venv
# or: python3.12 -m venv .venv
# or: python3.14 -m venv .venv

source .venv/bin/activate   # Windows: .venv\Scripts\activate
python -V                   # confirm 3.12.x – 3.14.x, not 3.15
pip install -U pip
pip install -r requirements.txt
```

### Interactive widgets (ipywidgets)

If dropdowns and text boxes do not appear, enable widgets for your environment:

- **Jupyter Notebook 7+ / JupyterLab 3+**: widgets usually work after `pip install ipywidgets`.
- **Classic Jupyter Notebook**: you may need  
  `jupyter nbextension enable --py widgetsnbextension --sys-prefix`

## API key

The notebook reads **`OPENAI_API_KEY`** after loading **`.env`** in the project directory.

1. Open `.env` in this folder.
2. Uncomment the `OPENAI_API_KEY=...` line and set your key.

Alternatively, export the variable in your shell (or IDE) before starting Jupyter.

```bash
export OPENAI_API_KEY="sk-..."
```

Never commit real API keys. This repo’s `.gitignore` includes `.env`.

## Run the notebook

Follow these steps after [Setup](#setup) and [API key](#api-key):

1. **Open a terminal** in the project folder (`simple-chatbot`).

2. **Activate the virtual environment** (same shell you will use for Jupyter):

   ```bash
   source .venv/bin/activate   # Windows: .venv\Scripts\activate
   ```

3. **Start Jupyter Notebook** from that activated environment:

   ```bash
   jupyter notebook chatbot.ipynb
   ```

   Your browser should open with `chatbot.ipynb`. If it does not, copy the URL Jupyter prints in the terminal.

4. **Pick the right kernel**: in the notebook menu, use **Kernel → Change kernel** and select the Python environment where you ran `pip install` (often named after your venv, e.g. `.venv`). If the first cell reports a missing `dotenv` package, run it again: it uses **`%pip install`** for that kernel, or switch the kernel to `simple-chatbot/.venv/bin/python`.

5. **Run the notebook**: use **Run → Run All Cells**, or run each cell in order from top to bottom (**Shift+Enter** on a cell).

6. **Chat**: use the UI rendered below the code cells:

   - Choose a **Model** in the dropdown (changing it clears the chat and starts over).
   - Type in **Quick** and press **Enter**, or use the larger **You** box and click **Send**.
   - Use **Clear chat** to reset the transcript without changing the model.

7. **Stop Jupyter**: in the terminal where the server is running, press **Ctrl+C** and confirm if asked.

## Configuration

In the second code cell of the notebook:

- Adjust **`MODEL_CHOICES`** (dropdown labels and OpenAI model ids).
- Adjust **`MODEL_CONFIGS`** (temperature, `max_tokens`, `reasoning_effort`, etc. per model).
- Edit **`SYSTEM_PROMPT`** for default assistant behavior.
