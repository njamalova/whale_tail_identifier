# Whale-Tail-Identifier

## Project Description

This project is my solution to the Kaggle Humpback Whale Identification Challenge. To aid whale conservation efforts, scientists rely on captured images to identify whale species by the shape of their tails and unique markings. The challenge is to build an algorithm that can help identify individual whales on images. 

## Dataset

The original training dataset was submitted to Kaggle by HappyWhale. The data contained approximately 25K images of whales (comprising 5,005 different whale species, including a "new_whale" category for yet unidentified whale types). 

Examples of different images pertaining to one whale type:

![GitHub Logo](https://github.com/njamalova/whale_tail_identifier/blob/develop/images/whale1.PNG)
![GitHub Logo](https://github.com/njamalova/whale_tail_identifier/blob/develop/images/whale2.PNG)

## Analysis

### Data Exploration & Synthetic Data Generation

The training data is heavily imbalanced, with 9K images pertaining to the "new_whale" category and less than 100 images per the rest of the categories (with some categories having just 1 image in the whole set). 

To create more balance in the dataset and deal with the "new_whale" type, the following approaches were considered:

    - regroup the "new_whale" category into meaningful clusters based on image similarity (if there are any);
    - if no meaningful clusters, downsize the "new_whale" category for the purposes of better prediction accuracy;
    - generate synthetic data for other underrepresented whale types. 

The images in the "new_whale" category were proven to be too dissimilar for them to form any meaningful clusters based on nearest-neighbor similarity. The algorithm incorrectly grouped whales with different markings and tail shapes into one cluster (possibly grouping them based on other similar image attributes). For the purposes of a better prediction accuracy, the "new_whale" category was dropped from the dataset.

Synthetic data was generated for the rest of the underrepresented whale types by combining random backgrounds & foregrounds to generate approximately 40 extra images for each whale category (thereby resolving the issue of whale types having a representation of just a few images). 

Examples of synthetically generated data:

![GitHub Logo](https://github.com/njamalova/whale_tail_identifier/blob/develop/images/synth1.PNG)
![GitHub Logo](https://github.com/njamalova/whale_tail_identifier/blob/develop/images/synth2.PNG)
![GitHub Logo](https://github.com/njamalova/whale_tail_identifier/blob/develop/images/synth3.PNG)

### Image Preprocessing

To facilitate better prediction accuracy and capture all possible ways a whale's tail might appear on the image, the following transformations were applied - horizontal/vertical flips, image normalization, rotation, etc. 

### Dataset Split

Since the generated data was too large to handle, the training was performed on a random sample of data, representing 2,572 different whale categories, with an 80%-20% split into train (35K images) and validation sets (9K images).

### Training

A 9-layered convolutional neural network was used to train on the preprocessed data. After training for 20 epochs, the accuracy achieved on the validation set approximated 63%, which is a lot higher than the "naive" training on the original data without synthetic images and with the "new_whale" category present (accuracy of appx. 40%). 

The curve below represents an almost exponential increase in model accuracy. 

![GitHub Logo](https://github.com/njamalova/whale_tail_identifier/blob/develop/images/acc.PNG)

## Conclusion & Further Improvements

Based on the analysis above, it can be concluded that generating synthetic images helps to increase the prediction accuracy. Moreover, training for more epochs and on a larger subset of data can potentially generate even better results and help encompass all whale species. 

Regarding the "new_whale" category, it seems like either more data is needed to generate meaningful clusters or a separate and more robust image similarity algorithm within this category. 


## References

1. https://www.kaggle.com/c/humpback-whale-identification
2. https://photutils.readthedocs.io/en/stable/background.html
3. https://www.kaggle.com/akhileshdkapse/foreground-extraction-opencv
4. https://github.com/akTwelve/tutorials/blob/master/image_composition/BasicImageComposition.ipynb






