## Universal Analytics -> GA4
For UA the authorization process was done using the [management api](https://developers.google.com/analytics/devguides/config/mgmt/v3). This API does not support GA4 properties. We've to use [Google Analytics Admin API V1](https://developers.google.com/analytics/devguides/config/admin/v1/rest) for managing GA4 properties.

In the [getting started](https://github.com/googleapis/google-api-python-client/blob/main/docs/start.md) of the [google-api-python-client repo](https://github.com/googleapis/google-api-python-client) there is a list of supported APIs. There's support for [analyticsadmin](https://github.com/googleapis/google-api-python-client/blob/main/docs/dyn/index.md) api, version `v1beta`.

## Authorisation
We need authorisation from the google cloud account like to the google analytics account in order to pull from the management api. The Oauth process in Google analytics links a GA account to a Google cloud project. The GA creds can be used to authentica the GC Project to access the GA API. Once we have this step complete, we can use other APIs in Google Cloud to access the GA API. 

Examples:
1. Google Analytics Reporting API (UA reports)
2. Google Analytics Admin API V1 (management details of the GA4 account)


### Simple example to check it works.
Using the pdb debugger. Populate with your GA token creds from the OAuth process. This example list all the GA4 `property_ids`  managed by these GA credentials.

```python

import pdb  
from google.oauth2.credentials import Credentials  
from googleapiclient.discovery import build  
import google_auth_httplib2  
from httplib2 import Http  
import pdb  
  
google_credentials = Credentials(  
    token="<>",  
    refresh_token="<>",  
    client_id="<>",  
    client_secret="<>",  
    token_uri="<>",  
    scopes=["https://www.googleapis.com/auth/analytics.readonly"],  
)  
    
analytics_service = build(  
    "analyticsadmin",  
    "v1beta",  
    http=google_auth_httplib2.AuthorizedHttp(  
        credentials=google_credentials, http=Http(timeout=1200)  # 1200 seconds request timeout  
    ),  
    cache_discovery=False,  
)  
data = []  
  
pdb.set_trace()  
accounts =  analytics_service.properties().list().execute()
```