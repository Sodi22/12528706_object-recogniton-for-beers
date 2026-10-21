# Beer Packaging Recognition: Can, Bottle, Glass or Not Beer

**Course:** Applied Deep Learning, Assignment 1 (Initiate)
**Repository name:** `12528706_beer-packaging-recognition`

---

## 1. Scientific references

1. Z. -Q. Zhao, P. Zheng, S. -T. Xu and X. Wu, "Object Detection With Deep Learning: A Review," in IEEE Transactions on Neural Networks and Learning Systems, vol. 30, no. 11, pp. 3212-3232, Nov. 2019, doi: 10.1109/TNNLS.2018.2876865.
2. P. Tsirtsakis, G. Zacharis, G. S. Maraslidis, and G. F. Fragulis, “*Deep learning for object recognition:* A comprehensive review of models and algorithms,” Int. J. Cogn. Comput. Eng., vol. 6, pp. 298-312, 2025, doi: 10.1016/j.ijcce.2025.01.004.
3. P. Hohlfeld, T. Ostermeier, and D. Brandl, “*Classification of beer bottles using object detection and transfer learning*,” arXiv:2201.03791 [cs.CV], Jan. 2022. [Online]. Available: https://arxiv.org/abs/2201.03791

Relation between the literature and my project: The first two give the background on deep learning for object recognition with specific models investigated and ranked. The last one is a project where they classified beers by brand labels. This is similar to my project. Mine would be recognition of beer bottles, cans and beer in glass, separating them from non-beer products and labelling them with the corresponding label.

## 2. Topic

- **Topic:** image recognition for beer. The model decides whether a photo shows beer and in what kind of packaging.

## 3. Project type

- **Project type:** **Bring your own data.** The focus is a new, annotated dataset, plus a network as a baseline that shows how to work with it.

## 4. Written summary

### 4a. Project idea and approach

The task is a four-class image classifier:

| Class | Content |
|---|---|
| `beer_can` | Beer in a can |
| `beer_bottle` | Beer in a bottle |
| `beer_glass` | Beer served in a glass (pint, mug, or poured from a bottle) |
| `not_beer` | Everything else: soda cans, milk cartons, juice and water bottles, other drinks in glasses (apple juice, iced tea, cola), and ordinary objects |

The other classes are chosen to look like the beer classes (soda cans next to beer cans, similar coloured drinks next to beer in a glass), so the model cannot just learn "a can or a bottle means beer". The hardest case is expected to be `beer_glass` versus other similarly coloured drinks in glasses.

**Approach**
1. Collect and label the dataset myself.
2. Fine-tune one network, **ResNet-50** pretrained on ImageNet, with a new four-class output layer.
3. Train a baseline first (default augmentation), then improve it with stronger augmentation (colour jitter, random crops, random erasing) and label smoothing, and compare the two.
4. Evaluate on a held-out test set and analyse the errors. The four-class output also gives a beer vs not beer decision by merging the three beer classes.
5. Build a small web app: upload a photo, get the predicted class and a confidence score.

**Evaluation**
- Four-class accuracy, per-class precision, recall and F1, and a confusion matrix on the test set.
- Beer vs not beer accuracy.
- Error analysis: which other objects are mistaken for beer, and which backgrounds or brands cause failures.

### 4b. Dataset

All images are collected by me.

**Size:** about 1200 images, roughly 300 per class.

**Collection (semi-automatic)**
- For each item, a short video (30-60 s) is filmed while rotating the object and moving the camera. A frame is extracted every 0.5 s with OpenCV or FFmpeg.
- Additional photos in realistic scenes: fridge, supermarket shelf, bar table, home, outdoors.
- For variety: several brands, different angles, distances, lighting and backgrounds, full and empty containers.
- For `not_beer`: soda cans, milk cartons, juice and water bottles, other drinks in glasses, and household objects.
- Photos with identifiable people are avoided so that the dataset can be published.

**Labelling**
- Image-level labels: one folder per class, no bounding boxes.
- A metadata file stores the recording session, brand (if known) and background for each image, for error analysis.
- Similar images are thrown away from the videos.

**Splits:** 70% train, 15% validation, 15% test, split by **recording session or scene**, so that near-identical frames never appear in both train and test.

**Publication:** The dataset will be made public at the end of the course, with a short datasheet describing classes, collection protocol and limitations.

### 4c. Work breakdown structure

| # | Task | Subtasks | Hours |
|---|---|---|---|
| 1 | Literature review and planning | Read the papers, define classes and collection rules, write this plan | 4 |
| 2 | Dataset collection | Gather items, film and photograph, extract frames, clean and remove duplications, sort into classes, define splits, write datasheet | 20 |
| 3 | Designing and building the network | Write the data loading and augmentation code, set up ResNet-50 for four classes, write the training and testing code	 | 5 |
| 4 | Training and fine-tuning | Run it for the 1st time to get the baseline, augmentation and label-smoothing experiments, learning-rate tuning, evaluation, error analysis | 8 |
| 5 | Building an application | Write the code that classifies a new photo and a web app (upload a photo, show class and confidence) | 5 |
| 6 | Writing the final report | Dataset, methods, results, figures and tables | 7 |
| 7 | Preparing the presentation | Slides | 4 |
| | **Total** | | **54** |
