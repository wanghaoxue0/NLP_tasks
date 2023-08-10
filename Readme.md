By combining with the latest biomedical text processing techniques, I develop a pipline to do the automatic annotation for biomodels, from getting the PMC link to generate the annotation outputs. The automatic annotation framework is based on SpaCy models for biomedical text processing. The whole procedure can be divided into three parts: 
•	prepare the pmc link, download the full text in .txt if it is open source, otherwise obtain the abstract only
•	use our algorithms to do the automatic annotation with matching ontology, output saved as json file
•	transfer the output into readable .csv file, which can be compared with ground-true labels if available
The corresponding codes and annotation can be found in automatic_annotation.ipynb
![image](https://github.com/wanghaoxue0/NLP_tasks/assets/55145514/00a6b35b-af94-4886-a11d-e800ee993109)
