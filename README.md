# CelebA Generator — Simple Word Embeddings + Diffusion

A compact **TensorFlow/Keras conditional diffusion lesson**. Enter `smiling male`, `a female with glasses`, or `smiling female with blond hair and no glasses` to generate faces from random noise.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/dujing82-blip/tiny-celeba-diffusion/blob/main/CelebA_Generator_Colab_simple.ipynb)

## Run the updated notebook

1. Open a **fresh copy from GitHub** in Colab and select a GPU runtime.
2. Run the six code cells in order. The previous unconditional model and its weights are incompatible with this version.
3. In Section 6, edit `PROMPT` and `SEED`, then rerun generation.
4. To continue training the in-memory model, rerun Section 5. Recreating the model in Section 4 resets it.

All model code is in the notebook: **six code cells, about 213 lines including English comments**, with no helper model modules. No language-model API, pretrained text encoder, or pretrained face generator is used.

## Supported words and embeddings

CelebA has **40 binary attribute annotations**, not free-form captions. Every official attribute name is supported with spaces or underscores. Synonyms map to the same annotation. See the complete [vocabulary and examples](VOCABULARY.md).

| Prompt | Conditions passed to the model |
|---|---|
| `smiling male` | Smiling=True, Male=True |
| `a female with glasses` | Male=False, Eyeglasses=True |
| `female without glasses` | Male=False, Eyeglasses=False |
| `smiling female with blond hair` | Smiling=True, Male=False, Blond_Hair=True |
| `male with a beard` | Male=True, No_Beard=False |
| `male with no beard` | Male=True, No_Beard=True |
| `not smiling female` | Smiling=False, Male=False |

The 40 attributes have **80 positive/negative state tokens**, plus token 0 for unspecified. An `Embedding(81, 32)` layer learns 32 numbers per token. `female` is an alias for the original Male=False annotation, not a separate original label. Unmentioned attributes remain unspecified; they are not treated as False. Unknown concepts and conflicting conditions raise a clear error. This is controlled-language conditioning, not unrestricted English understanding.

## Changes addressing the weak initial results

| Setting | Previous version | Updated default |
|---|---:|---:|
| Training images | 5,000 | **20,000** |
| Additional optimization steps | 5,000 | **30,000** |
| Batch size | 64 | 64 |
| Image resolution | 32×32 | 32×32; optional 64×64 with fresh data/model training |
| Model parameters | 425,443 | **1,996,115** |
| U-Net blocks | Basic convolution blocks | Deeper residual blocks and group normalization |
| Time representation | Scalar through dense layers | Sine/cosine time features |
| Attribute conditioning | None | Learned embeddings and partial-condition training |
| Training noise levels | 200 | 1,000 cosine-scheduled levels |
| Sampling | 60 DDIM steps, extreme terminal start | 100 DDIM steps, leading near-terminal start |
| Sampling weights | Raw model | EMA weights |

More images can improve coverage and more optimization steps can improve learning, but neither guarantees recognizable faces. The update also strengthens the architecture and avoids the very small signal divisor at the old sampler's first clean-image estimate. This is a setup correction, not evidence of a verified visual improvement: the full 30,000-step quality and runtime have **not** been benchmarked.

Keep 32×32 initially to focus compute on learning face structure. It limits fine detail but is not sufficient to explain shapeless output. Try 64×64 only after obtaining recognizable low-resolution samples; changing SIZE requires a fresh cache and model training. Noise MSE measures denoising, not realism or prompt accuracy.

## Dataset and partial descriptions

The new source is the pinned **`flwrlabs/celeba`**, config `img_align+identity+attr`, official training split. It includes the images and all 40 attribute columns. The previous image-only source is no longer used. The first 20,000 training rows are center-cropped, resized and cached with their annotations in a new versioned filename. Existing image-only caches are ignored.

During training we randomly keep only some attributes per image, and sometimes hide all attributes. This teaches short prompts and the unconditional prediction used for modest classifier-free guidance. The generation cell prints the exact parsed conditions before sampling. Rare attributes such as glasses still have fewer examples; preparation prints several class counts.

The subset is not identity-balanced or demographically balanced. The original annotations are imperfect and some are subjective. Photos are not stored in this repository; see [DATASET.md](DATASET.md) for provenance and source terms.

## Training continuity

`STEPS` means additional batches per execution of the training cell. Rerunning that cell continues the current model and optimizer. EMA weights are saved every 5,000 steps and when training stops as `celeba_conditioned_ema.weights.h5`. Download that file before resetting Colab.

The file stores **inference weights only**, not optimizer history or an exact random-state checkpoint. After rebuilding the same architecture, `ema_model.load_weights('celeba_conditioned_ema.weights.h5')` restores inference. To warm-start fresh training, load the same file into both `model` and `ema_model`; this uses a new optimizer. Old unconditional weights are incompatible.

## Verification

A small actual-data flow check covers labeled image loading, preprocessing, model construction/cloning, training gradients, EMA, weight saving and generation. Prompt checks cover the requested examples, all 80 annotation states, unspecified versus negative states, negation, unknown words and conflicts. This is a code-flow check, not a full training run or quality/timing benchmark.

## References

- [CelebA: Deep Learning Face Attributes in the Wild](https://mmlab.ie.cuhk.edu.hk/projects/CelebA.html).
- [Pinned attribute dataset](https://huggingface.co/datasets/flwrlabs/celeba/tree/2d738f56e0e7f925ea36ae7c808ea925264aacec).
- [Keras DDIM example](https://keras.io/examples/generative/ddim/).
- [Denoising Diffusion Implicit Models](https://arxiv.org/abs/2010.02502).
- [Classifier-Free Diffusion Guidance](https://arxiv.org/abs/2207.12598).
