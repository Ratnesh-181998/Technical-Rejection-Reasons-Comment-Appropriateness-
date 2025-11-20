Technical Rejection Reasons (Comment Appropriateness) 

Business Requirement:  

At GeM portal in bidding while rejecting sellers, buyers must select a drop- down list of reasons for 
rejecting sellers and relevant comments must be provided for each reason.  
Currently, some sellers are getting rejected by buyer without proper reasons & comments. In the 
present scenario, there’s no such module to find out the inappropriate remarks provided by buyer. GeM 
doesn't have a mechanism to validate the reasons specified by buyers and hence no way to check for 
unfair rejections. 
By this solution, will get the list of the inappropriate comments and GeM management will take required 
actions on these scenarios for proper bidding process. 

<img width="2151" height="1245" alt="image" src="https://github.com/user-attachments/assets/b9b2f2c2-9bbe-45a6-9c4a-0993417322e4" />


Proposed solution: 

Develop Technical Rejection module with AI/ML algorithms/ business rules. This module will showcase 
irrelevant comments that are provide by buyer in rejections. 
Note: This use case is not the part of the RFP and as suggested by CEO sir NEC developed this solution.

Solution Consumption: 

Following are the stakeholders and processes where this solution can have impact. 

• Buyers:  

The solution provides buyers with enhanced awareness through an automated comparison 
system that evaluates all offered products against a base product's technical specifications. 
While buyers retain the ability to select any product, the solution will present them with a clear 
comparison of each product's specification compliance. A "specification compliance score" will 
be generated for each product, which allows buyers to see how closely each offered product 
aligns with the base product's requirements. This transparency ensures that buyers are 
informed and can make more objective and data-driven decisions. 

• Sellers:  

Sellers will benefit from increased transparency in the evaluation process. Products that meet 
the required specifications will be clearly identified, reducing the likelihood of rejection due to 
misinterpretation or oversight by buyers. 

Consumption: 

This solution is consumed by two ways, i.e. on portal at bidding process and through BI dashboard. 
• Bidding process:  
When a rejection occurs, the buyer selects a reason from a drop-down list and adds comments. 
The GeM system should restrict inappropriate or useless comments during input to minimize 
irrelevant feedback from the start. This is achieved through API which processes comments and applies ML model to identify relevant / irrelevant comment. On basis of outcome below is alert 
provided to buyer on quoting irrelevant comment.

<img width="1271" height="717" alt="image" src="https://github.com/user-attachments/assets/b6fbb515-b59e-4310-bfbe-f35d96292231" />


 
• Dashboard: Dashboard gets updated monthly and comments with buyer and bid number details 
are showcased through dashboard. GeM validation team has the access to this dashboard.  


<img width="1296" height="810" alt="image" src="https://github.com/user-attachments/assets/296fcbcd-635e-4f37-a264-2fda61a258ec" />


Data: 
To build this solution required data consist of Bid number, buyer Id, Category Id, product Id, Status, 
Reasons, Comments, Category, bid starting date, bid ending date and seller Id. The required data is 
fetched from below tables. 
• Table: bdp_odb_bids 
• Table: bdp_odb_bid_participation 
• Table: bdp_odb_bid_participate_details 


Model Methodology: 

1. Data preprocessing: 
• It is a process of preparing the raw data and making it suitable for a machine learning model. It 
is the first and crucial step while creating a machine learning model.  
• When creating a machine learning project, it is not always a case that we come across the clean 
and formatted data. And while doing any operation with data, it is mandatory to clean it and put 
in a formatted way. So for this, data in json format converted to structured data. 
2. Topic modeling: 
• It is a machine learning technique that automatically analyzes text data. These are very useful 
for the purpose for organizing large blocks of textual data. In natural language processing, 
Latent Dirichlet Allocation (LDA) is a generative statistical model that explains a set of 
observations through unobserved groups. It is popular topic modeling technique used to classify 
text into a particular topic. 
• The topics produced by topic modeling techniques are clusters of similar words. A topic model 
captures this intuition in a mathematical framework, which allows examining and discovering, 
based on the statistics of the words. 
• The probabilities are generated based on this topic values for each comment in reasons wise. 
These probability values are calculated, the differentiation is done between the maximum 
probability and minimum probability values.  
3. Machine Learning algorithms 
• Many machine learning algorithms are capable of predicting a probability or scoring of class 
membership, and this must be interpreted before it can be mapped to a crisp class label.  
• This is achieved by using a threshold, where all values equal or greater than the threshold are 
mapped to one class and all other values are mapped to another class.  
4. Thresholds: 
• There the threshold limiting of probabilities takes place at which the inappropriate comments 
will be having very low differentiation value.  
• So, by this process we can find out the comments that are not remarked correctly as per the 
rejection in the bidding.


