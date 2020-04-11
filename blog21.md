# Tenable IO API -- Policy automation

![image](https://user-images.githubusercontent.com/20525440/79036352-6a6e5c00-7b7c-11ea-8445-66fd8bb0cf1b.png)

TenableIO is a cloud-based interface for Nessus vulnerability scanners. Using agents, you can take vulnerability scan information and prioritize vulnerability findings.

It's also known for some very awesome API documentation and usage, which you can use to automate aforementioned scans.

## Policies 

![image](https://user-images.githubusercontent.com/20525440/79036340-51fe4180-7b7c-11ea-874c-75589a7a247b.png)

Policies are templates of things to look for in a scan. This can be a network scan, compliance audit, specture/meldown detection, etc. 

You typically pick and edit a policy edit.

![image](https://user-images.githubusercontent.com/20525440/79037015-87a62900-7b82-11ea-8cd4-726b5b7a301e.png)
  
  ```
import requests

url = "https://cloud.tenable.com/policies"

payload = "{\"uuid\":\"example uuid\"}"
headers = {
    'accept': "application/json",
    'content-type': "application/json"
    }

response = requests.request("POST", url, data=payload, headers=headers)

print(response.text)
  ```

We can emulate the policy creation by taking the UUID of the policy template we want to create and send a POST request to the API endpoint.
```
import setPortScanRange
import requests
import os
import sys
import json
ACCESS = os.environ['TENABLE_ACC']
SECRET = os.environ['TENABLE_SEC']

url = "https://cloud.tenable.com/policies/%s" % sys.argv[1]
headers = {'accept': 'application/json','x-apikeys': "accessKey=" + ACCESS + ";secretKey=" + SECRET}
response = requests.request("GET", url, headers=headers)
detailsJSON=json.loads(response.text)
print(detailsJSON['settings']['portscan_range'])
detailsJSON['settings']['portscan_range'] = "2,7,9999"
print(detailsJSON['settings']['portscan_range'])
payload = json.dumps(detailsJSON)
#print(detailsJSON)

url = "https://cloud.tenable.com/policies/219"
headers = {
    'accept': "application/json",
    'x-apikeys': "accessKey=" + ACCESS + ";secretKey=" + SECRET
    }

print(payload)
response = requests.request("PUT", url, headers=headers,data=payload)
print(response.text)
```
We can then grab use a policy object in order to update this in the future.

![image](https://user-images.githubusercontent.com/20525440/79037167-9c36f100-7b83-11ea-98df-f9fd1ba96e80.png)

To explain, ACCESS and SECRET are API keys specific to your teable license. You muist generate them, and in this usage we call them from an environment variable.

We then use a format string to grab a policy number from the user of the script.
e.g. python3 polictest.py 400 would set our URL to policy ID 400

In the headers , we specify that we want JSON data from that URL, using our access and secret keys set before.

We then use this to make a GET request to that API endpoint, and load that into a json object called "detailsJSON" . The contents of this object are populated by information from the response from the API endpoint.

The response is typically a JSON object containing an array we can use to parse information.

An example is shown here where detailsJSON[settings][portscan_range] is actually a 2d array, where portscan range is a list inside of the "settings" array.

At this point, we print out the portscan range from the "settings" array in the policy that we are looking at.

From there, we can use json.dumps to properly print the response information (important for debugging)

We take the JSON object we have and update the "portscanrange" list to include ports 2,7, and 9999, and print this out to make sure the format is still aligned.

Using a "put" request, we can then make an API call with the new JSON object to update our current portscan range for a given policy.

By setting this as a cronjob reading from a file, you can automate whcih ports your nessus scanners are scanning for on advanced network sfcans.
