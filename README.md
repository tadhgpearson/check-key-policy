Check API Key Policy
====================

Annoyingly, Apigee Edge doesn't offer an API function to lookup the details of an Edge user, given their API key. See [this question on their community site](https://community.apigee.com/questions/34735/how-to-find-an-developer-and-their-app-with-the-ed.html). Turns out that creating this functionality is not that easy to do.
 
This is a simple policy for Apigee Edge that does just this lookup for you.

What does this do?
==================
This creates a proxy called `check-key` that will be deployed at `/check-key` on your Edge environment.

You invoke the proxy by calling 

> https://youredgeenvironment.apigee.net/check-key?apikey=API_KEY_TO_CHECK

It will return a HTTP 200 status with a JSON message like this if the key is valid
    
    { 
      "key-status" : "approved",
      "app-name" : "Tadhg-Test-App1",
      "developer-name" : "Tadhg",
      "developer-email" : "fakeemail@hotmail.com"
    }

You can edit this message in the `apikey-valid.xml` policy.

If the key is not valid, you will get the error like this

    { 
      "status" : 401,
      "message" : "Invalid or missing API key",
      "more_info" : "A valid API key is required to access this resource. Please ensure your request includes a query parameter called apikey, whose value is a valid API key from your account"
    }

You can edit the message in the `invalid-apikey.xml` policy

How do I deploy this?
=====================
1. Clone this repo, and zip the whole apiproxy folder.
2. In Apigee Edge admin UI, go to API Proxies -> Add new API proxy -> Proxy Bundle
3. After clicking next, import your zip file from step 1, and name the policy check-key
4. Add this policy to a product. Probably the best thing to do is choose an Internal or Private product, and share it with other administrators only.
5. Enjoy!
