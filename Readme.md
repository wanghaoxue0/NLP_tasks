## automatic annotation
by Haoxue Wang(hw613@cam.ac.uk)
with collaboration with European Bioinformatics Institute.

By combining with the latest biomedical text processing techniques, I develop a pipline to do the automatic annotation for biomodels, from getting the PMC link to generate the annotation outputs. The automatic annotation framework is based on SpaCy models for biomedical text processing. The whole procedure can be divided into three parts: 
1. prepare the pmc link, download the full text in .txt if it is open source, otherwise obtain the abstract only
2. use our algorithms to do the automatic annotation with matching ontology, output saved as json file
3. transfer the output into readable .csv file, which can be compared with ground-true labels if available

The corresponding codes and annotation can be found in automatic_annotation.ipynb
<img width="400" alt="image" src="https://github.com/wanghaoxue0/NLP_tasks/assets/55145514/0c2463df-8258-48aa-b371-e41b43952fdd">

