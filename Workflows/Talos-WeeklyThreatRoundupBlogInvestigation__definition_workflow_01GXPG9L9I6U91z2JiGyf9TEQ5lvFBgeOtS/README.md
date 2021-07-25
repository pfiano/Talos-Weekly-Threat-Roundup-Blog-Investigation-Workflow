
# Talos Weekly Threat Roundup Blog Investigation Workflow

This workflow will automate inside the SecureX Orchestrator the investigation based on the IOCs published weekly by Talos Threat Roundup blog to understand whether there has been an impact of one of those threats in your environment.


## Use Cases

- Simplify threat hunting: Give the ability to security analyst to investigate most prevalent threats that Talos has observed over the current week and converts it into a SecureX casebook

- SOC task automation: This workflow can be scheduled to periodically run at Talos Blog publish date so that investigation could be carried out immediatly upon blog availability


## Description

Every Friday evening, US time, Talos is publishing a glimpse into the most prevalent threats they have observed over the current week.

For each threat described in the weekly roundup blog, an accompanying JSON file can be retrieved making an https request to Talos Roundup hosted IOCs that includes the complete list of file hashes, as well as all other IOCs from the post. The https request will be directed to the following resource:

https://s3.amazonaws.com/talos-intelligence-site/production/document_files/files/000/095/<URL_ID>/original/YYYYMMDD-tru.json.txt

where DD stands for the calendar day of the publication (remember, always on Fridays!). For instance, the JSON file containing all the IOCs related to the most prevalent threats they have observed between Jul. 16th and Nov. 23rd will have the following link:

https://s3.amazonaws.com/talos-intelligence-site/production/document_files/files/000/095/597/original/20210723-tru.json.txt

where the <URL_ID> assigned by TALOS to this pubblication is 597.

The <URL_ID> will increment each week. The workflow will automatically increase the <URL_ID> and check if the URL is valid. Once the valid <URL_ID> is found out, this is recorded in the SecureX Orchestrator global variable **Talos_URL_ID**, so that it will remembered the week after for the next run starting from the stored Talos_URL_ID value.

## Installation Steps

1. Go to your instance of SecureX Orchestration and click on IMPORT

