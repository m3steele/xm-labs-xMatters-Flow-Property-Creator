# xMatters Flow Steps to Create Communication Plan Properties from Payload

This xMatters communication plan contains two Flow Designer Steps to help you easily and quickly create Communication Plan Properties

<kbd>
  <img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/disclaimer.png">
</kbd>

# Pre-Requisites

- xMatters account - If you don't have one, [get one](https://www.xmatters.com)!

# Files

- [xMatters Create Form Properties From Payload](CreateFormPropertiesFromPayload.zip)

# Use Cases

- Want to create lots of form properties but dont want to spend the time doing it.
- Building new integration with xMatters and need properties to match a payload from external system.
- Dynamically create event properties on the fly.

# How it works



# Included Custom Steps


- __Receive Payload__: This inbound http step sets a JSON payload to an output named "payload". This becomes the input for __Create Form Properties from Payload__ step.

- __Create Form Properties from Payload__: This custom step uses the output variable payload from the __Receive Payload__ inbound http step and creates xMatters properties based on the structure of the JSON.

<br><br>
   <kbd>
     <img src="/media/steps.png">
   </kbd>

# xMatters Configuration

## Import the xMatters Communication Plan

1. Download the [xMatters Create Form Properties From Payload](CreateFormPropertiesFromPayload.zip)

2. Follow instructions for [importing a communication plan](https://help.xmatters.com/ondemand/xmodwelcome/communicationplanbuilder/exportcommplan.htm)

## Set Usage Permissions on the Steps you would like to share

1. On the Developer tab, click Edit beside the **xMatters Rest API Flow Steps** communication plan and go to **Flows**.

2. Click on **API Form for Flow**.

3. Follow instructions for [Sharing a Custom Flow Step](https://help.xmatters.com/ondemand/xmodwelcome/flowdesigner/share-steps.htm)

## Use the steps

After importing the communication plan you will see two steps already on the canvas:
- __Receive Payload__
- __Create Form Properties from Payload__

### Get HTTP Trigger URL

1. Double click on __Receive Payload__ step.

<br><br>
   <kbd>
     <img src="/media/get-trigger-url.png">
   </kbd>
   
2. Select Authenticating User. This needs to be a user with access permissions on the communication plan you want to add properties to and the __REST Web Service User__ role.   
   
3. Copy the Trigger URL. This will be the endpoint used to initiate this Flow.


### Set Communication Plan name you want to create properties in.

1. Double click on __Create Form Properties from Payload__ step.

<br><br>
   <kbd>
     <img src="/media/comm-name.png">
   </kbd>
   
2. In the Input named __planName__, type the name of the communication plan you want the properties to be created in. This is the user friendly name of any communication plan in your xMatters environment.

<br><br>
   <kbd>
     <img src="/media/friendly-name.png">
   </kbd>


3. Go to __ENDPOINT__ tab and ensure the __xMatters__ Endpoint is selected.

<br><br>
   <kbd>
     <img src="/media/endpoint.png">
   </kbd>

### POST to xMatters Http Trigger URL using Postman or similar tool

- The xMatters http trigger step uses an api key for authentication. You will not need to include authentication headers in your POST.

1. Create new POST request.

2. Set endpoint url to the one you got from  Receive Payload http trigger in [Get HTTP Trigger URL](#get-http-trigger-url)

3. Set Body type to application/json.

4. Create a JSON object with the names of each property you would like to create in xMatters.
- Your object does not need to be flat.
- A "." will be added to your property name between each level of your JSON object.
- By changing the structure of your JSON object you can change the way properties are named.
- You must include a value as either "1" or empty"".  Valid JSON requires Key Value pairs. The value will be ignored.

- All properties will be created as:

		- Type: "TEXT"
		- Min characters: 0  
		- Max characters: 20,000
		

__All of the following JSON structures will work__:

<br><br>
```
{
  "a": 1,
  "b": 1,
  "e": 1
}
```
Creates the following properties:
- a
- b
- c


<br><br>
```
{
  "a": 1,
  "b": {
    "c": 1,
    "d": 1
  },
  "e": 1
}
```
Creates the following properties:
- a
- b.c
- b.d
- e

<br><br>
```
{
  "a": 1,
  "b": {
    "c": {
      "d": 1
    }
  },
  "e": 1
}
```
Creates the following properties:
- a
- b.c.d
- e


<br><br>
```
{
  "a": 1,
  "i": 1,
  "b": {
    "h": 1,
    "c": {
      "d": {
        "e": 1,
        "f": 1
      },
      "g": 1
    }
  }
}
```
Creates the following properties:
- a
- b.c.d.e
- b.c.d.f
- b.c.g
- b.h
- i

5. Send the POST request. You should receive 202 Accepted response.

<br><br>
   <kbd>
     <img src="/media/post.png">
   </kbd>


6. Go to Flow designer Activity stream to check everything worked as expected.

- the output __properties_created__ on __Create Form Properties__ Step will show an array with all properties that were created.
<br><br>
   <kbd>
     <img src="/media/activity.png">
   </kbd>


** Additional Notes**:
After adding properties successfully, you will still need to drag the properties on to a form.

<br><br>
   <kbd>
     <img src="/media/properties-added.png">
   </kbd>

<br><br>
   <kbd>
     <img src="/media/form.png">
   </kbd>



1. Drag the xMatters API step on the canvas and configure the step.
   [Get help using Flow designer](https://help.xmatters.com/ondemand/xmodwelcome/flowdesigner/flow-designer.htm)

The xMatters Rest API documentation will help you understand the inputs and outputs for each step.
[Get help with the xMatters API](https://help.xmatters.com/xmapi/index.html)

# Troubleshooting

[Get help using Flow designer](https://help.xmatters.com/ondemand/xmodwelcome/flowdesigner/flow-designer.htm)

[Get help with the xMatters API](https://help.xmatters.com/xmapi/index.html)
