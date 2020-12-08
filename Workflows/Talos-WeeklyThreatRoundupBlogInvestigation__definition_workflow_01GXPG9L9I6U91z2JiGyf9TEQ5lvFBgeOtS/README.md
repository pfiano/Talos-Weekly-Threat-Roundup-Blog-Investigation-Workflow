Every Friday evening, US time, Talos is publishing a glimpse into the most prevalent threats they have observed over the current week.

For each threat described in the weekly roundup blog, an accompanying JSON file can be retrieved making an https request to Talos Roundup hosted IOCs that includes the complete list of file hashes, as well as all other IOCs from the post. The https request will be directed to the following resource:

https://storage.googleapis.com/blogs-images/ciscoblogs/1/YYYY/MM/YYYYMMDD-tru.json_.txt 

where DD stands for the calendar day of the publication (remember, always on Fridays!). For instance, the JSON file containing all the IOCs related to the most prevalent threats they have observed between Nov. 6th and Nov. 13th will have the following link:

https://storage.googleapis.com/blogs-images/ciscoblogs/1/2020/11/20201113-tru.json_.txt 
 
The playbook steps:

1.	Build the HTTPS URI to retrieve the JSON file, describing the threats and IOCs of the weekly threat roundup, using the URI format that Talos employ for the publication of this resource.

2.	In case the URI resource (JSON file) is not published yet, a retry mechanism is implemented to check every hour whether has become available.

3.	Once the JSON file becomes available, an http request is carried out to retrieve TALOS Roundup hosted IOCs, do some clean up and store in a table the indicators with their description.

4.	Communicate via email and webex team message that the Talos Roundup threat automatic investigation has started, together with a summary of the threats identified and their respective description.

5.	Extract IOCs – for each Threat, extract the IOCs from JSON and store into a variable

6.	Parse IOCs - from raw text in the variable using SecureX Threat Response Inspect API 

7.	Enrich Observables - with SecureX Threat Response Enrich API to find any global sightings (in integrated feeds from threat intel sources such as AMP Global Intelligence, AMP File Reputation, Talos Intelligence, Umbrella - INVESTIGATE) and local sightings/targets (in integrated security modules like Umbrella, AMP, ESA, etc.). 

8.	A table is created with an entry for each module that has answered (and contributed) to the enrichment process and the information (json) has replied back.

9.	Determine Domains with Clean Verdict or Judgement. There may be the case that some Domain Names contacted by malware are not malicious. Therefore, a list of potential clean domains in the IOCs is created, with the objective to omit investigation with those observables.

10.	Find Targets - For each module that replied back, and for each target found and only for the first sighting, post on webex teams and/or send an email.

11.	Case Management – Once a threat has been worked out, in case targets are found a new casebook is created with all the IOCs related to the threat, and the Casebook link and ID is shared through posting in webex teams and/or sending an email. 

12.	In case targets have not been found, an informational message is sent out. 



# sxo-workflows
# sxo-workflows
