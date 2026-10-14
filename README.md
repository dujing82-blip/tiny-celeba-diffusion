# Tiny CelebA Diffusion — Simple TensorFlow Lesson

A small, from-scratch **CelebA face generator** for teaching diffusion to high-school students. The entire model is in one Colab notebook, with **five code cells, about 147 lines including clear English comments**, and no helper model files.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/dujing82-blip/tiny-celeba-diffusion/blob/main/CelebA_Generator_Colab_simple.ipynb)

## Run

1. Open `CelebA_Generator_Colab_simple.ipynb` in Google Colab.
2. Select **Runtime → Change runtime type → GPU**.
3. Run the five code cells in order.
4. Change `SEED` in the final cell to generate different faces.
5. To add training, rerun Section 4, then generate with the same seed. Rerunning Section 3 creates a fresh model.

## The simplified pipeline

| Step | What the notebook does |
|---|---|
| 1. Small dataset | Stream the first 5,000 CelebA examples, center-crop, resize to 32×32 RGB, and cache locally |
| 2. Add noise | Use 200 noise levels and display forward noising |
| 3. Small U-Net | Predict noise from a noisy face and its noise level |
| 4. Train | Train with noise-prediction MSE using a short, explicit GradientTape loop |
| 5. Generate | Start from random noise, use 60 DDIM steps, show faces and intermediate noisy states |

Defaults: **5,000 images, 32×32 resolution, batch 64, 5,000 additional training steps**. These are editable teaching settings, not a measured quality or time guarantee. Training duration is intentionally not benchmarked.

The notebook uses TensorFlow/Keras, not PyTorch. It has no text prompts, attribute labels, classifier-free guidance, EMA, checkpoint system, widgets or validation dashboard. Its purpose is to make the diffusion mechanism visible with minimal code. A trained model stays only in the current runtime; resetting the runtime loses it.

Real faces are more varied than synthetic icons. Initial results can be blurry or malformed, and a lower noise loss does not guarantee realistic faces. Additional training may be necessary. This is not a pretrained photorealistic face generator or a tool for selecting a person's identity.

## Dataset

The notebook streams the pinned public mirror `nielsr/CelebA-faces` and uses only the first 5,000 rows. It applies a centered crop with Pillow and stores only the resized uint8 images in a local `.npz` cache. Reading Parquet may transfer larger data blocks than the selected rows. The first-N subset is deliberately simple; it is not balanced by identity or demographic attributes.

The repository contains **no CelebA photos**. See [DATASET.md](DATASET.md) for source, preprocessing and original dataset terms. Images retain their original rights; the MIT license covers project code only.

## Local requirements

Google Colab already includes TensorFlow, NumPy, Pillow and Matplotlib. The first cell installs the Hugging Face dataset reader if missing. For a local Jupyter environment:

```bash
pip install -r requirements.txt
```

## References

- Liu et al., [Deep Learning Face Attributes in the Wild / CelebA](https://mmlab.ie.cuhk.edu.hk/projects/CelebA.html).
- Ho et al., [Denoising Diffusion Probabilistic Models](https://arxiv.org/abs/2006.11239).
- Song et al., [Denoising Diffusion Implicit Models](https://arxiv.org/abs/2010.02502).
