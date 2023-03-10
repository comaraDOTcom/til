
## Using fixtures to create a test scenario
Pytest uses fixtures to initialize test functions in a fixed scenario. This ensures they can be reliably ran to produce consitent results to properly test the code. [Pytest documentation on fixtures](https://docs.pytest.org/en/6.2.x/fixture.html).

#### Example
To test a part of a function called in a production environment. The `monkeypatch` fixture temporarily modifies classes and environment variables. Passing the `monkeypatch` fixture to a test function lets us edit the environment variables to match what they would be in production.

For example
```python
def build_model_config():
	if "DEVELOPMENT" not in os.environ:
		output = <do something>
	else:
		output = <do something else>
	return {"output": output }
```
Suppose we've a test that is run in development where `DEVELOPMENT:true` is an environment variable. We also want to write a test to test the output in the production scenario (when `DEVELOPMENT` is not an environment variable).
```python
def test__build_model_config_in_production_env(monkeypatch):
	monkeypatch.delenv('DEVELOPMENT') #delete the DEVELOPMENT env variable in the context of this text.
	< instantiate classes and make assertions>
```


### MagicMock


### Overriding methods
Useful example blog post:https://www.learnaws.org/2020/12/01/test-aws-code/
For the S3 example.
	We mock an S3 client using moto (using the mock_s3 decorator.)
In the S3 class.
	We create S3 bucket client in the class on instantiation. And have methods to list buckets and objects.
In the test itself thre is a fixture for creating a bucket called `s3_test` passed to the class which creates the bucket.
This fixture is then pased to the test function.


## Coverage on app.py



## Run configuration
1. Create S3 client
2. Create S3 bucket
	1. bucket name
3. Define a run_id and org_id fixture
4. Put object in bucket in `"runs/{organisation_id}/{run_id}/metadata.json"`
	1. Get an example of this.

## Integration tests
Check how functoins are working together.


### Testing the wrong functions
Basically wrote unit tests when i really wanted to test integration tests.


`os.getenv('DEVELOPMENT')`