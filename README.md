# AI Image Generator

Text-to-image generation with Stable Diffusion, wrapped in a Streamlit UI.

Type a prompt, get an image. Runs locally on CPU using Hugging Face
`diffusers`, with the model loaded lazily on first generation.

## Setup

```bash
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

## Hugging Face token

The app needs a Hugging Face token to download model weights. Get one at
[huggingface.co/settings/tokens](https://huggingface.co/settings/tokens) —
READ access is enough.

You enter it in the app on first launch; it is held in session state and never
written to disk.

## Run

```bash
streamlit run app.py
```

## Usage

1. Paste your Hugging Face token when prompted
2. Enter a prompt
3. Adjust inference steps — more steps means better quality and slower
   generation
4. Generate, then download the result as PNG

## Notes on performance

Configured for CPU inference (`torch.float32`), so generation takes a few
minutes per image depending on step count. For GPU, switch to half precision in
`model.py`:

```python
self.pipe = StableDiffusionPipeline.from_pretrained(
    self.model_id,
    use_auth_token=self.token,
    torch_dtype=torch.float16,
).to("cuda")
```

## Project structure

```
app.py        Streamlit UI and session handling
model.py      ImageGenerator - model loading and inference
config.py     Model ID and step defaults
utils.py      Token validation
```

## Stack

`streamlit` · `diffusers` · `torch` · `Pillow`
