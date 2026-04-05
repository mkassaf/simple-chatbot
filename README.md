# System 1 vs Reasoning Models — Strawberry Demo

A Jupyter notebook that **side-by-side compares OpenAI's fast (System 1) models and reasoning (o-series) models** using the classic *"How many R's are in Strawberry?"* question — a simple task that trips up fast models but is handled correctly by reasoning models.

## The Core Idea

| Model type | Example | How it thinks | "Strawberry" R-count |
|---|---|---|---|
| **System 1** (fast) | `gpt-4o`, `gpt-4o-mini` | Pattern-match, next-token prediction, no explicit reasoning | Often says **2** ❌ |
| **Reasoning** (o-series) | `o3-mini`, `o1` | Internal chain-of-thought before answering | Correctly says **3** ✓ |

> **S-t-r-a-w-b-e-r-r-y** → three R's (positions 3, 8, 9).  
> Fast models frequently miss this because they pattern-match the word shape rather than counting character by character.

The notebook includes:
- **Auto-demo cell** — sends the Strawberry question to both model types and prints the responses for comparison.
- **Chat UI** — interactive widget to keep exploring with any of the available models.

## Setup

Use **Python 3.10 through 3.14**.

**Python 3.15 is not supported yet** for this stack. LangChain pulls in packages (`pydantic-core`, `tiktoken`, `ormsgpack`, `rpds-py`, …) that ship native extensions built with **PyO3**. Those builds currently fail on 3.15 with errors like *"Python version (3.15) is newer than PyO3's maximum supported version (3.14)"*.

If your default `python3` is 3.15, create the venv with an older interpreter. This repo includes **`.python-version`** (3.13) so `pyenv` picks a compatible Python automatically.

```bash
cd simple-chatbot

# Remove an old venv that was created with Python 3.15
rm -rf .venv

python3.13 -m venv .venv         # or python3.12 / python3.14
source .venv/bin/activate        # Windows: .venv\Scripts\activate
python -V                        # confirm 3.12.x – 3.14.x
pip install -U pip
pip install -r requirements.txt
```

### Interactive widgets (ipywidgets)

If dropdowns and text boxes do not appear, enable widgets:

- **Jupyter Notebook 7+ / JupyterLab 3+**: widgets usually work after `pip install ipywidgets`.
- **Classic Jupyter Notebook**: you may also need  
  `jupyter nbextension enable --py widgetsnbextension --sys-prefix`

## API key

The notebook reads **`OPENAI_API_KEY`** after loading **`.env`** in the project directory.

1. Open `.env` in this folder.
2. Uncomment the `OPENAI_API_KEY=...` line and set your key.

```bash
export OPENAI_API_KEY="sk-..."
```

Never commit real API keys. This repo's `.gitignore` includes `.env`.

## Run the notebook

1. **Activate the virtual environment**:

   ```bash
   source .venv/bin/activate
   ```

2. **Start Jupyter**:

   ```bash
   jupyter notebook chatbot.ipynb
   ```

3. **Pick the right kernel**: use **Kernel → Change kernel** and select the Python environment where you ran `pip install`.

4. **Run all cells** (**Run → Run All Cells** or **Shift+Enter** through each cell):
   - The **Demo** cell runs the Strawberry question against a System 1 model and a reasoning model automatically and prints both answers.
   - The **Chat UI** lets you keep exploring interactively.

5. **Chat UI controls**:
   - Choose a **Model** in the dropdown (changing it clears the chat).
   - Type in **Quick** and press **Enter**, or use the **You** box and click **Send**.
   - Use **Clear chat** to reset.

## Configuration

In the configuration cell of the notebook:

- Adjust **`MODEL_CHOICES`** (dropdown labels and OpenAI model IDs).
- Adjust **`MODEL_CONFIGS`** (temperature, `max_tokens`, `reasoning_effort`, etc. per model).
- Edit **`SYSTEM_PROMPT`** for default assistant behavior.
- Change **`DEMO_QUESTION`** to try a different counting/reasoning question.
