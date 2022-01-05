# Using Elastic Beanstalk with Amazon EventBridge<a name="AWSHowTo.eventbridge"></a>

Using Amazon EventBridge, you can set up event\-driven rules that monitor your Elastic Beanstalk resources and initiate target actions that use other AWS services\. For example, you can set a rule for sending out email notifications by signaling an Amazon SNS topic whenever the health of a production environment changes to a *Warning* status\. Or, you can set a Lambda function to pass a notification to Slack whenever the health of your environment changes to a *Degraded* or *Severe* status\.

You can create rules in Amazon EventBridge to act on any of the following Elastic Beanstalk events:
+ *State changes for environment operations \(including create, update, and terminate operations\)\.* The event specifies if the state change has started, succeeded, or failed\.
+ *State changes for other resources\. *Besides environments, other resources that are monitored include load balancers, auto scaling groups, and instances\.
+ *Health transition for environments\.* The event states where the environment health has transitioned from one health status to another one\. 
+ *State change for managed updates\.* The event specifies if the state change has started, succeeded, or failed\.

To capture specific Elastic Beanstalk events that you're interested in, define event\-specific patterns that EventBridge can use to detect the events\. Event patterns have the same structure as the events they match\. The pattern quotes the fields that you want to match and provides the values that you're looking for\. Events are emitted on a best effort basis\. They're delivered from Elastic Beanstalk to EventBridge in near real\-time under normal operational circumstances\. However, situations can arise that may delay or prevent delivery of an event\.

