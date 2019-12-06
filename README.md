# Histopathologic Cancer Detection
### Identify metastatic tissue in histopathologic scans of lymph node sections

![alt text](https://miro.medium.com/max/2111/1*gNcFEL1cpGpDC4vo1zUAWA.png)

***Breast cancer ranks second in mortality among women after lung cancer***. According to clinical statistics, 1 out of every 8 women is diagnosed with breast cancer during their lifetime. Nevertheless, periodic clinical examinations and self-tests help in its early detection, and thus significantly increase the chances of survival. [Invasive](https://ru.wikipedia.org/wiki/%D0%98%D0%BD%D0%B2%D0%B0%D0%B7%D0%B8%D0%B2%D0%BD%D0%B0%D1%8F_%D0%BF%D1%80%D0%BE%D1%86%D0%B5%D0%B4%D1%83%D1%80%D0%B0) detection [methods](https://ru.wikipedia.org/wiki/%D0%98%D0%BD%D0%B2%D0%B0%D0%B7%D0%B8%D0%B2%D0%BD%D0%B0%D1%8F_%D0%BF%D1%80%D0%BE%D1%86%D0%B5%D0%B4%D1%83%D1%80%D0%B0) cause tumor rupture, accelerating the spread of cancer to surrounding areas. Consequently, there is a need for a more reliable, fast, accurate and effective non-invasive cancer detection system (Selvathi, D & Aarthy Poornila, A. (2018). Deep learning methods for detecting breast cancer using medical image analysis).
Early detection may give patients more treatment options. To detect signs of cancer, breast tissue from a biopsy is stained to improve nuclei and cytoplasm for microscopic examination. Pathologists then evaluate the extent of any abnormal structural change to determine if there are tumors.
Architectural distortion is a very thin seal of breast tissue and may represent the earliest sign of cancer. Since radiologists are unlikely to notice this, over the years several approaches for early detection have been proposed, but none of them use deep learning methods.
In its Renaissance Age, Deep Learning became the leading branch of machine learning that has been used for more than 10 years in many sectors of human life, including healthcare, the key requirement of deep learning methods is the data that the Kaggle platform provides in this [competition](https://www.kaggle.com/c/histopathologic-cancer-detection) for solving histopathological diagnosis of cancer.


### Task

## What is the challenge?

***The task of classifying two-dimensional images.*** It is necessary to determine the presence of metastases from digital histopathological images of 96x96 pixels in size. One of the key problems is that metastases can be as small as single cells in a large area of tissue.

## What is known about the data?

***Histopathological images are microscopic images of vitreous slides of lymph nodes stained with hematoxylin and eosin (H&E).*** This staining method is one of the most widely used in medical diagnostics, it stains blue, violet and red. Dark blue - hematoxylin binds to negatively charged substances, such as nucleic acids, Pink - eosin, to positively charged substances, such as amino acid side chains (most proteins). Usually the nuclei are stained blue, while the cytoplasm and extracellular parts have different shades of pink.

**Low resolution**             | **Medium resolution**            | **A high resolution** 
:-------------------------:|:-------------------------:|:-------------------------:
![](https://camelyon17.grand-challenge.org/site/CAMELYON17/serve/public_html/example_low_resolution.png) | ![Example of a metastatic region](https://camelyon17.grand-challenge.org/site/CAMELYON17/serve/public_html/example_mid_resolution.png) | ![Example of a metastatic region](https://camelyon17.grand-challenge.org/site/CAMELYON17/serve/public_html/example_high_resolution.png)


[An example of a metastatic region in the lymph nodes, CHAMELYON17](https://camelyon17.grand-challenge.org/Background/)

Lymph nodes are small glands that filter fluid in the lymphatic system, and they are the first place breast cancer spreads. The histological evaluation of lymph node metastases is part of the definition of the stage of breast cancer in the TNM classification , which is a recognized standard for classifying the extent of cancer spread. The diagnostic procedure for pathologists is tedious and time-consuming, since it is necessary to examine a large area of tissue to easily miss small metastases.

***Useful links for basic knowledge:***

+	[Camelyon Dataset (PCam)](https://github.com/basveeling/pcam)
+	[Hematoxylin and eosin staining of tissue and cell sections](https://www.ncbi.nlm.nih.gov/pubmed/21356829)
+	[H & E-stained patches of sentinel lymph nodes in patients with breast cancer: CAMELYON dataset](https://academic.oup.com/gigascience/article/7/6/giy065/5026175)
+	[CAMELYON16](https://camelyon16.grand-challenge.org/Background/)
+	[CAMELYON17](https://camelyon17.grand-challenge.org/Background/)
+	[TNM Classification](https://ru.wikipedia.org/wiki/TNM)
________________________________________
### Data parsing

***What data is provided to solve the problem?***

***220 thousand Training and 57tys. Evaluation images.*** The dataset is a subset of the [PCam dataset](https://github.com/basveeling/pcam), and the only difference between them is that in the data provided to us, all duplicate images have been deleted. The PCam dataset is obtained from the [Camelyon16 Challenge dataset](https://camelyon16.grand-challenge.org/Data/), which contains 400 H & E-stained complete images of lymph node sentinel slides, which were obtained and digitized at 2 different centers using a 40x lens. The PCam dataset, including this one, uses 10x downsampling to increase the field of view, resulting in a resulting pixel resolution of 2.43 microns.

A sentinel lymph node is a lymph node into which lymph outflow from the tumor is the first to occur, therefore, it is primarily affected. In the literature, there are two features that characterize SLU. The first is the lymph node closest to the tumor, and the second - this node is affected by metastases in the first place.

According to the [data description](https://www.kaggle.com/c/histopathologic-cancer-detection), ***there is a 50/50 balance*** between positive and negative examples ***in the training and test subsets***. However, the distribution of training is 60/40 (negatives / positives). A positive mark means that there is at least one pixel of tumor tissue in the central area (32 x 32 pixels) of the image. ***Tumor tissue in the region far from the center of the slide does not affect the label***. This means that a negatively labeled image may contain metastases in the central region. Thus, cropping images in the central region will not lose information.

***Description of the provided images Format:*** TIF Resolution: 96x96 Channels: 3 Color Depth: 8 Data Type: Unsigned char Compression Type: Jpeg
________________________________________
### How much quality data?
This dataset is a combination of two independent datasets collected at Radboud University Medical Center (Nijmegen, the Netherlands) and Utrecht University Medical Center (Utrecht, the Netherlands). Slides are made in accordance with normal clinical practice and should be studied by a trained pathologist. images to detect metastases. However, with these small image samples, some important information could be lost.

According to the data description, the data set is free of duplicates. However, this has not been confirmed by testing.
For the entire data set, when the slide level label was unclear during the verification of stained H&E stained glass, an additional layer of WSI film with sequential tissue section immunohistochemically stained for cytokeratin was used to confirm the classification.

+	https://academic.oup.com/gigascience/article/7/6/giy065/5026175

All vitreous slides included in the CAMELYON dataset were part of a routine clinical diagnosis, which means that the data is of diagnostic quality. However, during the scanning process, scanning may fail or result in out of focus images. As a quality control measure, all slides were checked manually after scanning. The test was carried out by an experienced technician (QM and NS for UMCU, MH or R.vd.L. for other centers) to assess the quality of the scan.
________________________________________
### Useful links for advanced knowledge:

+ [The library is designed for convenient image processing in Materials Science (materials science)](https://ttk.gricad-pages.univ-grenoble-alpes.fr/spam/intro.html#welcome-to-spam)
+ [About detection technology using wsi film](https://ilt.kharkov.ua/bvi/structure/d16/ru/nonequil_eff.html)
+ [Immunohistochemical markers in the diagnosis, staging and prognosis of various forms of urothelial carcinoma](https://www.science-education.ru/ru/article/view?id=4962)
+ [Side chains](http://chem21.info/info/1304270/)
+ [Biopsy](https://megabook.ru/article/%D0%91%D0%B8%D0%BE%D0%BF%D1%81%D0%B8%D1%8F%20(%D0%BC%D0%B5%D0%B4%D0%B8%D1%86%D0%B8%D0%BD%D0%B0))
+ [Video with comments of a histological section of a kidney in a light microscope](https://www.youtube.com/watch?v=Po5J67mW1JM)
+ https://ttk.gricad-pages.univ-grenoble-alpes.fr/spam/spam_examples/index.html



