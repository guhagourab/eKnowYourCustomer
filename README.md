# eKnowYourCustomer
Electronic Know Your Customer is a process of verifying the identity of a customer using electronic means. Computer vision can be used to verify types of documents, such as national ID cards. 

AI-OCR is a tool created using Deep Learning and Computer Vision. This tool is useful in the process of Document Verification & KYC for Financial Institutions.


First of all, let’s begin with — What is KYC?
Know Your Customer” or KYC is an important term used by businesses and refers to the process of verification of the identity of the customers and clients either before or during the start of doing business with them. Banks, digital payment companies, or any kind of financial institutions are now required by the RBI norms to have their customers KYC process completed before allowing them complete access to all services.
KYC Process requires document verification and validation by an executive from the company’s side but during this COVID time it has become a difficult task as people are not willing to step off their houses, so here comes the need for E-KYC backed by AI & Computer Vision.

To make the process more efficient and robust, the power of Artificial Intelligence can be leveraged and I will walk through this technique in detail in this article.

How can AI help in KYC?
The AI-OCR tool automatically captures, extracts, and creates an editable and searchable copy of the customer data for an efficient KYC completion.
AI-OCR tool is very apt for the Document Verification process as it performs operation on extracted text using OCR and performs verification and Validation check.


In this tutorial, I will take the example Aadhaar card for document verification as Aadhaar is accepted everywhere in the country.

Unique Features of Aadhaar card -:
Every document has some unique features which make it different from others
Aadhaar has these unique features -:
1. Emblem
2. GOVERNMENT OF INDIA [GOI] Symbol
3. QR Code
4. Design of Aadhaar
So, it’s now our choice on which features we want to train our Machine Learning model. I chose Emblem, GOI Symbol, Aadhaar Card, as earlier versions of Aadhaar had a barcode instead of a QR code, so I decided to omit it out because I wanted my model to be robust and efficient at the same time.

These features make the Aadhaar card distinguishable from other documents and help in validating whether the submitted document is an Aadhaar or not.


METHODOLOGY
STEP 1: Pre-Processing of Input Images
To verify a document first it needs to be processed as it may contain noise and that can affect the validation process, so in this case image will be processed first and noise will be eliminated using Gaussian Noise Filter or any other suitable noise removal filter can be used, it all depends on the quality of input data.
Now when noise is removed from the image, the Region of interest (in this case Aadhaar card) will be extracted out and this can be performed using the Canny Edge detection algorithm. We extract the region of interest to remove any irrelevant data present in the input image.

STEP 2: Extracting Region of Interest
After extracting the Region of Interest, feature extraction, and object recognition technology will be applied, and features specific to the Aadhaar card will be detected and marked.
For this, we need to train our custom Object detection model and I have chosen TensorFlow Object Detection API as I like the model zoo of TensorFlow Object Detection :-D

STEP 3: Training a Object Detection Model
Here I am listing steps briefly for how to train a custom object detection
model -:
Step I: Collect a comprehensive dataset concerning the Aadhaar card, make sure that there are enough images for each Aadhaar card in the dataset. While creating a dataset one should always take care of Quality as

QUALITY > QUANTITY.

Step II: After creating a dataset, we need to label it, so for this purpose, I used labellerr as it is an amazing tool and fastens up the whole process of data labeling. We labeled the images in three different categories namely “emblem”, “goisymbol” and “aadhaarcard”.

Step III: Now, all prerequisites are ready we will train our model, this step requires a proper environment setup and a lot of computational power also it is the most time-consuming step in the whole process. We will use the TensorFlow Object Detection model for core processing and configurations of “Faster R-CNN ResNet101” this model requires a bit more computational power as compared to others but the accuracy of this model compensates for the extra computational power. We divide the data-set into two parts one for training the model and the other for evaluating the model on a test data-set. This step took almost 34 hours of processing time and we will keep the loss while training the model of order 10^ (–2) so that model can predict features of the Aadhaar card more accurately as lower the loss more accurate model is trained. While processing we should keep an eye on the model training parameters and statistics at the tensor board and monitor the loss in the training graph closely. This graph tells the loss rate of the model while training and this is a service provided by TensorFlow to visualize statistics for an accurate and robust model. After this, we get our trained machine learning model in from of the frozen inference graph (.pb)which we will use to detect and recognize features on the input Aadhaar card image.


Live Statistics on Tensor board
Now, our machine has learned the features of the Aadhaar card and it is ready for evaluation so let’s go ahead and see how it performs :3

STEP 4: Verification of Document
The trained model will be used to verify whether the input document is a valid Aadhaar or not if it is, we will proceed to the next step, or else the document will be declared invalid and the process will end.

STEP 5: Extracting Data using OCR
After it is verified that the submitted document is an Aadhaar then information present on Aadhaar will be extracted by the means of Optical Character Recognition (OCR). This information will contain

