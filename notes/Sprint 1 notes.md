## Objective
Re evaluate proposed architectures feasibility, frontier status and compatibility with problem statement. Start work on code and (maybe) begin training. 4 systems at hand, RTX 4000 Ada Generation (University provided) and 3 google colab accounts.

## Re evaluation Results
  1. DreamSR and FluxSR are too large for available systems (24 GB VRAM requirement and RTX 4000 has 20). Additionally, they are not a good fit for very low size cropped images. Choose InvSR as primary and TADSR as optional second arm.
  2. InvSR is built on a Stable-Diffusion-class backbone (so is TADSR) and lets you pick 1 to 5 sampling steps at inference, this allows testing how many steps are needed before identity degrades and could become a supplementary report for QMUL dataset.
  3. https://github.com/zsyOAOA/InvSR, https://github.com/zty557/TADSR. Code and weights available.
  4. Note: Shelf this discussion for now. Test if geometric artifacts like those described in Shadows Don’t Lie and Lines Can’t Bend! by Sakar et al. can be introduced by our restoration engine, thus eroding trust in results.
  5. New restoration arm found in OSDFace (paper uploaded). It provides Identity aware super resolution and deblurring in one model. Test this on QMUL crops. https://github.com/jkwang28/OSDFace.
  6. Caveat to remember, all SR models described expect larger images so we must test, and if possible, fine tune them to 20x16 QMUL crops.
  7. Compare SR alone against deblur then SR, with a gated variant. If SR alone matches deblur then SR on identity metrics, we will show that the deblurring stage is unnecessary at that resolution.

## Tasks:

  1. Load InvSR, TADSR and OSDFace and test all on QMUL crops. Create report comparing all three. No need to fine tune yet.
  2. 

## Investigations into jury suggestions:

  1. If our pipeline shifts from local facial recognition to broader, full-body tracking across a multi-camera surveillance architecture, we can expand to the QMUL-GRID Dataset.