For a list of fields that are contained in Elastic Beanstalk events and their possible string values, see [Elastic Beanstalk event field mapping](#eb-eventbridge-mapping)\. For information about how EventBridge rules work with event patterns, see [Events and Event Patterns in EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-and-event-patterns.html)\. 

## Monitor an Elastic Beanstalk resource with EventBridge<a name="eb-eventbridge-tasks"></a>

With EventBridge, you can create rules that define actions to take when Elastic Beanstalk emits events for its resources\. For example, you can create a rule that sends you an email message whenever the status of an environment changes\. 

The EventBridge console has a **Pre\-defined pattern** option for building Elastic Beanstalk event patterns\. If you select this option in the EventBridge console when you create a rule, you can build an Elastic Beanstalk event pattern quickly\. You only need to select the event fields and values\. As you make selections, the console builds and displays the event pattern\. Alternatively, you can manually edit the event pattern that you build and can save it as a custom pattern\. The console also displays a detailed **Sample Event** that you can copy and paste to the event pattern that you're building\.

If you prefer to type or copy and paste an event pattern into the EventBridge console, you can select to use the **Custom pattern** option in the console\. By doing this, you don't need to go through the steps of selecting fields and values described earlier\. This topic offers examples of both [event\-matching patterns ](#eb-eventbridge-patterns) and [Elastic Beanstalk events](#eb-eventbridge-examples) that you can use\. 

**To create a rule for a resource event**

1. Log in to AWS using an account that has permissions to use EventBridge and Elastic Beanstalk\.

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. Choose **Create rule**\.

1. Enter a **Name** for the rule, and, optionally, a description\.

1. Under **Define pattern** choose **Event pattern**\.

1. Under **Event matching pattern**, choose **Pre\-defined pattern by service**\.
**Note**  
If you already have text for an event pattern and don't need the EventBridge console to build it for you, you can select **Customer pattern**\. You can then either manually enter or copy and paste text into the **Event Pattern** box\. Select **Save** and then skip to Step 12\. 

1. Select AWS for **Service provider**\.

1. For **Service name** select **Elastic Beanstalk**\.

1. For **Event type** select **Status Change**\.

1. This step covers how you can work with the **detail type**, **status**, and **severity** event fields for Elastic Beanstalk\. As you choose these fields and the values you want to match, the console builds and displays the event pattern\. Your choices also determine how the console displays the subsequent event fields, based on the hierarchy of the fields and the grouping of their values\. For more information, see the [Elastic Beanstalk event field mapping](#eb-eventbridge-mapping) table\.
   + If you select *only one* specific value for a given field, the next field in the hierarchy is displayed in the console\. You can choose one or more values with the displayed field\. For example, if you select the radio button for **Specific detail type\(s\)**, then the *detail types* drop\-down list is displayed\. If you select only one value from this list, options for you to work with the next field are displayed in the console\. In this example, it's the *status* field that follows\. For this field, you can choose either **Any status** or **Specific status\(es\)**\.
   + If you choose *more than one* value for a given field, or if you select the *Any* radio button for that field \(for example **Any status**\), the console doesn't provide you with options for the subsequent fields\. More specifically, it doesn't provide you with the option to choose specific values for the subsequent fields in the hierarchy\. In this example, the *severity* field follows *status* in the hierarchy\. It defaults to **Any severity** if more than one *status* is chosen prior\. If you choose **Specific severity\(s\)**, then *No items* are listed in the list of values\. The console is designed in this manner to prevent ambiguous matching logic across fields in your event pattern\.

   The **environment** event field isn't affected by this hierarchy, so it displays as described in the next step\.

1.  For **Environment**, select **Any environment** or **Specific environment\(s**\)\.
   + If you select **Specific environment\(s\)**, you can choose one or more environments from the drop\-down list\. EventBridge adds all of environments that you select inside the *EnvironmentName\[ \]* list in the *detail* section of the event pattern\. Then, your rule filters all events to include only the specific environments that you choose\.
   + If you select **Any environment**, then no environments are added to your event pattern\. Because of this, your rule doesn't filter any of the Elastic Beanstalk events based on environment\.

1. The **Select event bus** section defaults to **AWS default event bus**\. Leave the default selected and confirm that **Enable the rule on the selected event bus** is toggled on\.

1. Under **Select targets**, choose the target action to take when a resource state change event is received from Elastic Beanstalk\.

   For example, you can use an Amazon Simple Notification Service \(SNS\) topic to send an email or text message when an event occurs\. To do this, you need to create an Amazon SNS topic using the Amazon SNS console\. To learn more, see [Using Amazon SNS for user notifications](https://docs.aws.amazon.com/sns/latest/dg/sns-user-notifications.html)\.
**Important**  
Some target actions might require the use of other services and incur additional charges, such as the Amazon SNS or Lambda service\. For more information about AWS pricing, see [https://aws.amazon.com/pricing/](https://aws.amazon.com/pricing/)\. Some services are part of the AWS Free Usage Tier\. If you are a new customer, you can test drive these services for free\. See [https://aws.amazon.com/free/](https://aws.amazon.com/free/) for more information\. 

1. \(Optional\) Choose **Add target** to specify an additional target action for the event rule\.

1. Choose **Create**\.

## Example Elastic Beanstalk event patterns<a name="eb-eventbridge-patterns"></a>

Event patterns have the same structure as the events they match\. The pattern quotes the fields that you want to match and provides the values that you're looking for\.
+ *Health status change* for all environments

  ```
  {
     "source": [
      "aws.elasticbeanstalk"
    ],
    "detail-type": [
      "Health status change"
      ]
  }
  ```
+ *Health status change* for the following environments: `myEnvironment1` and `myEnvironment2`\. This event pattern filters for these two specific environments, whereas the previous *Health status change* example that doesn't filter sends events for all environments\.

  ```
  {"source": [
      "aws.elasticbeanstalk"
      ],
      "detail-type": [
          "Health status change"
      ],
      "detail": {
          "EnvironmentName": [
              "myEnvironment1",
              "myEnvironment2"
          ]
      }
  }
  ```
+ *Elastic Beanstalk resource status change* for all environments

  ```
  {
    "source": [
      "aws.elasticbeanstalk"
    ],
    "detail-type": [
      "Elastic Beanstalk resource status change"
      ]
  }
  ```
+ *Elastic Beanstalk resource status change* with `Status` *Environment update failed * and `Severity` *ERROR* for the following environments: `myEnvironment1` and `myEnvironment2`

  ```
  {"source": [
      "aws.elasticbeanstalk"
      ],
      "detail-type": [
          "Elastic Beanstalk resource status change"
      ],
      "detail": {
          "Status": [
              "Environment update failed"
              ],
          "Severity": [
              "ERROR"
              ],
          "EnvironmentName": [
              "myEnvironment1",
              "myEnvironment2"
          ]
      }
  }
  ```
+ *Other resource status change* for load balancers, auto scaling groups, and instances

  ```
  {
     "source": [
      "aws.elasticbeanstalk"
    ],
    "detail-type": [
      "Other resource status change"
      ]
  }
  ```
+ *Managed update status change* for all environments

  ```
  {
     "source": [
      "aws.elasticbeanstalk"
    ],
    "detail-type": [
      "Managed update status change"
      ]
  }
  ```
+ To capture *all events* from Elastic Beanstalk \(exclude the `detail-type` section\)

  ```
  {
    "source": [
      "aws.elasticbeanstalk"
    ]
  }
  ```

## Example Elastic Beanstalk events<a name="eb-eventbridge-examples"></a>

The following is an example Elastic Beanstalk event for a *resource status change*:

```
{ 
   "version":"0",
   "id":"1234a678-1b23-c123-12fd3f456e78",
   "detail-type":"Elastic Beanstalk resource status change",
   "source":"aws.elasticbeanstalk",
   "account":"111122223333",
   "time":"2020-11-03T00:31:54Z",
   "region":"us-east-1",
   "resources":[
      "arn:was:elasticbeanstalk:us-east-1:111122223333:environment/myApplication/myEnvironment"
   ],
   "detail":{
      "Status":"Environment creation started",
      "EventDate":1604363513951,
      "ApplicationName":"myApplication",
      "Message":"createEnvironment is starting.",
      "EnvironmentName":"myEnvironment",
      "Severity":"INFO"
   }
}
```

The following is an example Elastic Beanstalk event for a *health status change*:

```
{ 
   "version":"0",
   "id":"1234a678-1b23-c123-12fd3f456e78",
   "detail-type":"Health status change",
   "source":"aws.elasticbeanstalk",
   "account":"111122223333",
   "time":"2020-11-03T00:34:48Z",
   "region":"us-east-1",
   "resources":[
      "arn:was:elasticbeanstalk:us-east-1:111122223333:environment/myApplication/myEnvironment"
   ],
   "detail":{
      "Status":"Environment health changed",
      "EventDate":1604363687870,
      "ApplicationName":"myApplication",
      "Message":"Environment health has transitioned from Pending to Ok. Initialization completed 1 second ago and took 2 minutes.",
      "EnvironmentName":"myEnvironment",
      "Severity":"INFO"
   }
}
```

## Elastic Beanstalk event field mapping<a name="eb-eventbridge-mapping"></a>

The following table maps Elastic Beanstalk event fields and their possible string values to the EventBridge `detail-type` field\. For more information about how EventBridge works with event patterns for a service, see [Events and Event Patterns in EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-and-event-patterns.html)\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.eventbridge.html)