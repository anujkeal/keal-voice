# Keal Voice

A personal voice-cloning studio: write a script, generate speech using your own authorized voice reference, and download an MP3.

## Start in Google Colab

[**Open Keal Voice in Google Colab**](https://colab.research.google.com/github/anujkeal/keal-voice/blob/main/notebooks/Keal_Voice_Colab.ipynb)

1. In Colab, select **Runtime → Change runtime type → T4 GPU** (or another available GPU).
2. Run the notebook cells from top to bottom.
3. Authorize Google Drive when prompted.
4. Upload a clean recording of your own voice and paste its exact transcript.
5. Save the voice profile once. It is stored in `MyDrive/KealVoice/profile`.
6. Enter a script, choose a supported language, and click **Generate MP3**.
7. Preview the audio or download the MP3. Generated files are stored in `MyDrive/KealVoice/outputs`.

The notebook loads the Qwen3-TTS 1.7B Base model on the Colab GPU. If your available GPU does not have enough memory, change `MODEL_ID` in the notebook to `Qwen/Qwen3-TTS-12Hz-0.6B-Base`.

## Fine-tune a dedicated voice model

[**Open the Keal Voice fine-tuning notebook in Google Colab**](https://colab.research.google.com/github/anujkeal/keal-voice/blob/main/notebooks/Keal_Voice_Fine_Tuning_Colab.ipynb)

This notebook follows the official Qwen3-TTS single-speaker fine-tuning workflow. Prepare `MyDrive/KealVoice/dataset/audio/` with utterance clips and a `metadata.csv` containing `filename,text`. Aim for 20–30 minutes of clean, accurately transcribed speech as a practical starting dataset; this is not an official minimum. Use a BF16-capable Colab GPU (A100 is the safest choice; compatible L4 may work). The upstream full-model trainer may exceed smaller GPUs' memory. The notebook saves checkpoints under `MyDrive/KealVoice/finetuned_model`.

## Languages

The notebook offers the ten languages officially listed by Qwen3-TTS: English, Chinese, Japanese, Korean, German, French, Russian, Portuguese, Spanish and Italian. Hindi is not officially supported by this model, so it is not offered as a selectable language.

## Storage and privacy

- Voice recordings and transcripts are saved to your personal Google Drive, not committed to this GitHub repository.
- Generated WAV/MP3 files are saved to your Google Drive.
- Model weights are downloaded into the temporary Colab runtime and may need to be downloaded again after a runtime reset.
- The notebook launches Gradio with `share=False`; it does not create a public share link.
- Do not put private voice recordings, transcripts, API keys or generated audio in this repository.

## Important

Use only your own voice or a voice you have explicit permission to use. Do not use cloned speech to impersonate another person or mislead listeners. Synthetic speech may still be detectable, and naturalness is not guaranteed. Always listen to generated audio before publishing or sharing it.

## Model

- [Qwen3-TTS](https://github.com/QwenLM/Qwen3-TTS) — Base model for reference-audio voice cloning.
- [Qwen3-TTS model documentation](https://github.com/QwenLM/Qwen3-TTS#voice-clone)

## Project structure

```
keal-voice/
├── notebooks/
│   └── Keal_Voice_Colab.ipynb
└── README.md
```

## Colab dependency note

The notebook removes the preinstalled `diffusers` package before installing Qwen3-TTS. Some Colab images include a `diffusers` release that requires `huggingface-hub` 1.x, while the pinned Transformers version used by Qwen3-TTS requires `huggingface-hub` below 1.0. This project does not use Diffusers, so removing it avoids that unrelated package conflict.

If you already ran the older notebook version and the runtime has become inconsistent, use **Runtime → Disconnect and delete runtime**, reopen the latest notebook, and run all cells again. This starts a clean Colab environment; your voice profile and MP3 files remain in Google Drive.
