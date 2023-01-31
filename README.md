# Convolutional-Neural-Network-for-Concrete-Crack-Segmentation
A predictive model that could process images of concrete surface to label the crack

## Introduction
U-Net is built upon the so-called 'Fully Convolutional Network', it was first introduced in 2015 and won the ISBI challenge for segmentation of neuronal structures in electron microscopic stacks. For more details, please see the original article [1].

In 2019, Zhengqing Liu etc. had applied U-Net for concrete crack detection [2], they use a new convolutional block built from convolutional layer, batch normalization layer and relu activation layer, to replace the original convolutional layers in U-Net architecture. It works well even with a relatively small training dataset (with 84 images). However, the prediction result is highly sensitive to the quality of concrete surface and the width of cracks.

Here we design a new network by adding a preprocessor inspired by 'Inceptoin Network' to address this overfitting issue.

## Network Design
U-Net architecture has unique ability with high learning speed from training set, and high accuracy when prediction. Different from FCN, it supplements a usual contracting network with successive layers, where pooling oprators are replaced by upsampling operators. It would keep the output from each convolutional layer by either concatenating it with the upsampled results or simply adding them together. This modification can take advantage of these procedures to overcome the trade-off between localization accuracy and the use of context (FCN requires more max-pooling layers which reduce the localization accuracy, while small patches allow the network to see only little context). 

Thus, to minimize overfitting with U-Net, we need to preprocess our input images. An idea from 'Inception Network' is to add multiple processors that could capture different features from original images, and store them (either add or concatenate) as the input of next layer. By analysing testing images and their result, we have extracted 4 main characteristics that could lead an overfitting. The non-trainable pooling layers could be an excellent choice to standardize our input. As AveragePooling could flatten background noise and MaxPooling to highlight the potential cracks.

Figure 1. Characteristics of images
![Charastristic](https://user-images.githubusercontent.com/112973740/212207182-eeed65cb-a6e9-4103-a1b2-ece6b1353552.png)

Figure 2. Network architecture
![Convolutional Network](https://user-images.githubusercontent.com/112973740/212524893-25d07043-af57-4c5e-b04c-82414b115137.png)

## Training and Testing
To illustrate the improvement, we first train our model on a selected training set with limited characteristics (rough surface), then test it with all the characteristics. The training set is the subset 'CRACK500' from Concrete Crack Conglomerate Dataset [3]. For hyperparameter selection, please see the attached code 'Inception UNet.ipynb'. The result is shown in figure 3.

Figure 3. Testing result
![image](https://user-images.githubusercontent.com/112973740/212215508-92fbdcb4-5148-4470-954a-ca732ce29187.png)

## Conclusion
Convolutional Neural Network would generally need large training set to optimize all the parameters (7,783,545 trainable in our case). However, this network shows great potential as it trains fast, and predict precisely even with limited features in the training set.
## Reference

[1] Olaf Ronneberger, Philipp Fischer, Thomas Brox (2015) U-Net: Convolutional Networks for Biomedical Image Segmentation https://doi.org/10.48550/arXiv.1505.04597

[2] Zhenqing Liu, Yiwen Cao, Yize Wang, Wei Wang (2019) Computer vision-based concrete crack detection using U-net fully convolutional networks https://doi.org/10.1016/j.autcon.2019.04.005

[3] Eric Bianchi, Matthew Hebdon (2021) Concrete Crack Conglomerate Dataset https://doi.org/10.7294/16628596
