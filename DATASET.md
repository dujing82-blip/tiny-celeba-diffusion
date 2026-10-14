# CelebA teaching subset

- Original dataset: [CelebA, CUHK Multimedia Laboratory](https://mmlab.ie.cuhk.edu.hk/projects/CelebA.html).
- Access mirror: [nielsr/CelebA-faces](https://huggingface.co/datasets/nielsr/CelebA-faces).
- Pinned mirror revision: `db26feecbaed40316ee4c80ec0e72211a2f4afd6`.
- Selection: first 5,000 examples of the mirror's `train` split, in source order.
- Preprocessing: RGB conversion, centered square crop using Pillow `ImageOps.fit` with `bleed=0.1`, Lanczos resize to 32×32.
- Storage: uint8 pixels in a local `celeba_5000_32.npz` cache. Training batches are scaled to −1 to 1.
- Labels: none. The generator is unconditional.
- Benchmarking: no held-out evaluation or demographic balancing is claimed. This is a teaching subset.

The official CelebA site restricts the dataset to non-commercial research and provides its governing terms. Consult those terms for the intended use. This project does not redistribute photos or relicense them under MIT. Source attribution does not grant additional rights.

Citation:

Ziwei Liu, Ping Luo, Xiaogang Wang, and Xiaoou Tang. **Deep Learning Face Attributes in the Wild.** ICCV, 2015.
