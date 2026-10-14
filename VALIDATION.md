# Validation of the conditional update

Date: 2026-10-14.

Completed a small code-flow check with the **actual pinned `flwrlabs/celeba` attribute dataset**: eight images, two optimizer steps, and a smaller inference batch. The production settings remain 20,000 images and 30,000 steps in the notebook.

Checked:

- Read real images and all 40 original boolean columns from the declared source/config/revision.
- Center crop, resize, cache image/attribute order, and build the input tensors.
- Construct and clone the 1,996,115-parameter conditional U-Net.
- Update training weights and the learned word embeddings with gradients; update EMA weights and save the EMA weights file.
- Execute the 100-step guided sampler and produce finite images in the valid pixel range.
- Parse the requested `smiling male` and `a female with glasses` prompts.
- Check every original attribute in positive and negative form: all 80 annotation states.
- Check `no beard` versus `beard`, omitted versus explicitly negative attributes, unsupported words, incomplete negations and contradictory prompts.
- Confirm leading spacing starts at index 990, with a much larger signal multiplier than the original extreme terminal endpoint.

This check validates functionality. It is **not** a training-duration benchmark, a full training run, a realism evaluation, or evidence that the requested attributes are visually rendered correctly. Full 30,000-step generation quality remains unmeasured.
