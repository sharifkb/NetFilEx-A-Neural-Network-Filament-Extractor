# NetFilEx-A-Neural-Network-Filament-Extractor

We present a neural network-based object extraction algorithm. NetFilEx breaks down FITS files into (128×128)pix sized tiles and searches each one for Non-Thermal Filaments. The algorithm outputs mosaics of the located NTFs, stored in an HDUList. The Convolutional Neural Network has been trained on the MeerKAT survey of the Galactic Centre and has achieved a validation accuracy of 99.61%. These mosaics can be used for subsequent analysis by packages such as FilFinder, as demonstrated in this paper. Extraction algorithms like these will become increasingly necessary with the next generation of radio telescopes. Tradition object classification methods require a significant degree of human interaction, which will not be feasible as the amount of data from radio surveys grow. The automation facilitated by Convolutional Neural Networks will shorten the pipeline between telescopes and researchers.


0 Creating cuts:

This script cuts the original science frame into 128 by 128 cuts to use in subsequent training. Before the image is cut, an upper and lower border is remocer to reduce the number or repetative data samples.

1 Sorting by eye

This scrips is used to plot all of the data samples after they have been sorted into each class. The first separation was done using filfinder. Then, as models were trained, these were used instead to catagorise the data points do to the higher accuracy.
411 data samples were FP,
49 were FN,
77 were removed for being poor in quality.

2 GAN

This script takes data samples from the NTF class only and builds synthetic images based on the learned distribution.

3 CNN-Model_builder

This script was written in google colab to make use of their GPU accelerator. This optimises a series of hyperparameters in order to find the model which produces the highest validation accuracy.

3 CNN

This script performs preprocessing and augmentation of all all the data samples and constructs training and validation datasets and stores them in npy arrays. These arrays were saved to be transferred to google colab. It was also used to train some of the earlier CNN models. Techniques used here involved successively increasing the number of convolutional blocks until there was no significant further increse in validation accuracy. The architecture was then tuned using keras. Giving a validation accuracy of 98.89% This was replaced by the model built in "3 CNN-Model_builder".


4 Classification

This script was built to create a npy array of class predictions arranged in a 2D array with positions corresponding to the position of the relevant data sample. This was used to inform the development of NetFilEx. It was also used to re-sort data samples using the pretrained models.

5 Stitching Fits Together

This script was used to experiment with methods of creating mosaics using the prediction array from the previous script.

Filfinder

Performs FilFinder Analysis

Netfilex

The final algorithm which condenses "0. Creating cuts", the preparation portion of "3 CNN", "4 Classification" and "5 Stitching Fits Together" into a seried of functions.

To run the Netfilex algorithm locally the "modelBlueBEAR_0.9961212277412415_41_2_20" also needs to be downloaded.
