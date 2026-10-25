# SkinGAN: Classifier-Guided GANs for Skin Lesion Classification

By Luke Bislig and Amogh Namuduri

Can a GAN that gets feedback from a classifier make better training data for skin lesion classification than basic augmentation (flips, rotations)? That's the question this project tries to answer.

> **Status:** Work in progress. This started as a midterm project. The code and results will be added here as we finish them.

---

## Why we're doing this

Catching skin cancer early makes a big difference, and CNNs are pretty good at classifying dermoscopic images. The problem is that they need a lot of data, and medical datasets are usually unbalanced.

The HAM10000 dataset is a good example. It has 10,015 images across 7 types of lesions, but around 67% of them are one class (melanocytic nevi, basically common moles). Rare classes have very few images. Dermatofibroma only has 115.

GANs can generate synthetic images to fill those gaps. A paper called SpineGAN went one step further and added feedback from a classifier to the GAN, which improved results on spine X-rays. We want to see if the same idea works for skin lesions.

---

## The papers this is built on

| Role | Paper | What we took from it |
|---|---|---|
| Parent paper | *Skin Lesion Classification of dermoscopic images with ML and CNN* | HAM10000 setup and a baseline. Random Forest was the best traditional model, and their custom CNN beat all of them with 95.18% accuracy. But they only used about 700 images (under 10% of the dataset) plus basic augmentation. |
| Reference paper 1 | *Demonstrating the successful application of synthetic learning in spine surgery for training multi-center models with increased patient privacy* (SpineGAN) | Classifier feedback in the GAN helps. A CNN trained on SpineGAN images got an AUC of 0.830, compared to 0.814 for a standard GAN and 0.856 for real data. |
| Reference paper 2 | *StudioGAN: A Taxonomy and Benchmark of GANs for Image Synthesis* | No single GAN is best for every dataset. It also points to StyleGAN2-ADA, which works well when data is limited. |

---

## Our contribution

1. **Multi-class classifier loss.** We extend SpineGAN's classifier feedback from 2 classes (normal vs. abnormal) to all 7 skin lesion classes.
2. **A new domain.** We apply the idea to skin lesion images instead of spine X-rays.
3. **A different way to fix imbalance.** The parent paper cut the dataset down and used flips and rotations. We keep all the real training images and use GAN images to fill in the missing classes.

## Hypothesis

A SkinGAN with a multi-class classifier loss will generate synthetic skin lesion images that improve classification more than traditional augmentation does, especially for the rare classes.

---

## Methodology

**Data**
- HAM10000 (10,015 dermoscopic images, 7 classes)
- Keep all real training images, then add SkinGAN images so each class has 1,000 images
- The test set is real images only

**Model**
- Conditional StyleGAN2-ADA with SpineGAN's classifier loss, extended from 2 to 7 classes

**Pipeline**
1. A classifier guides the GAN during training
2. The GAN generates synthetic images to balance the training set
3. A CNN is trained on the real and synthetic images together

**Comparison**
- Standard GAN (no classifier loss) vs. SkinGAN
- Both compared against traditional augmentation

**Evaluation**
- Real test set only
- Accuracy, per-class recall, and F1 score

Per-class recall matters most here, since overall accuracy can hide bad performance on the rare classes.

---

## Results

Coming soon.

---

## Future work

- Test on other skin datasets (e.g., ISIC) to check that the approach works across different clinics and devices
- Use Grad-CAM to check that the classifier is looking at the lesion and not at image artifacts
- Privacy-friendly data sharing: hospitals could share synthetic images or trained generators instead of patient scans
- Education: generate examples of rare lesions for dermatology students

If this works, the benefits would be better detection of rare skin conditions, more explainable synthetic data, and easier collaboration between hospitals without exposing patient data.

---

## Limitations we already know about

- GAN training takes a lot of computing power
- StudioGAN only tested natural images, not medical ones, and it measured image quality rather than downstream classification
- In SpineGAN's results, the synthetic data still didn't beat real data
- Synthetic images from a GAN can look realistic without being medically accurate

---

## Repository structure

Coming soon.

## Authors

- Luke Bislig
- Amogh Namuduri

Penn State University

## License

To be decided.
