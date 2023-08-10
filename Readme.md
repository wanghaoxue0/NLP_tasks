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

Some Possible Problems and my answers:

1.	Why don’t we use European PMC API to fetch the full text?
European PMC API is the first choice as it is part of the EBI infrastructure. We will use European PMC API as long as it satisfies our requirement no matter it is in terms of quality and amount. However, when fetching the full context for 1073 biomodels literature, we got 31 full text from Europe PMC and 280 full text from PMC. For the consideration of model training, we use the PMC API to fetch more full text.

2.	Why don’t always use Biomodels manual annotation as the ground-true labels?
After careful investigating, we found that some of the manual annotations are not selected from the original literature. In addition, for some general models, they use the same annotations for that field. One prominent example is the ones for Covid-19, almost all the models use the same annotation, which is provided by the Biomodels group as the criterion. As they are not from the literature, our automatic annotation can not provide a good match.

3.	Why don’t we use BioBert for automatic annotation?
BioBert is a popular choice to train large language model, but it is in old version and don’t focus on entities extraction. On the contrary, we find SciSpacy is more helpful in our circumstances. Firstly, BioBert is based on tensorflow==1.15 and python==3.7, which has many system problems when applied in MacBook M2 chip or EBI cluster. We manage to solve the system problems with the help of IT group, but it seems the performance is not that good. We expect it to produce better match when it takes a long time to get the result. Secondly, BioBert doesn’t focus on automatic annotation and thus it doesn’t outperform existing biomedical text processing packages. As BioBert is not maintained frequently, we choose the SciSpacy instead. spaCy is a popular natural language processing (NLP) library in Python that offers efficient and accurate text processing capabilities. scispaCy, on the other hand, is an extension of spaCy specifically tailored for scientific and biomedical text. Thirdly, we need some light models to install on our infrastructure, which means the training and annotation process need to be finished in a timely manner. In this consideration, SciSpacy performs well and it only costs 3 hours to finish the annotation task for our 1079 literature.

4.	How to improve the performance of the coverage?
As the test label is not well-established yet, we are stilling finding way to evaluate the performance of our automatic annotation. One notable problem is that some words have been incorporated in the labels without appearing in the literature like (Homo sapiens, Ordinary differential equation model). As far as I know, we can design a new criterion to determine the quality of selection, which can be more into biological understanding of the annotation. In addition, we have created two filters in the pipline, one is for the words extracted, the other is for oncology. How to design the filter words or oncology can be a potential way to improve the selection. 

Besides, I notice SciSpacy has its pre-train biobert models to be downloaded as well, which will be more useful in our context. But we need to download spacy.load("en_core_sci_scibert")  and train/predict with large clusters, which will be practical from my experiences.


5.	Will the project be orchestrated into a publication or a report?
The pipline we have developed is aiming for the BioModels annotation, thus it has a great match of the design of the BioModels API. If we want to explore the usage of this pipline in other platform, we can rewrite the code to accommodate the need. For the publication consideration, as there is no much theory innovation, the author prefers to publish as a report or guidance to do the automatic annotation. 

6.	What are problems so far on Maaly’s biobert based model?
I have been working on Maaly’s biobert models in the first few works. Generally speaking, the pipline is clear but there are some problems with application to Biomodels. Here are some problems I found so far:
•	The pretrained models mentioned in the text can not be downloaded from EBI website due to the large size. Running the .sh file will return an error(can’t not find the zip files). One solution is to contact the authors and they will send you by Google drive or email.
•	Another prominent problem is the maintainence of BioBerts, which has not been done in two years. The python and TensorFlow version is so old, which can only be solved by installing virtual environments(still not working for MacBook M2 chips). The GPU clusters in EBI is the with the latest 11.8, however TensorFlow ==1.15 designed for 10.5.
•	The 16 categories are too limited when we want to have more annotations beyond the 16 models. After using the pretrained model, we hope to find more terms that have been appeared in the texts.
•	The expected json file can’t be generated in the output. One possible explanation is that the usage of EBI search API, which needs to be updated with the latest version. I use the same strategy to generate the oncology using  EBI search API and it works in the end.
•	In the end, I hope there is a solution to train the model and produce the result without going through the 16 categories procedure, which is unnecessary for some researches.
•	Overall, I appreciate the works and look forwards to further updates.
![image](https://github.com/wanghaoxue0/NLP_tasks/assets/55145514/bcc41833-405b-4dec-be14-40c42d65ec41)
