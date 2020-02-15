# Blog 15 [2.14.2020]

## Taking steps to prevent secret leaks in your git

"Secrets" is a hard word to define. 
The data is extremely variable and unstructured.
For example:

AWEFOIJ!:35HF1JFPIO

DFMWUFE1231,M2MVO123;LKJASDF

ThePorh!m2PorgAHf'2

f;oij600100310031

Are all technically valid API Keys if the framework was built to accept this.

It could also be a database credential, or some poorly encrypted filepath.

Regardless of whatever it is, it's not something you should be pushing. 

Admittedly, sometimes you misconfigure your .gitignore, pushing files that weren't to be pushed in the first place.
## The solution...kind of

![image](https://user-images.githubusercontent.com/20525440/74581869-7f0de980-4f69-11ea-9214-dd5078060763.png)

Github does have built in token-scanning, but only for a limited number of secret types, tokens associated with the following services
```
    Alibaba Cloud
    Amazon Web Services (AWS)
    Atlassian
    Azure
    CloudBees CodeShip
    Discord
    Dropbox
    GitHub
    GoCardless
    Google Cloud
    Hashicorp Terraform
    Mailgun
    npm
    Postman
    Proctorio
    Pulumi
    Slack
    Stripe
    Tencent Cloud
    Twilio
```

## So what else?

![image](https://user-images.githubusercontent.com/20525440/74582530-fe072000-4f71-11ea-8937-217962dd4a34.png)

The purpose of this blog was to shine a bit of light on a tool I found to help with this.

Nightfall is an NLP and machine-learning based tool for finding secrets in github.
While it currently boasts 200 different types of secrets it can detect, I do not belive I have access to download a comprehensive list of what those secret types are.

The webGUI shows responses as well, but the awesome part is being able to run it via the API.

```
curl https://radar.nightfall.ai/api/v1/scans/new \
-u API_KEY: \
-d 'github_url=https://github.com/nightfalldlp/sample'
```


```
{
  "status": "Success",
  "limit": 5,
  "page": 1,
  "scans": [
    {
      "id": "2ed12efe-1fbd-4d09-aad9-1e6867c1c80f",
      "url": "https://github.com/nightfalldlp/sample",
      "duration": "1.706554794",
      "created_at": "2019-04-23T23:39:58.282Z"
    },
    {
      "id": "f5edec8b-a215-4537-90db-8d02289e11e4",
      "url": "https://github.com/nightfalldlp/sample",
      "duration": "1.670032915",
      "created_at": "2019-04-23T23:38:14.560Z"
    }
  ]
}
                    
```
Set this as some arbitrary cronjob for a list of URLs and you've got yourself automated secrets scanning.

Next week:
https://github.com/sobolevn/git-secret
Hide the secrets from ever being committed in the first place.