<img width="1324" height="777" alt="image" src="https://github.com/user-attachments/assets/fd7fac70-a7da-47ce-8337-638c93a72c6c" />


Steps In Model:  
1. Data Extraction: 
• Extract all necessary information from the database. Ensure the data contains comments in 
JSON format, as well as other relevant fields required for analysis. 
2. Data Cleaning (Pre-processing): 
• Convert JSON data (containing reasons and comments) into a structured format like a Python 
DataFrame. 
• Handle missing values, duplicates, and standardize the text data for further analysis. 
3. Importing Libraries and Initial Exploration: 
• Import required libraries (e.g., pandas, numpy, sklearn, spacy). 
• Load the dataset into a DataFrame and explore the different types of "reasons" present in the 
data. 
4. Reason-wise Data Selection and Word Vector Embeddings: 
• Segment the data based on "reason". 
• Generate word vector embeddings for comments. 
• Create a Latent Dirichlet Allocation (LDA) model to uncover the hidden topics within each set of 
comments. 
5. Saving Models: 
• Save the Count Vectorizer model and LDA model to disk using libraries pickle for later use.
6. Generating Probabilities for Comments: 
• For each comment, compute the topic distribution probabilities using the LDA model. This step 
helps in understanding the relevance of each comment to the identified topics. 
7. Differentiating Probabilities: 
• Calculate the difference between the maximum and minimum probability values for each 
comment. 
• Map these values back to the DataFrame for further analysis. 
8. Filtering with Thresholds: 
• Apply a threshold to filter out comments that have a low probability of belonging to any 
significant topic. These are considered irrelevant. 
9. Predicting Irrelevant Comments for New Data: 
• Load the saved models (from the last 3 months) to process new incoming data. 
• Use these models to predict irrelevant comments based on the patterns learned from previous 
months' data.

Business Value:

1. The solution aims to enhance the bidding process on the GeM portal by identifying and filtering out 
irrelevant or inappropriate comments provided as reasons for bid rejections. By focusing on 
providing relevant and constructive feedback, the solution will improve the seller's experience by 
ensuring that rejection reasons are meaningful and actionable. This not only helps sellers better 
understand the shortcomings of their bids but also fosters a more transparent and fair bidding 
environment. 
2. Additionally, the GeM management will benefit from this solution by being able to identify and 
address buyers who consistently provide inadequate or inappropriate feedback. By addressing these 
issues, GeM management can ensure that the bidding process remains rigorous and fair, ultimately 
leading to more accurate evaluations and increased product sales and business transactions. This 
refined approach to feedback and bidding will contribute to a more effective marketplace, 
benefiting both sellers and buyers alike.

<img width="2267" height="1226" alt="image" src="https://github.com/user-attachments/assets/56fbcaf0-4fc8-4030-8085-e73db8485b17" />


<img width="2221" height="1242" alt="image" src="https://github.com/user-attachments/assets/6eec0be8-5483-4afb-8609-45493f5289f9" />


Integration & API: Developed and integrated RESTful APIs using FlaskAPI.

Input - Solution requires bid id as input.

<img width="1339" height="358" alt="image" src="https://github.com/user-attachments/assets/b5d51573-b5fe-4999-b9f8-c2260e75a758" />

Output – Output showcases all irrelevant comments for respective bid.  

<img width="1287" height="441" alt="image" src="https://github.com/user-attachments/assets/ccc79bf6-2733-49cf-b30f-ba23e931f831" />

<img width="1421" height="421" alt="image" src="https://github.com/user-attachments/assets/28353507-a7e5-4a03-ab92-3ef655bd85c3" />
<img width="1428" height="273" alt="image" src="https://github.com/user-attachments/assets/591a068a-d888-4aeb-83b7-149a78e32ec0" />





