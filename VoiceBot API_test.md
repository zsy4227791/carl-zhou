  | Change Version | API Version | Change nots | Change Date | Author |
  | - | - | - | - | - |
  | 2.0 | v1 | Voice Bot API | 2022-2-22 | Leon |  
 



# Summary
  - Voicebot
    - [VoiceBotSession](#VoiceBotsession-object)
       - [VoiceBotResponse](#VoiceBotresponse-object) 
          -[VoiceBotAction](#VoiceBotAction-object) 


## Voice Bot API
  - `POST /voicebots/{chatbotId}/sessions` - [Create session](#create-session)
  - `POST /sessions/{sessionId}:recieveMessage` - [Recieved Message](#Recieved-Message)
## Twillio Adapter  

## Sip Adapter



# Endpoints

### Create A New VoiceBot Session
`POST /bot/VoiceBotSessions`

#### Parameters
Request body

The request body contains data with the follow structure:

  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `VoiceBotId` | Guid | yes | |  the unique id of the bot |
  |`visitor`  |  [Visitor](#visitor-object) Object  |no |   |  |

example:
```Json 
  {
    "VoiceBotId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
    "channel": "LiveChat",
    "visitor": {
        "name":"Kart",
        "email":"kart@yahoo.com",
        "phone":"123-4355-212",
      }
    }
  }
```

#### Response
The Response body contains data with the follow structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`sessionId` | Guid | the unique id of the session |
  |`greeting`  |  [VoiceBotOutput](#VoiceBotoutput-object) Object    |  |


#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
    "VoiceBotId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48", 
    "visitor": {
      "name":"Kart",
      "email":"kart@yahoo.com",
      "phone":"123-4355-212",
    }
  }' -X POST https://domain.comm100.com/api/v4/bot/VoiceBotSessions
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json

  {    
    "sessionId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
    "greeting":{
          "id":"d3f5b968-ad51-42af-b759-64c0afc40b84",
          "content":[{
              "type":"VoiceBotActionSendMessage",
              "content":{
                      "VoiceBotActionSendMessageLinks": [],
                    "message": "Hi there! I'm a VoiceBot, here to help answer your questions.",
                    "nextActionId": "00000000-0000-0000-0000-000000000000",
                    "typingDelay": 1
              }
    }]
    }

  }
```

# Model

### VoiceBotSession Object
   

  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `id` | Guid  | | sessionId |
  | `interactions` |  [VoiceBotInteraction](#VoiceBotinteraction-object)[] Object |  |  |
  | `context` | VoiceBotSessionContext Object  |   |  |
### VoiceBotInteraction Object

  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `id` | Guid  | | dialogId |
  | `input` |  [VoiceBotInput](#VoiceBotinput-object) Object|  |  |
  | `output` |  [VoiceBotOutput](#VoiceBotoutput-object) Object |  |  |
### VoiceBotInput Object

  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `id` | Guid  | | questionId |
  | `type` | String  | | 	type of the response,including`text`,`audio`,`location`,`option`,`form`,`transferchat`|
  | `content` | object  | | [textInput](#textinput-object), [audioInput](#audioinput-object), [locationInput](#locationinput-object), [optionInput](#optioninput-object), [formInput](#forminput-object) , [Transferchat](#transferchat-object)|

  ### TextInput Object
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `text` | String  | |  |
  ### AudioInput Object
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `text` | String  | |  |
  | `audio` | String  | |  |
  ### LocationInput Object
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `location` | String  | | the longitude and latitude of the location, e.g. "-39.900000,116.300000" |
  | `action` | string  | | submit, cancel |

  ### OptionInput Object
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
| `optionId` | String  | |optionId |
  ### FormInput Object
  |Name| Type | Default | Description | 
  | - | - | :-: | - |
  | `formValues` | [FieldValue](#FieldValue-object)[]  | |  an array of [FieldValue](#FieldValue-object) objects |
  | `formId` | Guid  | |  |
 | `action` | string  | | submit, cancel |
  
### FieldValue Object

|Name| Type|  Default |  Description     |
| - | - | :-: |  - | 
|`name` | string |  | the name of a field in a form. |
|`value` | string |  | the value of a field. |
### VariableValue Object
|Name| Type|  Default |  Description     |
| - | - | :-: |  - | 
|`name` | string |  | the name of a variable in a form. |
|`value` | string |  | the value of a variable. |
### VoiceBotOutput Object
  VoiceBotMessage Object is represented as simple flat JSON objects with the following keys:  

  |Name| Type| Default | Description     |
  | - | - | :-: | - |
  | `id` | Guid  |  | the unique id of the response |
  | `content` | [VoiceBotResponse](#VoiceBotresponse-object)[]|  |   |


### VoiceBotResponse Object
  Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`type` | string | | type of the response,including `PlayAudio`,`PlayText`,`ClearValue`,`CollectDTMFDigits`,`CollectSpeechResponse`,`Condition`,`EndCall`,`GoToIntent`,`IVRMenu`,`SetVariableValue`,`TransferChat`,`Webhook`|
  | `content` | object | |  response's content. when type is `PlayAudio`, it represents [PlayAudio](#PlayAudio-object); when type is `PlayText`,it represents [PlayText](#PlayText-object);when type is `ClearValue`,it represents [ClearValue](#ClearValue-object);when type is `CollectDTMFDigits`,it represents [CollectDTMFDigits](#CollectDTMFDigits-object); when type is `CollectSpeechResponse`, it represents [CollectSpeechResponse](#CollectSpeechResponse-object);when type is `Condition`, it represents [Condition](#Condition-object);when type is `EndCall`, it represents [EndCall](#EndCall-object);when type is `GoToIntent`, it represents [GoToIntent](#GoToIntent-object);when type is `IVRMenu`, it represents [IVRMenu](#IVRMenu-object);when type is `SetVariableValue`, it represents [SetVariableValue](#SetVariableValue-object);when type is `TransferChat`, it represents [TransferChat](#TransferChat-object);when type is `Webhook`, it represents [Webhook](#Webhook-object);|
  |`delayTime` | decimal | 1 | how many seconds delay to show  |

#### PlayAudio Object
  Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  | Id of the Action.  |
  |`AudioPath` | String|  | String  |
  |`NextActionId` | Guid|  | Id of the  Next Action. |
#### PlayText Object
  Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  | Id of the Action.  |
  |`Message` | String|  | String  |
  |`NextActionId` | Guid|  | Id of the  Next Action. |
#### ClearValue Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  | Id of the Action.  |
  |`VariableId` | Guid|  | Id of the VariableId  |
  |`NextActionId` | Guid|  | Id of the  Next Action. |
 #### CollectDTMFDigits Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  | Id of the Action.  |
  |`Message` | String|  | String  |
  |`VariableName` | String|  | String  |
  |`NumberOfDigits` | Int|  | DTMF Digits  |
  |`StopGatherAfterPresskey` | int|  | int  |
  |`NextActionId` | Guid|  | Id of the  Next Action. |
#### CollectSpeechResponse Object
  Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  | Id of the Action.  |
  |`Message` | String|  | String  |
  |`VariableName` | String|  | String  |
  |`LowSTTConfidenceMessage` | String|  | String  |
  |`LowSTTConfidenceRepeatTimes` | Int|  | Int  |
  |`IsConfirmationRequired` | Bool|  | int  |
  |`ConfirmationMessage` | String|  | String  |
  |`ConfirmationText` | String|  | String  |
  |`ConfirmationKey` | int|  | int  |
  |`ActionIdWhenLowSTTConfidence` | Guid|  | int  |
  |`NextActionId` | Guid|  | Id of the  Next Action. |
#### Condition Object
  Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  | Id of the Action.  |
  |`OtherCaseActionId` | Guid |  | Id of the other case Action.  |
  |`VoicebotActionConditionCase` | [VoicebotActionConditionCase[]](#VoicebotActionConditionCase-object) |  |   |
#### VoicebotActionConditionCase Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  | Id of the Action.  |
  |`Order` | Int |  | Int  |
  |`ConditionExpressionType` | Type |  | Allowed values are "all", "any", "logicalExpression".  |
  |`LogicalExpression` | string |  | string  |
  |`GoToActionId` | Guid |  | Id of the Action.  |
  |`Id` | Guid |  | Id of the Condition Case.  |
  |`VoicebotActionConditionCaseConditions` | [VoicebotActionConditionCaseConditions[]](#VoicebotActionConditionCaseConditions-object) |  |   |

#### VoicebotActionConditionCaseConditions Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionConditionCaseId` | Guid |  | Id of the Condition Case.  |
  |`FieldName` | [FieldName](#FieldName-object) |  |   |
  |`operator` | Type |  | Allowed values are "is", "contains", "notContains", "isMoreThan", "isLessThan", "isNot", "isNotLessThan", "isNotMoreThan", "regularExpression", "isOneOf", "isNotIn", "dateNotEqualTo", "before", "after", "dateEqualTo".  |
  |`Value` | string |  | string  |
  |`Order` | Int |  | Int  |
  |`Id` | Guid |  | Guid  |
  |`Items` | List |  | List  |


#### FieldName Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Operator|
  | - | - |
  |`{!Visitor.Country/Region}` | is/isNot/contains/notContains/regularExpression |
  |`{!Visitor.State/Province}` | is/isNot/contains/notContains/regularExpression |
  |`{!Visitor.City}` | is/isNot/contains/notContains/regularExpression |
  |`{!Visitor.Time Zone}` | isOneOf/isNotIn |
  |`{!Visitor.Language}` | is/isNot/contains/notContains/regularExpression |
  |`{!Visitor.CardId}` | is/isNot/contains/notContains/regularExpression |
#### TransferChat Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  |  Id of the Action. |
  |`TransferTo` | String |  | String |
  |`ActionIdWhenTransferFailed` | Guid |  | Guid  |

#### EndCall Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  |  Id of the Action. |
  
#### GoToIntent Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  |  Id of the Action. |
  |`IntentId` | Guid |  |  Id of the Intent. |

#### IVRMenu Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  |  Id of the Action. |
  |`Message` | String |  |  String |
  |`InvalidInputMessage` | String |  |  String |
  |`InvalidInputRepeatTime` | Int |  |  Int |
  |`ActionIdWhenInvalidInput` | Guid |  |  Guid |
  |`VoicebotActionIVRMenuOptions` | [VoicebotActionIVRMenuOptions[]](#VoicebotActionIVRMenuOptions-object) |  |  List |
  
#### VoicebotActionIVRMenuOptions Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`Text` | String |  |  String |
  |`Key` | Int |  |  Int |
  |`NextActionId` | Guid |  |  Id of the Next Action. |
  |`VoicebotActionId` | Int |  |   Id of the Action. |
  |`Order` | Int |  |  Int |
  
#### SetVariableValue Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  |  Id of the Action. |
  |`Value` | String |  |  String |
  |`SaveToType` | Type |  | Allowed values are "LiveChat", "CustomVariable", "Tick", "Variable". |
  |`FieldId` | Guid |  |  Id of the Field |
  |`Field` | [Field[]](#Field-object) |  |   |
  |`CustomVariableId` | Guid |  | Id of the Custom Variable  |
  |`CustomVariable` | [CustomVariable[]](#CustomVariable-object) |  |   |
  |`TicketingFieldId` | Guid |  | Id of the Custom Ticket Field.  |
  |`TicketingField` | [TicketingField[]](#TicketingField-object) |  |   |
  |`Variable` | String |  |  String |

#### Field Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`Id` | Guid |  |  Id of the Field. |
  |`Name` | String |  |  Name of the Field. |
 
#### CustomVariable Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`Id` | Guid |  |  Id of the CustomVariable. |
  |`Name` | String |  |  Name of the CustomVariable. |
  
#### TicketingField Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`Id` | Guid |  |  Id of the TicketingField. |
  |`Name` | String |  |  Name of the TicketingField. |

### Form Object
FormReplyResponse is represented as simple flat json objects with the following keys:

|Name| Type| Default | Description     | 
| - | - | :-: | - | 
|`message` | string |   | A separate message which is sent before the button is sent.|
|`title` | string |  | when a button is sent to visitor, clicking this button will open a form that contains information bot wants to collect from the visitor. the title refers to the title of that form, and it is also placed on the button as a name.|
|`isConfirmationRequired` | bool |   | whether visitor needs to click confirm after filling out the information in a form.|
|`fields` | [Field](#field-object)[] | | an array of [Field](#field-object) Object |
|`submitButtonText` | string |   | |
|`cancelButtonText` | string |   | |
|`confirmButtonText` | string |   | |


### Field Object
Field is represented as simple flat json objects with the following keys:

|Name| Type| Default | Description     | 
| - | - | :-: | - | 
|`type` | string | | enums: `text` ,`textArea`,`radio` ,`checkBox` ,`dropDownList` ,`checkBoxList`,`email` type refers to the different kinds of fields which can be used in a form. |
|`name` | string |  | a field’s name in a form. |
|`defaultValue` | string | | a field’s value |
|`isRequired` | bool |  | to mark whether a field in a form is required or not. |
|`isMasked` | bool |  | if this is true, visitor information will be masked with symbols in chat logs. |
|`options` | string[] |  | an array of of string when the fieldType is `radio` ,`dropDownList` ,`checkBoxList`|
|`order` | integer |  | must greater than or equal 0, ascending sort |

### Visitor Object

|Name| Type|  Default |  Description     |
| - | - | :-: |  - | 
|`name` | string |  |  |
|`email` | string |  |  |
|`phone` | string |  |  |
|`state/province` | string |  |  |
|`country/region` | string |  |  |
|`city` | string |  |  |
|`ip` | string |  |  |
|`email` | string |  |  |
|`currentPageURL` | string |  |  |
|`searchEngine` | string |  |  |
|`searchKeywords` | string |  |  |