![image](https://user-images.githubusercontent.com/67795055/123630969-79d7d480-d816-11eb-9fb1-5de04cd86ae7.png)


2. We have two options here, through a GitHub repository or browse/copy-paste the JSON of the workflow

**_Option through GitHub repository_ (please configure the Git Repository with the Cisco Code Exchange -> Explore -> Repository where the workflow is published)**

![image](https://user-images.githubusercontent.com/67795055/123631411-04b8cf00-d817-11eb-8149-8659ad9ad160.png)

**_Option Paste JSON or upload the workflow to import_**

![image](https://user-images.githubusercontent.com/67795055/123651803-15277480-d82c-11eb-81d3-3b65878f18c5.png)

3. When starting the import the following warning will be displayed. This is due to the fact that the workflow needs configuration updates. Please click UPDATE

![image](https://user-images.githubusercontent.com/67795055/123652905-fbd2f800-d82c-11eb-9664-90a0603ad67a.png)

4. Provide SecureX Threat Response API Credentials

![image](https://user-images.githubusercontent.com/67795055/123654822-ba434c80-d82e-11eb-9a1b-609a97adfaf4.png)

5. Provide email credentials for the SMTP Endpoint

![image](https://user-images.githubusercontent.com/67795055/123656722-76514700-d830-11eb-9cca-e3c5063a0984.png)

6. Provide the Webex Teams Bot Token

![image](https://user-images.githubusercontent.com/67795055/123657175-dc3dce80-d830-11eb-971c-e7512252334e.png)

7. Provide again the Webex Teams Bot Token when asked

![image](https://user-images.githubusercontent.com/67795055/123657175-dc3dce80-d830-11eb-971c-e7512252334e.png)

8. The workflow at this point should be succefully imported

![image](https://user-images.githubusercontent.com/67795055/123658821-5ae73b80-d832-11eb-8a61-7d4e930aeb51.png)

9. Update the SMTP settings accordingly with the smtp server and port information in the SMTP Endpoint

![image](https://user-images.githubusercontent.com/67795055/123675079-9db10f80-d842-11eb-9c31-60dbe65680e4.png)

10. Verify that the following Endpoints have been created under Targets

![image](https://user-images.githubusercontent.com/67795055/123659801-5a02d980-d833-11eb-9892-5f973da0ce1e.png)

11. Verify the following credentials have been created under Account Keys

![image](https://user-images.githubusercontent.com/67795055/123660105-a77f4680-d833-11eb-9644-2d89fe9c3947.png)

12. Open the workflow, the following message will be displayed. Click OK

![image](https://user-images.githubusercontent.com/67795055/123660323-d990a880-d833-11eb-9ca7-a33119346144.png)

13. In the Canvas, click on warnings

![image](https://user-images.githubusercontent.com/67795055/123660687-2b393300-d834-11eb-8f7f-e74b1f86db15.png)

14. Select SxTR Check Deliberate Verdict

![image](https://user-images.githubusercontent.com/67795055/123660865-4f950f80-d834-11eb-9b64-3b3f43f57b39.png)

15. Go to Properties of the SxTR Check Deliberate Verdict and select _Override Workflow Target_, from the list menu select **ADD NEW**

![image](https://user-images.githubusercontent.com/67795055/123661780-1e690f00-d835-11eb-81f6-63114c0c33a7.png)

16. Create now a new HTTP Target Endpoint with the following configuration (leave the other fields empty, and select the right SecureX Cloud visibility.amp.cisco.com for NAM, visibility.eu.amp.cisco.com for EU):

![image](https://user-images.githubusercontent.com/67795055/123662415-bb2bac80-d835-11eb-93b7-049627df73e2.png)

![image](https://user-images.githubusercontent.com/67795055/123662278-9800fd00-d835-11eb-8dea-44151a3dc69a.png)

17. After saving, you should see the following, with the warning counter down to 2. Now select _SxTR Create Casebook_

![image](https://user-images.githubusercontent.com/67795055/123663058-515fd280-d836-11eb-9eec-6d3548f3f339.png)

18. Select ADD NEW

![image](https://user-images.githubusercontent.com/67795055/123663183-72282800-d836-11eb-88d6-ceed57c71a28.png)

19. Create now a new HTTP Target Endpoint with the following configuration (leave the other fields empty, and select the right SecureX Cloud private.intel.amp.cisco.com for NAM, private.intel.eu.amp.cisco.com for EU):

![image](https://user-images.githubusercontent.com/67795055/123663419-aac80180-d836-11eb-9155-33774038b79b.png)

![image](https://user-images.githubusercontent.com/67795055/123663900-2033d200-d837-11eb-9c30-5342931dab50.png)

20. After saving, you should see the following, with the counter down to 1. Now select _Webex Teams - Post Message to Room_

![image](https://user-images.githubusercontent.com/67795055/123664017-42c5eb00-d837-11eb-82d1-237695bd4527.png)

21. Select **Webex Teams** from _Override Workflow Target_

![image](https://user-images.githubusercontent.com/67795055/123664417-9d5f4700-d837-11eb-9962-5c72e1e8b52c.png)

22. Now should looks like this, with no warnings active (counter down to 0). Validate the workflow.

![image](https://user-images.githubusercontent.com/67795055/123664899-1199ea80-d838-11eb-919b-d90b50217670.png)

23. Verify that that teh Global Variable **Talos_URL_ID** been created. Please see the Description section of this README file to understand the role of the Talos_URL_ID global variable.

![image](https://user-images.githubusercontent.com/67795055/123665319-76eddb80-d838-11eb-823e-30c82e11cb41.png)

24. Now it is time for testing the workflow. Enter the workflow and click on RUN, the following screen for input variable pops up

![image](https://user-images.githubusercontent.com/67795055/123666126-393d8280-d839-11eb-9a33-6228f0f61644.png)

Please note that these values have (required) defaults in the variables defined in the workflow properties. You can change the defaults there.

- **max_number_retry_iterations** is the max number of retries the workflow does in case the IOCs have not been published for the week. Between each retry, the workflow will sleep for _retry_interval_. When the max_number_retry_iterations is reached, the workflow send a notification and quits. The workflow will then be activated at the next schedule trigger.

- **retry_interval** is the sleep time between each retry (in seconds)

- **webex_bot_token**, you can specify a different Webex Teams Bot token in case you would not like to use the configured one

- **email_recipients**, comma separated, in order to send notifications

- **talos_blog_uri**, please insert manually here the URI of the IOCs of a specific Talos Roundup

- **webex_room_name**, to specify the name of the Webex Teams room. Please note that if a room does not exists with the provided name, then it will be created.

25. Let's do first a manual run by providing manullay the URI of the IOCs published in a roundup. For instance, we go to the Talos Roundup of June 17th 2021 at https://blog.talosintelligence.com/2021/06/threat-roundup-0611-0617.html, then click on **_here_** where the IOCs are published and copy the URL https://s3.amazonaws.com/talos-intelligence-site/production/document_files/files/000/095/584/original/20210617-tru.json.txt?1623962447, and paste in the workflow input variable **talos_blog_uri** just the Relative URL, in this case **/talos-intelligence-site/production/document_files/files/000/095/584/original/20210617-tru.json.txt?1623962447**

![image](https://user-images.githubusercontent.com/67795055/123669153-152f7080-d83c-11eb-8dc9-ad00c7fdbe80.png)

![image](https://user-images.githubusercontent.com/67795055/123669514-73f4ea00-d83c-11eb-8722-b941f4f3f965.png)

![image](https://user-images.githubusercontent.com/67795055/123673916-4cece700-d841-11eb-9e95-432d34d4ee7b.png)

Then click RUN.

26. In case of automatic or scheduled run, the workflow will automatically determine the URI for the latest Talos Roundup. 

## Talos_URL_ID

if you wish to run again the workflow against a past Talos Roundup blog, first modify the global variable **Talos_URL_ID** and detract 50 to the current value. For instance, if the current value for the  Global Variable **Talos_URL_ID is 550**, then change it and 500:

![image](https://user-images.githubusercontent.com/67795055/123676556-62174500-d844-11eb-9070-950a3ce94b67.png)

Then change it to 500

![image](https://user-images.githubusercontent.com/67795055/123676651-82df9a80-d844-11eb-90d6-3b0e2e9a110b.png)


## Playbook Steps

![image](https://user-images.githubusercontent.com/67795055/123629583-bacee980-d814-11eb-95b3-756ff3d5ed38.png)

1. Through a Schedule trigger, we program the workflow to start every Friday evening (schedule trigger description is outside of this README file, just follow the Schudule capability that SecureX Orchestrator offers in order to do so). The option also exists to enter manually a Blog URI in case we wish to run the workflow against a past published blog.

2. Once the workflow starts, first step will be to build the URI of the blog for the current week, then do an https request to retrieve the JSON file describing the threats and IOCs.

3. In case the URI resource (JSON file) is not published yet, a retry mechanism is implemented in the workflow to check every certain period of time (configurable) whether the Json file has become available.

4. Once the JSON file becomes available, an https request is carried out to retrieve the IOCs, some clean up is done and the threats and their associated description and IOCs are stored in a table.

5. A Communication is sent via email and webex team message to inform that the automatic investigation has started, together with a summary of the threats identified and their respective description.

6. Then for each Threat, IOCs are extracted from the JSON file and parsed.

7. The verdict of the IOCs is checked and those with Clean Verdict are omitted from the Investigation in order to reduce False positives.

8. Then the actual investigation is launched through the enrichment process based on the final list of IOCs - denoting the current analyzed threat, against the customer integrated Security solutions and Intelligence Sources (Public and Private)

9. For each Security module that has answered (and contributed) during the enrichment process the returned information is checked to determine whether the customer environment has been impacted - For this we run a loop through each Security module that replied back during the enrichment process, and for each target reported, we post on webex teams and send an email providing preliminary information such as Target information, Security Module that provided the information, sightings.

10. Once we have processed all Security modules and if we do have targets in the customer environment related to the current investigated threat, a new casebook is then created with all the IOCs related to the threat, and the Casebook link and ID is shared through posting in webex teams and via email.

11. In case targets have not been found, an information message is sent out in any case.


## Required Targets

1. CTR_For_Access_Token (default)
2. CTR_API (default)
3. Private_CTIA
4. SMTP
5. Webex Teams


## Required Account Keys

1. CTR_Credentials
2. Webex Teams Token
3. Email Credentials


## Required Atomic Workflows

1. CTRGenerateAccessToken
2. Webex Teams - Post Message to Room
3. Webex Teams - Create Room
4. Webex Teams - Search for Room
5. CTR Inspect Observables
6. CTR CheckDeliberateVerdict
7. CTR Enrich Observable
8. CTR Create Casebook


## Video

https://www.youtube.com/watch?v=5ZchL_qsRPc

## Commnents and Suggestions

Please send your comments and suggestions to Nicola (pfiano@cisco.com)
