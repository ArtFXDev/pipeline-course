# Memo Deadline 

## Manuals 

* [Documentation Home](https://docs.thinkboxsoftware.com/products/deadline/10.1/1_User%20Manual/index.html)
* [Manual Job Submission documentation](https://docs.thinkboxsoftware.com/products/deadline/10.1/1_User%20Manual/manual/manual-submission.html)

## Scripting

### Deadline Scripting Reference

The "Deadline Scripting API" is used inside Deadline's features, for various tasks, including: 
- renderer plugins 
- event plugins
- job scripts
- submitter scripts
- etc.

Documentation

* [Scripting Overview](https://docs.thinkboxsoftware.com/products/deadline/10.1/1_User%20Manual/manual/scripting-overview.html)
* [Scripting API documentation](https://docs.thinkboxsoftware.com/products/deadline/10.2/2_Scripting%20Reference/index.html)

Example (from the RezVray plugin):

```python
def SetupRez(self):
        
        self.rezExecutable = self.GetPluginInfoEntryWithDefault("RezExecutable", None) or self.GetRenderExecutable("RezExecutable", "rez")
        self.rezRequires = self.GetPluginInfoEntryWithDefault("RezRequires", None) or self.GetConfigEntryWithDefault("RezRequires", "vray")
        rezCommand = self.GetPluginInfoEntryWithDefault("RezCommand", None) or self.GetConfigEntryWithDefault("RezCommand", "vray")
     
        self.rezArgument = 'env {} -- {}'.format(self.rezRequires, rezCommand)
        self.LogInfo("[SetupRez] {} {}".format(self.rezExecutable, self.rezArgument))
```


### Python Standalone API

The "Python Standalone API" is a wrapper around the REST API.

* [Standalone API documentation](https://docs.thinkboxsoftware.com/products/deadline/10.2/3_Python%20Reference/index.html)

Example submitting a job

```python
from Deadline.DeadlineConnect import DeadlineCon
dl = DeadlineCon(DEADLINE_HOST, DEADLINE_PORT)
dl.Jobs.SubmitJob(job.job_info, job.plugin_info)
```

Example quering jobs

```python
workers = dl.Slaves.GetSlaveNamesInPool("test")

for worker in sorted(workers):
    infos = dl.Slaves.GetSlaveInfo(worker)
    print(f"{worker} -> {infos.get('User')} {infos.get('Stat')} {infos.get('StatDate')}")
```

## Rest

* [Rest API](https://docs.thinkboxsoftware.com/products/deadline/10.1/1_User%20Manual/manual/rest-overview.html)  
* [Realpython : consuming a Rest API](https://realpython.com/api-integration-in-python/#rest-and-python-consuming-apis)

Note: the REST API is wrapped by the "Python Standalone API" (see above)

### REST used in python

Examples on using the REST API in python.

Async quering of groups:

````python
import logging
import traceback
import aiohttp
logger = logging.getLogger("deadline")

class DeadlineRunner:

    @staticmethod
    async def query_repos(query_url: str):
        try:
            async with aiohttp.ClientSession() as session:
                async with session.get(query_url) as response:
                    query = await response.json()

                    return query

        except Exception as e:
            logger.error(
                "Could not connect to Deadline WebService and query Groups and Pool information: "
                + traceback.format_exc()
            )
            raise Exception(str(e))

    @staticmethod
    async def get_groups():
        groups = await DeadlineRunner.query_repos(
            f"http://{DEADLINE_HOST}:{DEADLINE_PORT}/api/groups"
        )

        return groups
````

Setting a job to Pending state using PUT
```python
import requests
url = "http://deadline:8081/api/jobs"
data = {
    "Command": "pend",
    "JobID": "63f7879702e8f442e1348d07",
}
response = requests.put(url, json=data)
print(response)
```


## CLI

* [CLI](https://docs.thinkboxsoftware.com/products/deadline/10.1/1_User%20Manual/manual/command.html)

Can be used on a machine where DL is installed. Example:  
```shell

@echo off
for /f "delims=" %%i in ('"C:\Program Files\Thinkbox\Deadline10\bin\deadlinecommand.exe" -GetSlaveNamesInGroup classrooms') do ( echo %%i >> \\...\deadline\artfxTools\workerManagement\output.txt )
"C:\Program Files\Thinkbox\Deadline10\bin\deadlinecommand.exe" -ExecuteScript \\...\artfxTools\workerManagement\stopWorkers.py
echo.>\\...\deadline\artfxTools\workerManagement\output.txt
pause

```

## Web Service

For the REST API and the Python Standalone API, the "web service" needs to run.

To start the service locally (deadline needs to be installed), open powershell (keep powershell during runtime) and type: 
```shell
cd "C:\Program Files\Thinkbox\Deadline10\bin"
./deadlinewebservice.exe
```

Test the python API : 
```python
import sys
api_path = r'\\deadline\DeadlineRepository\api\python'
sys.path.append(api_path)
from Deadline.DeadlineConnect import DeadlineCon
deadline = DeadlineCon('localhost', 8081)
for u in deadline.Users.GetUserNames():
    print(u)
```
