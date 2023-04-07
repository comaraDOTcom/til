 [Unittest] is a python unit testing framework. It can mock functions and methods using decorators. We can call `self.assertTrue`, `self.assertRaises` and other testing methods.

Testing may require mocking objects. You can make assertions about what args were used to call a method, what a method will return or set attributes. The `patch` function can be called as a decorator to patch modules just for the scope of a test.

Here's an example of how I used this today.

### Example using GA client
The [google-api-python-client](https://github.com/googleapis/google-api-python-client) is a client to interact with Google's apis. I've used it to interact with the google analytics API. The service can be called like so
```python
analytics_service = build(
	"analyticsadmin", 
	"v1beta", 
	http=google_auth_httplib2.AuthorizedHttp(
	credentials=google_credentials, http=Http(timeout=1200)
	),
	cache_discovery=False,
)
```
 And the service can be used to list your accounts via, returning a `dict`.
 ```python
 analytics_service.accountSummarise().list().execute()
```
To mock this client for testing we can use `@patch` decorator.
  
 ```python
 from unittest.mock import patch

@patch("<path/to/file/that/calls/build")
def test_google_analytics_client(self, mock_google_analytics_client):
	mock_google_analytics_client.accountSummaries().list().exectue.return_value = {

"accountSummaries": [
	{
		"name": "accountSummaries/1",
		"account": "accounts/1",
		"displayName": "testname.com",
		"propertySummaries": [
				{
				"property": "properties/100",
				"displayName": "testname1 - GA4",
				"propertyType": "type1",
				"parent": "accounts/1",
				},
				{
				"property": "properties/200",
				"displayName": "testname2 - GA4",
				"propertyType": "type2",
				"parent": "accounts/1",
				},
			],
		}
	]
}
```

The `.return_value` is equivalent to `()` calling the method. Here the we are patching the client and also mocking what it would return if called.

There is another function option to mock`.side_effect`  instead of `.return_value`. I currently don't know what that does or when to use either of them. #TODO

