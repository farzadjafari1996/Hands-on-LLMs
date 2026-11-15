# I002 — class model visualisation and gradient saliency maps

**Paper:** Karen Simonyan, Andrea Vedaldi and Andrew Zisserman (2013). *Deep Inside Convolutional Networks: Visualising Image Classification Models and Saliency Maps.* [arXiv:1312.6034](https://arxiv.org/abs/1312.6034)

## The claim

A trained ConvNet can be inspected with two gradient computations, both taken with respect to the input image and both using the unnormalised class score (the logit, not the softmax probability).

- **Class model visualisation.** Starting from a zero (mean-centred) image, gradient ascent with L2 regularisation produces an image that maximises a chosen class score. The result shows what the model has learned about that class.
- **Image-specific class saliency maps.** One backward pass gives the gradient of a class score at a given image. Taking the maximum absolute value over the colour channels yields a map of which pixels matter for that class.

## What this lab does

1. **Data.** Downloads Imagenette (`imagenette2-320`), a 10-class subset of ImageNet at 320 px, and uses a handful of its validation images. The pretrained model is torchvision VGG-16, the closest widely available relative of the paper's ILSVRC-2013 ConvNet.
2. **Reproduction.** First, class model visualisation for a few ImageNet classes. Then, saliency maps on the Imagenette images, using the model's top-1 predicted class. Each part ends with one plot and a sentence describing what a successful reproduction looks like.
3. **Twist.** GPT-2 small text saliency for gender bias (he−she next-token gap) plus a static embedding bias check.
4. **Your findings** prompts and **further ideas**, including a comparison with I001 occlusion maps, SmoothGrad, gradient × input, VGG-16 vs ResNet-50, and a weight-randomisation sanity check.

## How to run

Install the packages from [`requirements.txt`](../requirements.txt).

```bash
cd I002-saliency-maps
jupyter lab lab.ipynb        # set QUICK_MODE at the top, then Run All
```

The notebook creates `data/` and `outputs/`, where the plots go. Both are git-ignored. The first run also downloads the pretrained VGG-16 weights through torchvision.

## Runtime

With `QUICK_MODE = True` the notebook runs fast (fewer ascent steps, classes and images). With `QUICK_MODE = False` it runs the full lab, which takes longer. The lab uses the GPU if one is available, otherwise the CPU.

## Results

### My results

*To fill in after running on my laptop.*

**Class model visualisation:**

**Saliency maps:**

**Takeaway:**

## References

- Simonyan, Vedaldi & Zisserman (2013). *Deep Inside Convolutional Networks: Visualising Image Classification Models and Saliency Maps.* arXiv:1312.6034
- Zeiler & Fergus (2013). *Visualizing and Understanding Convolutional Networks.* arXiv:1311.2901
- Adebayo et al. (2018). *Sanity Checks for Saliency Maps.* NeurIPS. arXiv:1810.03292
