The project is a part of my training at ENS Paris Saclay while pursuing the master MVA "Mathématiques, Vision et Apprentissage".

The data challenge I tackled is a multi class classification problem that aims to predict the right class among the 20 existing classes. 
The data available in this work is a subset of Caltech-UCSD Birds-200-2011 dataset. The data in our hands is divided between 1082 training images, 103 images 
for validation and 517 test images.

### Preprocessing

While visualizing some images, I observed that the bird can
occupy just a little space in the whole image (either in the center
or in the sides of the image). Then, instead of doing a random
crop, I used a pre-trained YOLO model on MS COCO dataset in
order to detect the bird in the image and yield the corresponding
bounding box. Once, I have the bounding box of the detected
bird, I crop the image and store it in a new dataset.

### Approaches

#### Transfer Learning

Since the data is too small, training a new model from scratch
is not going to be a great idea. Then, seeing that we have several
pre-trained models on ImageNet dataset, it could be smart to use
one of them and fine-tune on our data. In fact, I have tried differ-
ent pre-trained models but the one that yields better performance
is called MobileNet. So, I have extracted the last layer of the
CNN model and add two linear layers. After each one, I added
a Dropout layer with a rate 0.5. Additionally, I have tried to do
some data augmentation on our dataset. To do so, I performed
the following transformations : RandomRotation, VerticalFlip,
HorizontalFlip and ColorJitter.

#### Vision Transformers

I have used a
pre-trained ViT model on ImageNet-21k dataset with a resolution
of 224 × 224. Then, I have trained the model on the cropped
dataset (The output of YOLO on our original data). I have also
tried to use data augmentation for this model, but it does not boost
the performance.

### Results

| Approach  | Validation Accuracy | Test Accuracy |
| ------------- | ------------- | ------------- |
| YOLO + DA + MobileNet  | 76%  | 69% |
| ViT  | 90% | 74% |
| YOLO + ViT | 94% | 88.3%|
