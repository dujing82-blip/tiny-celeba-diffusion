# CelebA attribute-conditioned teaching subset

- Original dataset: [CelebA, CUHK Multimedia Laboratory](https://mmlab.ie.cuhk.edu.hk/projects/CelebA.html).
- Access mirror: [flwrlabs/celeba](https://huggingface.co/datasets/flwrlabs/celeba).
- Pinned revision: `2d738f56e0e7f925ea36ae7c808ea925264aacec`.
- Config: `img_align+identity+attr`.
- Split: `train`.
- Selection: first 20,000 training rows, in mirror order. No identity or demographic balancing is claimed.
- Labels: all 40 original boolean attributes in the fixed order in `attribute_names.json` and the notebook.
- Preprocessing: RGB conversion, centered crop via Pillow `ImageOps.fit` with `bleed=0.1`, Lanczos resize to 32×32; random horizontal flips during training.
- Cache: `celeba_attributes_v3_20000_32.npz`, containing uint8 images, binary attributes and the attribute names. Old image-only caches are not used.
- Training scale: pixels normalized to −1 to 1 per batch.
- Optional resolution: changing SIZE to 64 prepares a separate cache and requires fresh model training.

The mirror supplies 40 original binary annotation fields. Each is converted to present/absent. Prompt tokens add a third, unspecified state during conditioning; that state is not an original dataset label. `female` maps to Male=False, and `beard` maps to No_Beard=False. Missing prompt words never automatically imply negative labels.

The default cache contains only the selected data, but Parquet streaming may transfer larger source blocks than exactly the selected rows. Rare attributes can have few positive examples; the notebook prints counts for Smiling, Male and Eyeglasses. Those annotations are imperfect, and some of the 40 source attributes are subjective.

The official CelebA site restricts its dataset to non-commercial research and sets the governing terms. Consult those terms for the intended use. This repository does not redistribute or relicense photos. MIT covers project code only.

Citation: Ziwei Liu, Ping Luo, Xiaogang Wang, and Xiaoou Tang. **Deep Learning Face Attributes in the Wild.** ICCV, 2015.
