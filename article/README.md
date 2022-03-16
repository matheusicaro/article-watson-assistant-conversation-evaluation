# Article: Chatbot in production? Generate report for evaluation of responses in Watson Assistant 
[![license](https://img.shields.io/github/license/DAVFoundation/captain-n3m0.svg?style=flat-square)](https://github.com/matheusicaro/template-watson-assistant-conversation-evaluation/blob/master/LICENSE)

## Summary:

- [Part - 1](#part---1-chatbot-in-production-generate-report-for-evaluation-of-responses-in-watson-assistant)
- [Part - 2](#part---2-chatbot-in-production-generate-report-for-evaluation-of-responses-in-watson-assistant)

## Part - 1: Chatbot in production? Generate report for evaluation of responses in Watson Assistant 

![img-1](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/1.gif)
<br>
<br>

Not always the developers of Chatbots, they demonstrate the excitement to carry out the evolution process that is to analyze the user's interaction with the bot and evaluate the correct errors in order to teach and improve the flow and the responses of your Chatbot. However, in the depths of our consciousness, we know that this process is very important, if not, fundamental to being able to evolve the knowledge and expertise of a chatbot.


In the Hop team, we always look for a strategy to accomplish this task, in a nimble and pleasant way, and with this, we have been able to develop reports to evaluate Chatbot’s responses, which has aroused a lot of interest and satisfactory results.

We will use Watson Assistant, in case you do not know it, follow some recommendations below so you can know a little.

> - **[Watson Assistant](https://cloud.ibm.com/docs/services/assistant?topic=assistant-tutorial)**
>
> - **[Dialog, how it works?](https://cloud.ibm.com/docs/services/assistant?topic=assistant-dialog-overview)**
>
> - **[Intentions](https://cloud.ibm.com/docs/services/assistant?topic=assistant-intents)**: *are purposes or goals expressed in a customer entry, such as answering a question or processing an account payment. By recognizing the intent expressed in a customer’s input, the Watson Assistant service can choose the correct dialog stream to respond to this.*


This short story will be divided into two parts, the first part will be an introduction to the problem and demonstration of the hypothesis and the other part we will implement the implementation in Watson Assistant.


Let’s start!


In common projects, the sprint design of Chatbot is designed in a flow of interactions that are messages exchanged with the user until arriving at a final answer that will solve a certain problem or bring the information sought by the user. See the example below:

![img-2](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/2.png)


In this dialog, we can realize that the user problem has been solved with the intention of changing your password, #access. However, we might not have noticed some user dissatisfaction with the delivered response in analyzing of reports the conversations between chatbot and users.


#### To get an idea, imagine a daily flow of 1000 messages a day?

#### Now imagine how tedious it would be to scan all messages to check user dissatisfaction like this?


The objective is to evaluate the responses that are delivered by a chatbot, and to hear a negative evaluation, should capture feedback if the user wanted to inform.


Now see below the dialog following the new implementation:


![img-3](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/3.png)

![img-4](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/4.png)


In summary of the image, the following interaction was performed:


1. CHATBOT: Performs a greeting when initiating the interaction;
2. USER: Request information or report a problem;
3. CHATBOT: Identifies the user’s intent and requests data to generate the response;
4. USER: Reports the requested data;
5. CHATBOT: Delivers the response to the acknowledged intent and requests an evaluation of the user for the response delivered;
6. USER: Evaluates if the answer delivered by chatbot solved your problem;
7. CHATBOT: Ask the customer if he would like feedback to correct the answer that was negative;
8. USER: Informs that you would like to send feedback;
9. CHATBOT: Informs you that you will capture feedback on the next interaction;
10. USER: Enter the feedback and send it as a message;
11. CHATBOT: Ask if the user’s message is correct and if you can proceed with the sending.
12. USER: Confirms feedback;


As a purpose of this short story, we will implement this response assessment in Watson Assistant in a way that is generic to any new project, such as a boilerplate. For the implementation, I will consider that you have a copy of the messages in a database so that it is possible to generate the reports later. Let’s follow the following chatbot implementation architecture:


![img-5](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/5.gif)


1. The users interact with the Watson Assitant through a Front End;
2. The Front-end sends the user’s message to the API-Middleware;
3. API-Middleware sends the message to be processed in the Watson Assistant tree
4. Watson Assistant returns to the API-Middleware a response to the processed message;
5. API-Middleware writes the message to the database and also returns the response from Watson Assistant to the Front-end.


Following the architecture model above, we were able to have a copy of the dialogs in the database.


![img-6](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/6.png)


Since we will have all the messages of the conversation flow with Watson Assitant in our database, we can quantify the data and build analysis reports like this:


![img-7](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/7.png)


A report like this can help us locate the intentions that have received negative ratings, as we imagined in the beginning for the Service Chatbots that can have huge amounts of dialogs per day, we can work with agility to correct them and also identify the points that are more searched by users.


In the second part of this story, we will implement step by step Watson Assistant to generate the report of the evaluation of answers and also a report of general conversations in which we will be able to visualize the feedback of the user.

---

## Part - 2 Chatbot in production? Generate report for evaluation of responses in Watson Assistant 

> *For the second part of this story, we will implement step by step in the watson Assistant a boilerplate to evaluate the responses delivered by Chatbot using the Watson Assitant and generate reports that represents demonstrate the evaluations.*

- If you did not see the [first part of this story](#part---1-chatbot-in-production-generate-report-for-evaluation-of-responses-in-watson-assistant), please read to understand the problem and its application.
- [The workspace of Watson Assistant](https://github.com/matheusicaro/template-watson-assistant-conversation-evaluation/blob/master/skill-Boilerplate-Evaluation-of-Responses.json)


![img-8](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/8.png)
<div align="center">
Image [1]: Response Evaluation Report
</div>

![img-9](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/9.png)
<div align="center">
Image [2]: Report of All Messages of Dialog with Feedback
</div>


For the Watson Assistant processing tree according to the conversation flow in the [first part of this story](#part---1-chatbot-in-production-generate-report-for-evaluation-of-responses-in-watson-assistant), they were implemented as follows:


![img-10](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/10.gif)
<div align="center">
Image [3]: Watson Assistant Processing Tree Flow
</div>


Following the stream above representing the navigation in the Watson Assistant dialog tree, we will go through each node of the tree and implement the context variables that will map the data we need to assemble the reports. If you do not know what [context variables](https://www.ibm.com/cloud/blog/enhance-chatbot-conversation-context-variables-system-entities) are and how they work, see this tutorial for a better understanding.


![img-11](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/11.png)
<div align="center">
Image [4]: Evaluating Response
</div>


1. Watson Assistant recognizes the intent **#ACCESS** and requests the user’s email forwarding to the next node;
2. At this node **(@account: email)** of the tree, when an e-mail pattern is recognized, the response is sent to the user;
3. Also in this node **(@account: email)**, the context variables **$responseDelivered** are set to true because it means that the response has been delivered, and the other **$responseIntention** context variable contains the integer that represents the response contained in this node that goes be identified in the response evaluation reports.


![img-12](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/12.png)
<div align="center">
Image [5]: Message Saved in the Database.
</div>


Note that when this response is sent to our API Middleware, we will have this object as a response from Watson Assistant and we will be able to store this data from the context variable in the database.


4. After the message is delivered, Jump to the node of the tree that represents the evaluation of this answer, because it is in this node that we will collect the satisfaction of the user if the response delivered by the bot solved its problem.
5. Tree node for response evaluation.


<div align="center">
...
</div>


Continuing the flow …


![img-13](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/13.png)
<div align="center">
Image [6]: Node to Evaluate Responses
</div>


1. This **node (Response Evaluation)** represents the response evaluation, notice that it is the last block of the main tree and will not be accessed because it is below the **“anything_else”** node that is responsible for handling all the cases that were not recognized by the chatbot. However, this node will only be accessed when it receives a forwarding, and this is what we want this node to be accessed only when the message is delivered to the proper intent it identifies.
2. In this **node (Response Evaluation)** were the answer **“Was this answer helpful to you?”** Entered with the options for evaluation, and then should be directed to the internal node. Also, let’s delete **context variables $responseDelivered** because the message has already been delivered in a previous node, right? So let’s delete this variable that still exists in the flow of the dialog so as not to compromise our reports that will be generated later.


![img-14](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/14.png)
<div align="center">
Image [7]: Deleting Context Variable $responseDelivered
</div>


3. In this **node (Check Selected Option)** you must identify which option was selected by the user.
4. Still in this **node (Check Selected Option)** notice that the condition is set to **(True)** because after user interaction, we want this node to be accessed. Note: It could have two internal nodes, but I prefer to do with just one node and use several conditioned responses inside this node.
5. In this step, notice that when the user’s message is acknowledged as **positive** we will only respond with a thank you. And for the **negative** answer, we will direct to the **Feedback** node and try to collect user feedback.
6. In this step when accessing the settings of each response, we will configure two new **context variables** that will map the response that was entered by the user. When evaluating positive by selecting the option **“Yes, it helped me.”** Let’s map as **$responseAvaliationUser = “positive”** and for negative evaluation we will map as **$responseAvaliationUser = “negative”**.


![img-15](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/15.png)
<div align="center">
Image [8]: Configuring Answers from Step 6.
</div>


![img-16](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/16.png)
<div align="center">
Image [9]: Message Saved From Step 6 Answers.
</div>

After collecting the user’s evaluation and continuing our dialogue flow in the tree, when it is evaluated as negative we will direct it to a Feedback node in order to collect the feedback.

![img-17](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/17.png)
<div align="center">
Image [10]: Node to Collect Feedback.
</div>


1. In this **node (Feedback)** when it is identified that the user wants to send feedback, it will be responsible for collecting user feedback.
2. In this node we have to delete **the context variable $responseAvaliationUser** because if this node is accessed, we will consider that a response has already been evaluated, and thus, we will only have one evaluation collected in the database. Remember that for each node of the Watson Assistant, it will be storing an object in the database, and with this, we will not want to have duplicate data. According to the image [9] that demonstrates the object of the message already saved in the database.
3. After the user selects one of the options, it will be redirected to the internal nodes.
4. When it is recognized as **@positive** through the **“Yes, I want to give you feedback.”** Option, it will be accessed from the internal node.


<div align="center">...</div>


![img-18](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/18.png)
<div align="center">
Image [11]: Internal Node Collecting Feedback.
</div>


1. When the **node @confirmation: positive** is acknowledged, it should respond to the user with one message and then forward it to the internal node.
2. Still for the **node @confirmation: positive** sends the message to the user. **“OK! please provide your feedback below … “**.
3. This **node (Get Text of feedback)** will be set to true because once you receive the user interaction that is the feedback, it will be accessed for confirmation of submission.
4. Still in the **node (Get Text of feedback)** we use the **context variable $feedback** to store the last input of the user.
5. Still for the **node (Get Text of feedback)** we demonstrate the captured input and give the options to the user to confirm if it is in fact their feedback. Another option is to correct the feedback before sending, and finally another option if you want to give up sending the feedback.
6. Demonstrates the Watson Assistant message with the collected feedback and confirmation options.


<div align="center">...</div>


In this way, for the options offered we will perform the treatment for each option according to the image below, and this node is conditioned to true because after the user interacts, we want to be accessed this node for confirmations in the feedback collection process.


![img-19](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/19.png)
<div align="center">
Image [12]: Node Responsible for Finalizing the Feedback Process.
</div>


1. Options for setting up the answers.


<div align="center">...</div>


For the internal options of the node at the image [10], we have:


![img-20](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/20.png)
<div align="center">
Image [13]: Node Options of Image [10].
</div>


1. For the **positive optio**n recognized by Watson Assistant, let’s set a **$feedbackApproved** environment variable to true, which should map the feedback is correct and mark it in the database.


![img-21](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/21.png)
![img-22](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/22.png)
<div align="center">
Image [14]: Feedback Message Saved in the Database.
</div>

2. For the **negative option** recognized by Watson Assistant, we will configure only to perform a Jump to the node (**@confirmation: positive**) that should return to the feedback collection process.
3. For the output option recognized by the Watson Assistant, it can be directed to any other node. In this case, we are only informing the user that we recognize his decision and that he can type anything else to continue the interaction with the chatbot


<div align="center">...</div>


Now, to generate the reports demonstrated at the beginning, it is necessary to develop a code that can build the Xls file and consume the data that we have already allocated in the database, through Queries such as:


- Search for all messages that have been delivered with the respective topics…


![img-23](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/23.png)


- Search for all messages that have been evaluated as negative…


![img-24](https://https://github.com/matheusicaro/article-watson-assistant-conversation-evaluation/blob/master/article/24.png)


<div align="center">...</div>


So ending this little story, this was an implementation that was able to help me and our team of Chatbot Developers of the Hop of the Camss Group. I hope you can help other Chatbots developers and feel free to suggest something or fix something.


- [The workspace of Watson Assistant in this example](https://github.com/matheusicaro/template-watson-assistant-conversation-evaluation/blob/master/skill-Boilerplate-Evaluation-of-Responses.json)


I would like to express my thanks to the entire team of [Hop](http://hop.digital/) Developers of the [Camss](http://hop.digital/) Group team who were responsible for developing the assessment technique and reports.


