# How We Run Checks

In this section, we cover how checks are performed.

### Test Definition

Each test has a defined frequency, which can be as frequent as every 5 minutes.

### Scheduler

Our scheduler checks every 5 minutes for tests that are scheduled to run, and invokes the test runner.

### Runner

The test runner injects your test code into the pre-configured environment and then runs the test.

The output is recorded and parsed to determine if the test passed or failed.

If the test failed, the result is categorized into one of the following:

* Address mismatch
* Multiple matching HTML elements
* Timeout
* Other
