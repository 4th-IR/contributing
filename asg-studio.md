# Table of Contents

[Exposing Rest APIs to ASG Studio 2](#_Toc85030586)

   [1.The Swagger Interface 2](#_Toc85030587)

[2. Enabling the port that lets you use the swagger interface](#2.-Enabling-the-port-that-lets-you-use-the-swagger-interface)

[3.Creating a Provider 4](#_Toc85030589)

[4.Creating the Skill 6](#_Toc85030590)

[4.1 Overview of the Skill Section 6](#_Toc85030591)

[4.2 Worked Example of adding a skill 6](#_Toc85030592)

[5.Creating the Engine 8](#_Toc85030593)

[6.Testing your skill on ASG Studio 10](#_Toc85030594)

## Exposing Rest APIs to ASG Studio

#### 1. The Swagger Interface

Swagger is an interface for allowing easy use of the Rest APIs you created on the ASG Studio platform. Once a rest API has been documented in this way on swagger, it will appear inside the skills section on ASG Studio, and if everything works you will be able to call it into a process as a datasource. The swagger page used for connecting with ASG Studio contains multiple subheadings, which can all be used to interact with the software in different ways, but there are three that are needed for uploading an AI skill.

The three components that require a POST upload on swagger to add an AI skill to ASG Studio are:

- Provider – This is where you define who is providing the skill – in this case, 4th-ir. This part only needs to be done once for a local instance, so if there is already a provider ID for the machine you are using, you don&#39;t need to do this step.
- Skill – This is where you will define the input and output that is expected for the skill. It is also where you define what you are going to call the skill.
- Engine – This is where you define the endpoint of your rest API, and the type of http request it is supposed to send (i.e POST / GET).

You can use the same provider for all services. Hence, create your provider first, and use the provider ID for all future skills (you will have to add it to the JSON when creating an engine).

Then for every new service, add &quot;skill&quot; first, then &quot;engine&quot; last.

For each step, you will need to add a Tenant ID. As we are working with just one tenant on a local machine, the tenant ID will always be the following: (for providers, skills and engines):
 00000000-0000-0000-0000-000000000001
![](file://C:/Users/jmeel/OneDrive - 4th-IR/100 Management/Docs/Contributing_Readme/images/asgStudio_image1.png)

#### 2. Enabling the port that lets you use the swagger interface

To enable the port 5300, you must first make sure ASG Studio is running on your machine, to make the localhost:5300 available. To do this, open ASG.StudioManager, and click start. To do this, you may have to accept for the app to make changes to your device/ accept some firewall settings, depending on your machine setup.

![image-2.png](attachment:image-2.png)

Once this is up and running, follow this link to the swagger interface:[https://localhost:5300/api/index.html#/](https://localhost:5300/api/index.html#/)

This is where you will upload the services; provider, skill and engine.
 As mentioned, let&#39;s start by adding a provider.

#### 3. Creating a Provider

You can use the same provider for every skill, so you do not have to do this every time you are adding a new one to Zenith. The provider has nothing specific to an AI Service, it simply describes who is providing it (in our case 4th-IR). If you already have a provider ID for your machine for machines at the Innovation Centre, it can be found on the desktop in a text file), feel free to skip this section. If not, you can create a provider by doing the following:

Click on &quot;Providers&quot; then &quot;POST&quot; on the swagger interface. To add anything, you will need to click on &quot;Try it out&quot; in the top right, as marked in the above picture.

![image-3.png](attachment:image-3.png)

Add the tenant ID (00000000-0000-0000-0000-000000000001) and then add the following JSON input for the provider.

    {
      &quot;name&quot;: &quot;4th-IR Provider&quot;,
      &quot;authenticationType&quot;: &quot;QueryHeaders&quot;,
      &quot;authenticationParametersDefinition&quot;: [
        {
          &quot;type&quot;: &quot;Header&quot;,
          &quot;key&quot;: &quot;api-key&quot;
        }
      ]
    }

Collect your providerID from below – you will need this for all engines you create.

![image-4.png](attachment:image-4.png)

**Create a notepad file with this info, so you don&#39;t have to create a provider next time:**

i.e.,

**ProviderID: 7854756e-39d3-4f6d-a338-4e44276d3b29**

If you received the ID, it means it worked; if not, you will have an error message giving you info on how to debug.

#### 4. Creating the Skill

##### 4.1 Overview of the Skill Section

The skill is what describes the expected input and output of the AI service you have created. It also allows you to give the name that will appear on your local ASG Studio when you finish the upload.

The swagger interface is expecting the following values:

    {
    &quot;code&quot;: &quot;string&quot;,
    &quot;name&quot;: &quot;string&quot;,
    &quot;categoryId&quot;: &quot;string&quot;,
    &quot;input&quot;: &quot;string&quot;,
    &quot;output&quot;: &quot;string&quot;
    }
    
A description of each of these values is shown below:
- **_code_**: A unique code that represents this service on your local ASG Studio – can not be the same for multiple services.
- **_name_**: The name that will appear in ASG Studio
- **_categoryId_**: Determines if a skill is Beta release or out of the box – as all of our skills are beta release, this will always be the same number.
- **_Input_**: The expected payload of the service
- **_Output_**: The expected output of the service

As you can see, each part is expecting a string.

##### 4.2 Worked Example of adding a skill

As this part is more complicated, we will go through an example for each of these steps. Let&#39;s take an a translation service that we already have a rest API for. The service has the following expected input and output schemas:

| Example Input: ![image-5.png](attachment:image-5.png) | Example Output: ![image-6.png](attachment:image-6.png)|
| --- | --- |

As can be seen, it is expecting a JSON input, with two strings, and the same schema for the output.

However, when using the swagger interface, it is expecting a stringified JSON Schema for both of these, as opposed to adding JSONs within JSONs. So tTo get these into the correct format, it is necessary to pass them through these two online apps:

1. [easy-json-schema-demo](https://easy-json-schema.github.io/)
2. [JSON Stringify Text - Online Text Tools](https://onlinetexttools.com/json-stringify-text)

The first will convert to JSON schema, the second will stringify this schema, and once it has been passed through both, it is ready to be used

The code is the internal name for the skill – e.g.,  4thirTranslate1– *for this, don&#39;t put spaces*

The name is how it will appear on Zenith – e.g., 4th-ir Translate 1 – *for this, use spaces as it&#39;s the name that will appear on ASG Studio*

The category ID is always - **90f418bb-489b-43d0-baed-5e88ce7924e3**

The input is the stringified JSON of the expected input – as mentioned before, the expected input structure should be passed through the two apps mentioned above, which will give this:

    &quot;{\n \&quot;type\&quot;: \&quot;object\&quot;,\n \&quot;required\&quot;: [],\n \&quot;properties\&quot;: {\n \&quot;text\&quot;: {\n \&quot;type\&quot;: \&quot;string\&quot;\n },\n \&quot;source\_lang\&quot;: {\n \&quot;type\&quot;: \&quot;string\&quot;\n }\n }\n}&quot;

The output is the stringified JSON of the expected output - *as mentioned before, the expected input structure should be passed through the two apps mentioned above, which will give this:*

    &quot;{\n \&quot;type\&quot;: \&quot;object\&quot;,\n \&quot;required\&quot;: [],\n \&quot;properties\&quot;: {\n \&quot;translations\&quot;: {\n \&quot;type\&quot;: \&quot;array\&quot;,\n \&quot;items\&quot;: {\n \&quot;type\&quot;: \&quot;object\&quot;,\n \&quot;required\&quot;: [],\n \&quot;properties\&quot;: {\n \&quot;detected\_source\_language\&quot;: {\n \&quot;type\&quot;: \&quot;string\&quot;\n },\n \&quot;text\&quot;: {\n \&quot;type\&quot;: \&quot;string\&quot;\n }\n }\n }\n }\n }\n}&quot;

Skill

{

    &quot;code&quot;: &quot;4thirTranslate1&quot;,

    &quot;name&quot;: &quot;4th-ir Translate 1&quot;,

    &quot;categoryId&quot;: &quot;90f418bb-489b-43d0-baed-5e88ce7924e3&quot;,

    &quot;input&quot;: &quot;{\n \&quot;type\&quot;: \&quot;object\&quot;,\n \&quot;required\&quot;: [],\n \&quot;properties\&quot;: {\n \&quot;text\&quot;: {\n \&quot;type\&quot;: \&quot;string\&quot;\n },\n \&quot;target\_lang\&quot;: {\n \&quot;type\&quot;: \&quot;string\&quot;\n }\n }\n}&quot;,

    &quot;output&quot;: &quot;{\n \&quot;type\&quot;: \&quot;object\&quot;,\n \&quot;required\&quot;: [],\n \&quot;properties\&quot;: {\n \&quot;translations\&quot;: {\n \&quot;type\&quot;: \&quot;array\&quot;,\n \&quot;items\&quot;: {\n \&quot;type\&quot;: \&quot;object\&quot;,\n \&quot;required\&quot;: [],\n \&quot;properties\&quot;: {\n \&quot;detected\_source\_language\&quot;: {\n \&quot;type\&quot;: \&quot;string\&quot;\n },\n \&quot;text\&quot;: {\n \&quot;type\&quot;: \&quot;string\&quot;\n }\n }\n }\n }\n }\n}&quot;

    }

Save the Skill ID as you will need it later when connecting this part to the engine description.

Now all that is left is to create the engine.

#### 5. Creating the Engine

The engine has the following JSON input:

    {
    &quot;providerId&quot;: &quot;string&quot;,
      &quot;name&quot;: &quot;string&quot;,
      &quot;httpVerb&quot;: &quot;string&quot;,
      &quot;endPoint&quot;: &quot;string&quot;,
      &quot;contentType&quot;: &quot;string&quot;,
      &quot;javascript&quot;: &quot;string&quot;,
      &quot;httpHeaders&quot;: {
        &quot;additionalProp1&quot;: &quot;string&quot;,
        &quot;additionalProp2&quot;: &quot;string&quot;,
        &quot;additionalProp3&quot;: &quot;string&quot;
      },
      &quot;queryParams&quot;: {
        &quot;additionalProp1&quot;: &quot;string&quot;,
        &quot;additionalProp2&quot;: &quot;string&quot;,
        &quot;additionalProp3&quot;: &quot;string&quot;
      },
      &quot;metadataParams&quot;: {
        &quot;additionalProp1&quot;: &quot;string&quot;,
        &quot;additionalProp2&quot;: &quot;string&quot;,
        &quot;additionalProp3&quot;: &quot;string&quot;
      }
    }

Most of this you can immediately delete, and leave:

    {
      &quot;providerId&quot;: &quot;string&quot;,
      &quot;name&quot;: &quot;string&quot;,
      &quot;httpVerb&quot;: &quot;string&quot;,
      &quot;endPoint&quot;: &quot;string&quot;,
      &quot;contentType&quot;: &quot;string&quot;,
      &quot;javascript&quot;: &quot;string&quot;
    }

NB: Remember to delete the trailing comma for the javascript.

You can now edit the parameters that are left as follows:

- *_providerID_* – Either the one you created or the one already on your local machine.
- *_name_* – 4th-IR Engine (or however you want to name it)
- *_httpVerb_* – Probably &quot;Post&quot; but depends on the type of request your API uses
- *_endpoint_* – your enpoint on our k8s cluster
- *_contentType_* – the type of content taken in by your API – in our example &quot;application/json&quot;

For our example this will be the engine input:

    {
      &quot;providerId&quot;: &quot;7854756e-39d3-4f6d-a338-4e44276d3b29&quot;,
      &quot;name&quot;: &quot;4th-IR Engine&quot;,
      &quot;httpVerb&quot;: &quot;Post&quot;,
      &quot;endPoint&quot;: &quot;[https://translt-deepl-api.aks.4th-ir.com/get\_translation](https://translt-deepl-api.aks.4th-ir.com/get_translation)&quot;,
      &quot;contentType&quot;: &quot;application/json&quot;,
      &quot;javascript&quot;: &quot;&quot;
    }

Remember to add the skill ID and the tenant ID.

If this worked, the skill should now be available on your local ASG Studio.

#### 6. Testing your skill on ASG Studio

This tutorial will go through how to test a string- based service, for other types of services more documentation will follow at a later date.

Create a project and then inside your project create a datasource:

![image-7.png](attachment:image-7.png)

Define your datasource on the right hand side. Select Datasource – AI Skill, Name (name it how you want, it will be how it appears in your local environment), Skill – Select the skill that you uploaded (the name will be how you defined it on Swagger), and select it&#39;s engine (the name will be how you defined it on swagger). Click on save to save your datasource.

![image-8.png](attachment:image-8.png)

Create a process model and name it testModel (no spaces or &quot; \_&quot;)

![image-9.png](attachment:image-9.png)

Add a green start button, a blue box, and a red end button. Define the blue box as a service task.

![image-10.png](attachment:image-10.png)

Define the parameters of the blue box – name will add text to the blue box, select the datasource you just defined from the datasource dropdown, select method = execute, add the input json for your rest API and type the name you want to use for your output variable under &quot;response&quot;

![image-11.png](attachment:image-11.png)

Click save and debug in the top left and it will take you to the following screen. Click the play button (marked).

![image-12.png](attachment:image-12.png)

If it worked, you should get your output json on the right with the name saved as you named your variable:

![image-13.png](attachment:image-13.png)

If your skill works, add to the following outputs to your ticket, and mark your ticket as ready to be checked:

The two JSON inputs from Swagger; Skill JSON and Engine JSON
