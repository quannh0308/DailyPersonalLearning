# QnABot on AWS

## Overview

QnABot on AWS is a multi-channel, multi-language conversational interface (chatbot) that responds to your customer’s questions, answers, and feedback. It allows you to deploy a fully functional chatbot across multiple channels including chat, voice, SMS, and Amazon Alexa.

##  Benefits

### Enhance your customer’s experience 
Provide personalized tutorials and question and answer support with intelligent multi-part interaction. Use the Command Line Interface (CLI) to import and export questions from your QnABot setup. Use Amazon Kendra natural language processing (NLP) capabilities to better understand human questions. Import questions and answers and session attributes from an Excel file.

### Reduce call center wait times 
Automate customer support workflows.

###  Implement the latest machine learning technology 
Create engaging, human-like interactions for chatbots. Use intent and slot matching to implement different types of question-and-answer workflows.

## Technical details
 ![QnABot on AWS](/asset/image/technology/aws/solution/QnABot/qnabot-arch-diagram.png?raw=true "QnABot on AWS")

**Step 1**

The admin deploys the guidance into their AWS account, opens the Content Designer UI, and uses [Amazon Cognito](https://aws.amazon.com/cognito/) to authenticate.

**Step 2**

After authentication,  [Amazon CloudFront](https://aws.amazon.com/cloudfront/) and [Amazon Simple Storage Service](https://aws.amazon.com/s3/) (Amazon S3) deliver the contents of the Content Designer UI.

**Step 3**

The admin configures questions and answers in the Content Designer and the UI sends requests to [Amazon API Gateway](https://aws.amazon.com/api-gateway/) to save the questions and answers.

**Step 4**

The Content Designer [AWS Lambda](https://aws.amazon.com/lambda/) function saves the input in [Amazon OpenSearch Service](https://aws.amazon.com/opensearch-service/) in a questions bank index.

**Step 5**

Users of the chatbot interact with [Amazon Lex](https://aws.amazon.com/lex/) via the Web Client UI or [Amazon Connect](https://aws.amazon.com/connect/).

**Step 6**

Amazon Lex forwards requests to the AWS Lambda (Bot Fulfillment) function. Users can also send requests to this Lambda function via Amazon Alexa devices.

**Step 7**

The Bot Fulfillment function takes the users input and uses [Amazon Comprehend](https://aws.amazon.com/comprehend/) and [Amazon Translate](https://aws.amazon.com/translate/) (if necessary) to translate non-English requests to English and then looks up the answer in Amazon OpenSearch Service. 

If [Amazon Kendra](https://aws.amazon.com/kendra/) and [Amazon SageMaker](https://aws.amazon.com/sagemaker/) are configured at the time of deployment, the Bot Fulfillment function also sends a request to those indexes.

**Step 8**

User interactions with Bot Fulfillment functions generate logs and metrics data, which is sent to [Amazon Kinesis Data Firehose](https://aws.amazon.com/kinesis/data-firehose/) then to Amazon S3 for later data analysis.

 
## AWS services used
**[Amazon Cognito](https://aws.amazon.com/cognito/)** - Implement secure, frictionless customer identity and access management that scales

**[Amazon CloudFront](https://aws.amazon.com/cloudfront/)** - is a content delivery network (CDN) service built for high performance, security, and developer convenience. Securely deliver content with low latency and high transfer speeds.

**[Amazon Simple Storage Service](https://aws.amazon.com/s3/)** -  Object storage built to retrieve any amount of data from anywhere

**[Amazon API Gateway](https://aws.amazon.com/api-gateway/)** - 
Create, maintain, and secure APIs at any scale

**[AWS Lambda](https://aws.amazon.com/lambda/)** - is a serverless, event-driven compute service that lets you run code for virtually any type of application or backend service without provisioning or managing servers.

**[Amazon OpenSearch Service](https://aws.amazon.com/opensearch-service/)** - makes it easy for you to perform interactive log analytics, real-time application monitoring, website search, and more. OpenSearch is an open source, distributed search and analytics suite derived from Elasticsearch.

**[Amazon Comprehend](https://aws.amazon.com/comprehend/)** - is a natural-language processing (NLP) service that uses machine learning to uncover valuable insights and connections in text. Derive and understand valuable insights from text within documents

**[Amazon Translate](https://aws.amazon.com/translate/)** - is a neural machine translation service that delivers fast, high-quality, affordable, and customizable language translation.

**[Amazon Lex](https://aws.amazon.com/lex/)**  Build chatbots with conversational AI. Amazon Lex is a fully managed artificial intelligence (AI) service with advanced natural language models to design, build, test, and deploy conversational interfaces in applications. 

**[Amazon Connect](https://aws.amazon.com/connect/)** -  Provide superior customer service at a lower cost with an easy-to-use cloud contact center. With Amazon Connect, you can set up a contact center in minutes that can scale to support millions of customers. 

**[Amazon Kendra](https://aws.amazon.com/kendra/)** - is an intelligent enterprise search service that helps you search across different content repositories with built-in connectors.

**[Amazon SageMaker](https://aws.amazon.com/sagemaker/)** -  Build, train, and deploy machine learning (ML) models for any use case with fully managed infrastructure, tools, and workflows.


## References
- Main topic: https://aws.amazon.com/solutions/implementations/qnabot-on-aws/?did=fs_card&trk=fs_card
- All AWS architecture resources: https://aws.amazon.com/architecture/
- AWS architecture by customers: https://www.youtube.com/playlist?list=PLhr1KZpdzukdeX8mQ2qO73bg6UKQHYsHb