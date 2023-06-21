# README

**Research Problem:**

The contemporary digital age has been characterized by the pervasive impact of electronic products on both individual lives and business operations, engendering a revolution in communication, leisure, and productivity. With the ascendance of e-commerce platforms like Amazon, electronic products have become accessible to a global market, leading to a surge in product reviews that offer potential buyers insights into product functionality, quality, and user satisfaction (Luca, 2016). However, this beneficial dynamic is complicated by the increasing prevalence of fake reviews, undermining trust in e-commerce platforms and inhibiting informed purchasing decisions (Mayzlin, Dover, & Chevalier, 2014). Therefore, the investigation into the authenticity of reviews, particularly for electronic products, has garnered significant importance.

***The Research Question***
The primary research question driving this paper revolves around review authenticity: Can we develop a machine-learning model that predicts whether a review is genuine based on its content, rating, and timing? Identifying fake reviews on e-commerce platforms, specifically within the electronics sector, poses a significant challenge due to the volume of data and the evolving sophistication of deceptive tactics. Nevertheless, as purchasing electronic products often involves considerable financial commitments and hinges on reliable reviews, ensuring the authenticity of these reviews is critical (Li, Wu, & Mai, 2019). 

***Contributions and Approach***
This paper addresses the problem by leveraging advanced machine-learning techniques and large-scale review data to predict authenticity of reviews. Our approach integrates several critical factors contributing to review credibility, including review content, sentiment score, product rating, spelling errors, and review timing. Our results significantly enhance our understanding of patterns and indicators in genuine and fake reviews and contribute to developing robust and practical models for detecting fake reviews. Furthermore, our study generates practical and theoretical implications by revealing the nuanced relationship between review characteristics and their authenticity.

***Differences and Novelty***
Compared to previous work in the area, our research offers a unique perspective by focusing on electronic product reviews and leveraging a comprehensive set of features within a machine-learning framework. Our approach is differentiated by amalgamating various indicators and using multiple machine-learning models to enhance prediction accuracy, such as (补充下具体model). Also, applying these models to a large-scale real-world dataset from Amazon provides empirical validation of the proposed methods, setting our study apart (Li et al., 2019; Luca, 2016).

***Roadmap***
The remainder of this paper is structured as follows: we delve into the details of the dataset and the feature engineering processes we employ, followed by an explanation of the machine learning pipeline utilized in our study. Subsequent sections offer an in-depth analysis of the results obtained from the various models, discussing their implications for the problem at hand. Finally, we conclude by reflecting on our findings, their practical implications, and potential avenues for future research.


**Dataset and Feature Engineering:**

We used the Amazon Customer Reviews Dataset for our study. The data is available in TSV files in the amazon-reviews-pds S3 bucket in AWS US East Region, covering over 130+ million customer reviews. For this study, we focused on the “Electronics” reviews dataset to conduct our analysis. The dataset consists of the following variables:  marketplace, customer_id , review_id, product_id, product_parent, product_title, star_rating, helpful_votes,  total_votes, vine, verified_purchase, review_headline, review_body, review_date,  year. 

We firstly removed reviews that have null values. Next, we made the ‘verified_purchase’ (our response variable) into dummies and check if two classes are balanced or not. Because there are 2522841 observations in one class and 498007 observations in another class (more verified purchases than non-verified purchases), we need to balance the dataset by downsampling the ‘verified purchases’ class. 
Next, we did some additional feature engineering and fit them into our machine learning models.  

1.Spelling Errors: Because fake review might contain more spelling mistakes, we used the PySpark built in library ‘SpellChecker’ to count the spelling errors in each review. 

2.Sentiment Score: Some fake reviews might have extreme sentiments (overly positive or negative). Therefore, sentiment score could also become an interesting covariate.

3.Helpful votes percentage: we integrated two variables  “helpful_votes” and “total_votes” into one – helpful votes percentage. 
Review_length: fake reviews and genuine reviews might also vary by length, so we calculate review length for each review. 

4.Vine_purchase: we made the variable ‘vine’ a dummy variable indicating whether a review is written by a vine reviewer or not. Because a vine reviewer is less likely to write a fake review. 

5.MarketplaceVec: Because the variable ‘marketplace’ has five different categories. we conducted one-hot-encoding to make them into vectors. 
Star_rating: The rating of a product is also an important indicator for review quality. So we included it in the model. 


**Pipeline:**

We utilize PySpark to execute our machine learning pipelines due to the exceptional scalability provided by Spark. Its ability to efficiently distribute and parallelize computations makes it well-suited for handling large-scale machine learning tasks. Moreover, Spark offers a unified platform for diverse data processing operations, enabling seamless end-to-end data processing and machine learning workflows.

Once we have engineered all the required features, we employ the VectorAssembler in PySpark as a feature transformer. The VectorAssembler allows us to consolidate the targeted predictors, namely 'marketplaceVec', 'review_length', 'v_dummy', 'star_rating', 'sentiment_score', 'spelling_errors', and 'helpful_pct', into a single vector column. This process is crucial for assembling the necessary features to train our machine learning models.

Our project encompasses the training of five models: Logistic Regression, Random Forest, Support Vector Classifier, Decision Tree Classifier, and Gradient-Boosted Tree Classifier. For each model, we construct a machine learning pipeline. The pipeline facilitates the sequential execution of multiple stages. The initial stage, named "vector_assembler," consolidates the selected features from the dataset into a vector representation. The subsequent stage represents the model training phase using a specific machine learning algorithm. The model is trained using the feature vectors assembled in the "vector_assembler" stage.

By combining these stages within a pipeline, we ensure the consistent and reproducible execution of feature engineering and model training steps. The pipeline's advantages include the ability to apply the same preprocessing and training processes to different datasets without code duplication. Furthermore, it simplifies the deployment of the entire pipeline in production environments, streamlining the workflow from development to deployment.

**Models:**

Results comparison 


### Model Evaluation

Model | Logistic Regression | Random Forest | Decision Tree Classifier | Gradient-Boosted Tree Classifier  | Support Vector Classifier 
--- | --- | --- | --- |--- |---
Training AUC | 0.7059 | 0.7214 | 0.7176 |  | 0.6662 
Test AUC | 0.7191 | 0.6419 | 0.7348 |  | 0.6982 
Training Accuracy | 0.6577 | 0.6446 | 0.6776 |  | 0.6662 
Test Accuracy | 0.6752 | 0.6655 | 0.6465 |  | 0.6460 

## Logistic Regression
![Logistic Regression](https://github.com/macs30123-s23/final-project-final_proj/blob/main/figures/LR.png)
## Random Forest
![Random Forest](https://github.com/macs30123-s23/final-project-final_proj/blob/main/figures/RF.png)
## Decision Tree Classifier
![Decision Tree Classifier](https://github.com/macs30123-s23/final-project-final_proj/blob/main/figures/DT.png)
## Gradient-Boosted Tree Classifier

## Support Vector Classifier
![Support Vector Classifier](https://github.com/macs30123-s23/final-project-final_proj/blob/main/figures/SVC.png)