Name — XXXX
DOB: XX-XX-XXXX
Gender — XXXX
Aadhaar Number — 0000 1111 2222

STEP 6: Validation of Data extracted using OCR
This information will be processed and validated by using an existing database. Name, DOB, Gender will be verified by comparing it with customer records available in the database with the concerned financial institutions.

STEP 7: Validation of Aadhaar number
Aadhaar Number will be verified using Verhoeff Algorithm and then cross-checking with the UIDAI database. If all these conditions are met then only the document will be verified as a Valid Aadhaar Card

Aadhaar Number fact — The actual UIDAI-Aadhaar number is 11 digits and not 12 digits. Well, do not be surprised. The first 11 digits of the 12-digit Aadhaar number displayed on your Aadhaar card are the actual UID Number and the 12th digit is the checksum associated with the Verhoeff Algorithm scheme.

Verhoeff Algorithm — The Verhoeff algorithm is a checksum formula for error detection developed by the Dutch mathematician Jacobus Verhoeff and was first published in 1969 (Source — Wikipedia)

Compilation of Scores obtained from all Verification Layers
There are three verification layers in the system, one more layer can be added if you have a legal contract with UIDAI. The three layers which are supported by this AI-OCR tool are -:
I. Document template check using Computer Vision
II. Cross verification of Name along with Date of Birth, Gender with the database of financial institution.
III. Validation of 12-digit Aadhaar number which is based on the Verhoeff Algorithm.

Here is the Process Flow of Complete Algorithm


Process flow Aadhaar AI-OCR & Computer Vision Tool
Now, our AI-OCR and Computer Vision tool is ready. Let’s see how it will perform in different scenarios or technical term its different test cases

TEST CASES
Scenario — I: User submits Pan card or any other document instead of Aadhaar.
In this scenario, if the user submits documents except for the Aadhaar card, it will be detected by the trained machine learning model and the system will notify the user to submit the valid image of the Aadhaar again. This scenario is possible when the user submits documents in hurry and mistakenly submits the wrong document.

Scenario — II: The user submits an image in the format of Aadhaar card but not an actual Aadhaar card.
In this scenario when someone tries to dupe the software by submitting an image in the template of Aadhaar card but not an actual Aadhaar card, this type of mischief is handled by software and labeled as an invalid Aadhaar card.

Scenario — III: User mistakenly submits the Aadhaar card of someone else instead of his/her own.
In this scenario document is a valid Aadhaar card but it doesn’t belong to the user. This is checked by the software based on the information extracted from the Aadhaar card. This scenario is possible in case of user mismatch or human error.

Scenario — IV: Aadhaar submitted by the user belongs to him but it is not genuine.
This scenario will arise when the Aadhaar number is not according to UIDAI’s Verhoeff algorithm and it is randomly created 12-digit number, as actual Aadhaar is of 11 digits and the last digit is a checksum to validate the Aadhaar number. This adds to the verification process.

Scenario — V: Aadhaar submitted by the user is genuine and it belongs to him.
This scenario is an acceptable scenario and all details submitted by the user are genuine including Aadhaar number and name.

All these scenarios are handled in the above-discussed methodology and process-flow if any of these situations arise then the system will reject the submitted document and mark it as Invalid.
Scenario V is the ideal scenario but our system should be robust enough to handle all kinds of intentional or unintentional errors.

SAMPLE OUTCOMES OF THE TOOL

Sample outcomes of Machine Learning model detecting Aadhaar card features
SAMPLE OUTCOME OF THE OCR TOOL


Left image shows feature detected by the tool and right image shows data extracted by OCR Tool.
All these input images were freely available on google and none of them have been leaked or taken from any classified database

You can see a short demo of this project here — https://youtu.be/tJs9aaSyYPo

If you are interested in the Validation and Information extraction from backside of Aadhaar card as well refer to

Aadhaar Card Verification and Information extraction from Front & Back Side using AI-OCR Tool.
Verification and Extraction of Information from Front & Back Side of Aadhaar Card using TensorFlow Object Detection and…
thavani-shiva3.medium.com

Final Outcome
This AI-OCR tool is useful for all the financial institutions as KYC has been mandated by the Reserve Bank of India (RBI) and especially in the post-COVID world where all efforts are being made to reduce Human to Human interaction, so this tool will resolve both the issues and help financial institutions to ease up the whole process efficiently.

Tools & Technologies Used
Python — Most suitable programming language for carrying out all AI tasks.
Google Cloud OCR — To extract the text from the Aadhaar card & validate it.
Tensorflow — To train our ML model on Aadhaar features.
Labellerr — To annotate the images for training a Model
OpenCV— To pre-process the images and make their format suitable to proceed onto training step.
Docker — To containerize the whole application and deploy it on cloud platforms.
