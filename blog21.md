# Tenable IO API 

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

