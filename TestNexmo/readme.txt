code[
Nexmo Java

You use Nexmo Java to rapidly integrate Nexmo functionality into your Apps:

SMS - send and receive a high volume of SMS anywhere in the world within minutes.
Verify - check that a user has access to a specific phone number.
Number Insight - retrieve information such as number validity, number type, number presence and more for a given number.
Note: to ensure privacy, HTTPS is used for all interaction with Nexmo.

To integrate Nexmo Java into your App you need to:

Fulfill the prerequisites
Build Nexmo Java
Run the examples
Integrate Nexmo Java into your App
Code Nexmo functionality into your App

Fulfill the prerequisites

In order to use Nexmo Java:

Download and Install Java and Ant on your local machine.
Make sure that you can run Java and Ant from the command line. See ant.apache.org/manual/index.html.
In a directory, <NEXMOJAVADIR>, clone this repository locally from GitHub: https://github.com/Nexmo/nexmo-java.

Build Nexmo Java

To build Nexmo Java:

Open a terminal on your machine and navigate to <NEXMOJAVADIR>.
Use the following command to build the library:
ant compile

Run the examples

The example sample constructs a message request then submits it to Nexmo.

To compile Nexmo Java and execute this first example:

Open a terminal on your machine and navigate to <NEXMOJAVADIR>.
Edit src/com/nexmo/messaging/sdk/examples/SendTextMessage.java and update the following:
public static final String 'API_KEY' = "API_KEY";
public static final String 'API_SECRET' = "API_SECRET";
public static final String SMS_FROM = "441632960961";
public static final String SMS_TO = "441632960960";
public static final String SMS_TEXT = "Nexmo Java: Hello World!";
public static final String TOPIC_ARN = "arn:was:sns:us-east-1:10000000009:my_sms_channel";
Use the following command to run the example:
ant clean example
You can now run all the examples:

Example	Description
example	In SendTextMessage we create a new NexmoSmsClient('API_KEY', 'API_SECRET') and send a new TextMessage(SMS_FROM, SMS_TO, SMS_TEXT) using NexmoSmsClient.submitMessage and print out the SmsSubmissionResult.
example2	In SendWapPush we create a new NexmoSmsClient('API_KEY', 'API_SECRET') and send a new WapPushMessage(WAP_PUSH_FROM, WAP_PUSH_TO, WAP_PUSH_URL, WAP_PUSH_TITLE) using NexmoSmsClient.submitMessage and print out the SmsSubmissionResult.
example-signed	In SendSignedTextMessage we create a new NexmoSmsClientSignedRequests('API_KEY', 'API_SECRET') and send a signed TextMessage(SMS_FROM, SMS_TO, SMS_TEXT) using NexmoSmsClient.submitMessage and print out the SmsSubmissionResult.
sns-example	In SubscribeAndPublishExample we create a new NexmoSnsClient('API_KEY', 'API_SECRET') then:
Send a new SubscribeRequest(TOPIC_ARN, SMS_TO) using NexmoSnsClient.submit and print out the SubscribeResult.
Send a new PublishRequest(TOPIC_ARN, SMS_FROM, SMS_TEXT) using NexmoSnsClient.submit and print out the PublishResult.

Integrate Nexmo Java into your App

To integrate Nexmo Java into your App:

Open a terminal on your machine and navigate to <NEXMOJAVADIR>.
Use the following command to build the library:
ant compile
Copy nexmo-sdk.jar to your App.
Add the dependancies in /lib to your App.
Ensure that these libraries are in your CLASSPATH.
Nexmo Java is ready to use. You pass all configuration information in the constructors of the different objects.


Code Nexmo functionality into your App

The following examples show you how to:

Code Messaging functionality into your App
Code Verify functionality into your App
Code Number Insight functionality into your App
Code Nexmo SNS functionality into your App

Code Messaging functionality into your App

To integrate Nexmo Messaging functionality in your App:

Choose a client class:
NexmoSmsClient - send SMS messages anywhere in the world using Nexmo.
NexmoSmsClientSignedRequests - an extension of NexmoSmsClient that sends requests using an MD5 signature derived from a secret key.
Choose a messages class:
com.nexmo.messaging.sdk.messages.TextMessage;
com.nexmo.messaging.sdk.messages.BinaryMessage;
com.nexmo.messaging.sdk.messages.WapPushMessage;
com.nexmo.messaging.sdk.messages.UnicodeMessage;
Instantiate your object:
smsClient = new NexmoSmsClient('API_KEY', 'API_SECRET');
Send your request:
results = smsClient.submitMessage(new TextMessage(SMS_FROM, SMS_TO, SMS_TEXT));
Check your results:
if (results[i].getStatus() == SmsSubmissionResult.STATUS_OK) System.out.println("SUCCESS");

This method returns an array of SmsSubmissionResult objects with 1 entry for each SMS sent. Certain messages, such as text messages greater than 160 characters, are send in multiple SMS. Each SmsSubmissionResult in this array has an individual messageId and a status that explains if the message was sent successfully, or why sending failed.

For a full list of the status codes.


Code Nexmo Verify functionality into your App

To integrate Verify functionality in your App:

Instantiate a NexmoVerifyClient object to validate that a user has access to a phone number:
Verify: *verifyClient = new NexmoVerifyClient(getApiKey(), getApiSecret());
Send your request:
VerifyResult r = client.verify(getProperty("verify.number");
Check your results:
VerifyResult r = verifyClient.verify(getProperty("verify.number"), getProperty("verify.brand"), getProperty("verify.sender"), 6, Locale.US); assertEquals(VerifyResult.STATUS_OK, r.getStatus());
For more information about the response codes, see the Verify Check Response Codes.


Code Nexmo Number Insight functionality into your App

To integrate Number Insight functionality in your App:

Instantiate a NexmoInsightClient object to search for information about a phone number:
niClient = new NexmoInsightClient(getApiKey(), getApiSecret());
Send your request:
InsightResult r = niClient.request( getProperty("insight.number"), getProperty("insight.callback.url"), features, getLong("insight.callback.timeout"), getProperty("insight.callback.method", false), getProperty("insight.callback.clientRef", false), getProperty("insight.callback.ipAddress", false));
Check your results:
assertEquals(InsightResult.STATUS_OK, r.getStatus()); assertEquals(getProperty("insight.number"), r.getNumber()); assertEquals(0.03f, r.getRequestPrice(), 0);
For more information about the response codes, see the Number Insight Response.


Code Nexmo SNS functionality into your App

To integrate Nexmo SNS functionality in your App:

Setup SNS access.
Instantiate a NexmoSnsClient using your Nexmo api_key and api_secret in the constructor.
Instantiate a SubscribeRequest to subscribe a phone number to your SNS service.
Pass the SubscribeRequest object to the submit method of your NexmoSnsClient instance.
This returns a SubscribeResult with the service subscription status.
Create a PublishRequest to trigger a message to your users.
Pass the PublishRequest object to the submit method of your NexmoSnsClient instance.
This returns a PublishResult withg the message submission status.
Possible return codes are:

STATUS_OK = 0;
STATUS_BAD_COMMAND = 1;
STATUS_INTERNAL_ERROR = 2;
STATUS_INVALID_ACCOUNT = 3;
STATUS_MISSING_TOPIC = 4;
STATUS_INVALID_OR_MISSING_MSISDN = 5;
STATUS_INVALID_OR_MISSING_FROM = 6;
STATUS_INVALID_OR_MISSING_MSG = 7;
STATUS_TOPIC_NOT_FOUND = 8;
STATUS_TOPIC_PERMISSION_FAILURE = 9;
STATUS_COMMS_FAILURE = 13;
]
