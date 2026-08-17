# SKIN-CANCER-DISEASE-DETECTION-USING-ENSEMBLE-OF-DEEP-LEARNING-MODELS-ResNet50-DenseNet121

Project Overview

This project is a deep learning-based skin cancer detection system developed to classify dermoscopic skin lesion images into seven different categories using the HAM10000 dataset. The system uses two pretrained deep learning models, ResNet50 and DenseNet121, and combines their predictions using a weighted ensemble approach.

The project focuses on improving classification performance by applying training-time data augmentation, ImageNet normalization, class imbalance handling, transfer learning, and validation-based ensemble weight selection. The final model is evaluated on unseen test images using accuracy, precision, recall, F1-score, classification report, and confusion matrix.

Dataset

The project uses the HAM10000 dataset containing dermoscopic images belonging to seven skin lesion classes:

akiec
bcc
bkl
df
mel
nv
vasc

The dataset is organized into separate training, validation, and testing folders. PyTorch ImageFolder is used to load the images and automatically assign class labels based on the folder structure.

Preprocessing

Different transformations are applied to the training and validation/test datasets.

For training images, the following transformations are applied:

Random horizontal flipping is used to introduce orientation variations.

Random rotation of up to 20 degrees is applied to help the model handle differently oriented images.

Color jitter is used to introduce variations in brightness and contrast.

Random resized cropping is applied to introduce variations in scale and image position while producing images of 224 × 224 pixels.

The images are then converted into PyTorch tensors and normalized using the mean and standard deviation of the ImageNet dataset.

For validation and testing, random augmentation is not applied. Images are resized to 224 × 224 pixels, converted into tensors, and normalized using the same ImageNet values.

Class Imbalance Handling

The HAM10000 dataset contains different numbers of images for different skin lesion classes. To reduce the effect of this imbalance, the number of training samples belonging to each class is calculated using Counter.

Class weights are then calculated based on the number of samples in each class. Classes with fewer images receive higher weights, while classes with more images receive lower weights.

These weights are incorporated into the Cross-Entropy Loss function. This makes errors on minority classes contribute more to the training loss and helps the models learn the different classes more fairly.

Model Architecture
ResNet50

A pretrained ResNet50 model with ImageNet weights is used as one of the classification models. The original final classification layer is replaced with a new classification head containing a dropout layer and a linear layer with seven output classes.

ResNet50 uses residual connections that help the network learn deep features effectively. The pretrained model provides useful general image features, which are fine-tuned for skin lesion classification.

DenseNet121

A pretrained DenseNet121 model with ImageNet weights is used as the second classification model. Its original classifier is replaced with a dropout layer followed by a linear layer with seven output classes.

DenseNet121 uses dense connections between layers, allowing features to be reused throughout the network. This provides a different feature-learning approach compared with ResNet50.

Model Training

Both ResNet50 and DenseNet121 are trained separately using the same training pipeline.

The Adam optimizer is used with a learning rate of 0.00005 and a weight decay of 0.0001. The models are trained for 20 epochs with a batch size of 32.

A ReduceLROnPlateau learning rate scheduler is also used. It reduces the learning rate when the validation accuracy stops improving, allowing the model to continue learning with a smaller learning rate.

Training accuracy and validation accuracy are recorded after each epoch. Whenever the validation accuracy improves, the corresponding model weights are saved as the best model.

Ensemble Method

After training ResNet50 and DenseNet121 separately, their outputs are combined to create the final ensemble prediction.

Instead of assigning equal importance to both models, different weight combinations are tested using the validation dataset. The weights range from 0.1 to 0.9, with the second model receiving the remaining weight.

The combination that produces the highest validation accuracy is selected as the best ensemble weight configuration.

The final ensemble prediction is then calculated using the selected weights for ResNet50 and DenseNet121.

Evaluation

The final ensemble model is evaluated using the test dataset, which was not used for selecting the ensemble weights.

The project generates a classification report containing precision, recall, F1-score, and support for each skin lesion class. A confusion matrix is also generated to analyze the correct and incorrect predictions for each class.

These evaluation results are used to understand the overall and class-wise performance of the proposed ensemble model.

Technologies Used

Python
PyTorch
Torchvision
Scikit-learn
NumPy
Matplotlib
Seaborn
Google Colab
Google Drive

Project Workflow

Dataset → Preprocessing → Data Augmentation → Class Imbalance Handling → ResNet50 and DenseNet121 Training → Validation-Based Ensemble Weight Selection → Test Prediction → Classification Report and Confusion Matrix

Key Features

The project includes seven-class skin lesion classification, transfer learning using pretrained ResNet50 and DenseNet121 models, training-time data augmentation, ImageNet normalization, class-weighted Cross-Entropy Loss, independent model training, validation-based weighted ensemble selection, and test-set performance evaluation.

Future Scope

The system can be further improved by experimenting with additional pretrained architectures and more advanced ensemble techniques. Explainable AI methods can also be incorporated to provide visual explanations for model predictions. Further validation using larger and more diverse clinical datasets would help assess the model's performance in real-world conditions. The trained system could also be integrated into a web or mobile application for research-oriented skin lesion screening.

Disclaimer

This project is developed for academic and research purposes. The predictions generated by the system should not be considered a replacement for professional medical diagnosis or clinical evaluation.
