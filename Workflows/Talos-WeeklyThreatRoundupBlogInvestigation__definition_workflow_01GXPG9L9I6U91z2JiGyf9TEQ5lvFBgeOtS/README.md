Every Friday evening, US time, Talos is publishing a glimpse into the most prevalent threats they have observed over the current week.

For each threat described in the weekly roundup blog, an accompanying JSON file can be retrieved making an https request to Talos Roundup hosted IOCs that includes the complete list of file hashes, as well as all other IOCs from the post. The https request will be directed to the following resource:

https://s3.amazonaws.com/talos-intelligence-site/production/document_files/files/000/095/<URL_ID>/original/YYYYMMDD-tru.json.txt

where DD stands for the calendar day of the publication (remember, always on Fridays!). For instance, the JSON file containing all the IOCs related to the most prevalent threats they have observed between Nov. 6th and Nov. 13th will have the following link:

https://s3.amazonaws.com/talos-intelligence-site/production/document_files/files/000/095/<URL_ID>/original/20201113-tru.json.txt

The <URL_ID> will increment each week. The workflow will increase the <URL_ID> and check if the URL is valid. Once the valid <URL_ID> is found out, this is recorded in the global variable Talos_URL_ID, so that it will remembered the week after for the next run starting from the stored Talos_URL_ID.

The playbook steps:

1. Through a Schedule trigger, we program the workflow to start every Friday evening. The option also exists to enter manually a Blog URI in case we wish to run the workflow against a past published blog.

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


Required Targets

1. CTR_For_Access_Token (default)
2. CTR_API (default)
3. Private_CTIA
4. SMTP
5. Webex Teams


Required Account Keys

1. CTR_Credentials
2. Webex Teams Token
3. Email Credentials


Required Atomic Workflows

1. CTRGenerateAccessToken
2. Webex Teams - Post Message to Room
3. Webex Teams - Create Room
4. Webex Teams - Search for Room
5. CTR Inspect Observables
6. CTR CheckDeliberateVerdict
7. CTR Enrich Observable
8. CTR Create Casebook
