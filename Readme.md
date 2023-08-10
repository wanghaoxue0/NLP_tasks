## automatic annotation
by Haoxue Wang(hw613@cam.ac.uk)
with collaboration with European Bioinformatics Institute.

By combining with the latest biomedical text processing techniques, I develop a pipline to do the automatic annotation for biomodels, from getting the PMC link to generate the annotation outputs. The automatic annotation framework is based on SpaCy models for biomedical text processing. The whole procedure can be divided into three parts: 
1. prepare the pmc link, download the full text in .txt if it is open source, otherwise obtain the abstract only
2. use our algorithms to do the automatic annotation with matching ontology, output saved as json file
3. transfer the output into readable .csv file, which can be compared with ground-true labels if available

The corresponding codes and annotation can be found in automatic_annotation.ipynb
<img width="400" alt="image" src="https://github.com/wanghaoxue0/NLP_tasks/assets/55145514/0c2463df-8258-48aa-b371-e41b43952fdd">

The advantages of our pipline can be summarised in three aspects:
1.	Our pipline is based on the frequently maintained algorithms, which means there will be less system errors than other models.
2.	Our algorithms are developed to incorporate the need for the EBI’s Biomodels
3.	Our algorithms enjoy the flexibility by adding two manual filters, whose values can be set up by biological experts. Thus the algorithms can outperform other black-box large models.

## Some Possible Problems and my answers:

### 1.	Why don’t we use European PMC API to fetch the full text?
European PMC API is the first choice as it is part of the EBI infrastructure. We will use European PMC API as long as it satisfies our requirement no matter it is in terms of quality and amount. However, when fetching the full context for 1073 biomodels literature, we got 31 full text from Europe PMC and 280 full text from PMC. For the consideration of model training, we use the PMC API to fetch more full text.

### 2.	Why don’t always use Biomodels manual annotation as the ground-true labels?
After careful investigating, we found that some of the manual annotations are not selected from the original literature. In addition, for some general models, they use the same annotations for that field. One prominent example is the ones for Covid-19, almost all the models use the same annotation, which is provided by the Biomodels group as the criterion. As they are not from the literature, our automatic annotation can not provide a good match.

### 3.	Why don’t we use BioBert for automatic annotation?
BioBert is a popular choice to train large language model, but it is in old version and don’t focus on entities extraction. On the contrary, we find SciSpacy is more helpful in our circumstances. Firstly, BioBert is based on tensorflow==1.15 and python==3.7, which has many system problems when applied in MacBook M2 chip or EBI cluster. We manage to solve the system problems with the help of IT group, but it seems the performance is not that good. We expect it to produce better match when it takes a long time to get the result. Secondly, BioBert doesn’t focus on automatic annotation and thus it doesn’t outperform existing biomedical text processing packages. As BioBert is not maintained frequently, we choose the SciSpacy instead. spaCy is a popular natural language processing (NLP) library in Python that offers efficient and accurate text processing capabilities. scispaCy, on the other hand, is an extension of spaCy specifically tailored for scientific and biomedical text. Thirdly, we need some light models to install on our infrastructure, which means the training and annotation process need to be finished in a timely manner. In this consideration, SciSpacy performs well and it only costs 3 hours to finish the annotation task for our 1079 literature.

### 4.	How to improve the performance of the coverage?
As the test label is not well-established yet, we are stilling finding way to evaluate the performance of our automatic annotation. One notable problem is that some words have been incorporated in the labels without appearing in the literature like (Homo sapiens, Ordinary differential equation model). As far as I know, we can design a new criterion to determine the quality of selection, which can be more into biological understanding of the annotation. In addition, we have created two filters in the pipline, one is for the words extracted, the other is for oncology. How to design the filter words or oncology can be a potential way to improve the selection. 

Besides, I notice SciSpacy has its pre-train biobert models to be downloaded as well, which will be more useful in our context. But we need to download spacy.load("en_core_sci_scibert")  and train/predict with large clusters, which will be practical from my experiences.

