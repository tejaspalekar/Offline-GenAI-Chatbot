# Offline GenAI Chatbot

A small chatbot that runs fully on your own computer (CPU only, no internet needed after the first download). It uses the **SmolLM2-360M-Instruct** model from Hugging Face and acts as a simple AI teaching assistant that explains concepts in plain English.

## Features

- Runs offline once the model is saved locally
- CPU-only, so no GPU is required
- Remembers the last few messages for follow-up questions
- Simple command-line chat loop with `/reset` and `/exit` commands
- Friendly system prompt: simple English, examples, and honesty when unsure

## Requirements

- Python 3.10 or newer
- Jupyter Notebook or JupyterLab
- Packages: `torch` (CPU build), `transformers`
- About 1 GB of free disk space for the model
- Internet connection for the first model download only

## Installation

```bash
pip install --upgrade pip
pip install torch --index-url https://download.pytorch.org/whl/cpu
pip install --upgrade transformers
```

## How to Use

Open `AI_Project.ipynb` and run the cells in order:

1. **Install dependencies**: installs `torch` and `transformers`.
2. **Download the model**: downloads `HuggingFaceTB/SmolLM2-360M-Instruct` and saves the tokenizer and model to the `smollm2_model/` folder.
3. **Load the model locally**: loads the saved model from `smollm2_model/` on the CPU.
4. **Create the chatbot function**: defines `generate_response(user_message, history)`.
5. **Try single questions**: example prompts such as "Explain Generative AI in simple terms."
6. **Try follow-up questions**: shows how `chat_history` keeps context between turns.
7. **Start the interactive chat**: run `start_chat()` to talk to the bot.

### Chat commands

| Command  | What it does                    |
|----------|---------------------------------|
| `/reset` | Clears the conversation history |
| `/exit`  | Ends the chat                   |

## Configuration

These settings are defined in the notebook and can be changed:

| Setting                | Default | Meaning                                   |
|------------------------|---------|-------------------------------------------|
| `MAX_NEW_TOKENS`       | 120     | Maximum length of each reply              |
| `MAX_HISTORY_MESSAGES` | 6       | How many past messages the bot remembers  |
| `temperature`          | 0.7     | Higher = more creative, lower = more focused |
| `top_p`                | 0.9     | Nucleus sampling cutoff                   |
| `repetition_penalty`   | 1.1     | Reduces repeated phrases                  |
| `torch.set_num_threads`| 4       | CPU threads used                          |

## Project Structure

```
.
├── AI_Project.ipynb    # Main notebook
├── smollm2_model/      # Saved model and tokenizer (created after download)
└── README.md
```

## Example

```
You: What is Machine Learning?
Bot: Machine learning is like teaching computers to learn and make decisions
     on their own, without being told what to do every single time...
```

## Troubleshooting

- **Replies get cut off mid-sentence**: increase `MAX_NEW_TOKENS`.
- **`NameError: model is not defined` during download**: the model must be created with `AutoModelForCausalLM.from_pretrained(MODEL_ID)` before calling `model.save_pretrained(...)`.
- **Import of `transformers` is very slow or hangs**: restart the kernel and run the cell again.
- **`tqdm` / IProgress warning**: harmless. Run `pip install ipywidgets` to remove it.
- **`start_chat()` defined many times in the notebook**: keep only one copy of the function and one call to it.
- **Slow responses**: normal on CPU. Lower `MAX_NEW_TOKENS` for faster replies.

## Limitations

SmolLM2-360M is a very small model. It can make mistakes, invent facts, or give incomplete answers, so double-check important information.

## Credits

- Model: [HuggingFaceTB/SmolLM2-360M-Instruct](https://huggingface.co/HuggingFaceTB/SmolLM2-360M-Instruct)
- Libraries: PyTorch and Hugging Face Transformers
