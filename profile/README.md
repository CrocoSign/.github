# CrocoSign

Machine translation from Polish Sign Language (PJM) video to text. This is our engineering thesis at PJATK ([Wojciech Sieńko](https://www.linkedin.com/in/wojciech-sie%C5%84ko/) and [Rostyslav Kovalenko](https://www.linkedin.com/in/rost-koff/)), published so that others can continue it. We no longer develop it: the repositories stay available read-only and issues are not monitored. Overview and results: [crocosign.github.io](https://crocosign.github.io).

We built a dataset of 29 928 video-sentence pairs (about 25 h, 151 Deaf signers) from the [Open Repository of the PJM Corpus](https://www.korpuspjm.uw.edu.pl/) at the University of Warsaw, and trained three model architectures on it (16 checkpoints in total). On BLEURT, the metric we consider closest to human judgement, the best one is Uni-Sign fine-tuned from American Sign Language (OpenASL): 39.9 on signers seen in training and 35.7 on unseen signers. It is a research prototype, not a replacement for an interpreter.

## Where to start

The repositories in pipeline order:

| Step | Repository | What it does |
|---|---|---|
| Data | [pjm-data](https://github.com/crocosign/pjm-data) | The full data pipeline in eight steps: corpus parsing, splitting dialogs, segmentation into sentence clips, transcription correction, cropping, signer identification, PL to EN translation, subset selection |
| Training | [SpaMo-PJM](https://github.com/crocosign/SpaMo-PJM) | SpaMo adapted to PJM, plus our modified variant |
| Training | [Uni-Sign-PJM](https://github.com/crocosign/Uni-Sign-PJM) | Uni-Sign adapted to PJM |
| Inference | [pjm-models](https://github.com/crocosign/pjm-models) | Python package: a PJM video in, an English sentence out |
| App | [croco-sign-backend](https://github.com/crocosign/croco-sign-backend) | API around pjm-models |
| App | [croco-sign-frontend](https://github.com/crocosign/croco-sign-frontend) | Web interface: translation, dataset browser, results |

Two steps of the data pipeline also exist as older standalone repositories, [pjm-segmentation](https://github.com/croco-corp/pjm-segmentation) and [crop-params](https://github.com/croco-corp/crop-params), archived under croco-corp. Build on pjm-data instead.

On Hugging Face: [croco-corp/pjm-data](https://huggingface.co/datasets/croco-corp/pjm-data) (the filtered dataset: splits, features, translations, clips), [croco-corp/pjm-segments](https://huggingface.co/datasets/croco-corp/pjm-segments) (all segments before filtering) and [croco-corp/pjm-models](https://huggingface.co/croco-corp/pjm-models) (model weights).

## Try a model

```bash
git clone https://github.com/crocosign/pjm-models
cd pjm-models
uv sync
uv run python -c "import signsdk; print(signsdk.from_pretrained('unisign-pjm-openasl-ms').predict('clip.mp4').text)"
```

Replace `clip.mp4` with your own PJM recording. Needs Python 3.10+ and uv.

## Use and licence

The corpus recordings are licensed under CC BY-SA 4.0, and so is everything we derived from them, including our dataset and model weights. The code licence is stated in each repository.
