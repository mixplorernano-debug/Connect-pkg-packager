# python -m venv venv
source venv/bin/activate

What is this repository for?
%!VENV

### Install

First, create a new directory and set up a virtual environment:

```sh
python -m venv venv
source venv/bin/activate
```


#This will create and activate a virtual environment named .venv

Then, install the required packages:

```sh
pip install fastapi uvicorn upstash-workflow
```

### Get QStash token

Go to [Upstash Console](https://console.upstash.com/qstash) and copy the `QSTASH_TOKEN`, set it in the `.env` file.

```sh
export QSTASH_TOKEN=
```

### Define a Workflow Endpoint

To declare workflow endpoints, use the `@serve.post` decorator. Save the following code to `main.py`:

```python
from fastapi import FastAPI
from upstash_workflow.fastapi import Serve
from upstash_workflow import AsyncWorkflowContext

app = FastAPI()
serve = Serve(app)

# mock function
def some_work(input: str) -> str:
    return f"processed '{input}'"

# serve endpoint which expects a string payload:
@serve.post("/example")
async def example(context: AsyncWorkflowContext[str]) -> None:
    # get request body:
    input = context.request_payload

    async def _step1() -> str:
        output = some_work(input)
        print("step 1 input", input, "output", output)
        return output

    # run the first step:
    result: str = await context.run("step1", _step1)

    async def _step2() -> None:
        output = some_work(result)
        print("step 2 input", result, "output", output)

    # run the second step:
    await context.run("step2", _step2)
```

In the example, you can see that steps are declared through the `context` object.

The kinds of steps which are available are:

* `context.run`: execute a function
* `context.sleep`: sleep for some time
* `context.sleep_until`: sleep until some timestamp
* `context.call`: make a third party call without consuming any runtime

You can [learn more about these methods from our documentation](https://upstash.com/docs/workflow/basics/context).

### Run the Server

Upstash Workflow needs a public URL to orchestrate the workflow. Check out our [Local Development](https://upstash.com/docs/workflow/howto/local-development) guide to learn how to set up a local tunnel.

Create the tunnel and set the `UPSTASH_WORKFLOW_URL` environment variable in the `.env` file with the public URL:

```sh
ngrok http localhost:8000
```

```sh
export UPSTASH_WORKFLOW_URL=
```

Then, set the environment variables:

```sh
source .env
```

Finally, run the server:

```sh
uvicorn main:app --reload
```

FastAPI server will be running at `localhost:8000`.

## Contributing

### Development

1. Clone the repository
2. Install [Poetry](https://python-poetry.org/docs/#installation)
3. Install dependencies with `poetry install`
4. Create a .env file with `cp .env.example .env` and fill in the environment variables
5. Run tests with `poetry run pytest`
6. Format with `poetry run ruff format .`
7. Check with `poetry run ruff check .`
8. Type check with `poetry run mypy --show-error-codes .`
```
git clone https://pip.wizard@bitbucket.org/code-bucket-workspace/bitbucket-upload-file-pkg-packager.git
```
1 Create Python app First, we’ll create a new directory for our Python app. We’ll call it clean-db-cron.The database we’ll be using is Redis, so we’ll need to install the upstash_redis package. Copy Ask AI
```
mkdir clean-db-cron

Copy
Ask AI

cd clean-db-cron

Copy
Ask AI

pip install upstash-redis
```
2 Cleanup logic Let’s write the Python code to clean up the database. We’ll use the upstash_redis package to connect to the database and delete all keys. index.py Copy Ask AI

from upstash_redis import Redis

redis = Redis(url="https://YOUR_REDIS_URL", token="YOUR_TOKEN")

def delete_all_entries(): keys = redis.keys("*") # Match all keys redis.delete(*keys)

delete_all_entries()

Try running the code to see if it works. Your database keys should be deleted! 3 Make the Python code into a public endpoint In order to use QStash, we need to make the Python code into a public endpoint. There are many ways to do this such as using Flask, FastAPI, or Django. In this example, we’ll use the Python http.server module to create a simple HTTP server. api/index.py Copy Ask AI

from http.server import BaseHTTPRequestHandler from upstash_redis import Redis

redis = Redis(url="https://YOUR_REDIS_URL", token="YOUR_TOKEN")

def delete_all_entries(): keys = redis.keys("*") # Match all keys redis.delete(*keys)

class handler(BaseHTTPRequestHandler): def do_POST(self): delete_all_entries() self.send_response(200) self.end_headers()

For the purpose of this tutorial, I’ll deploy the application to Vercel using the Python Runtime, but feel free to use any other hosting provider.

Deploying to Vercel There are many ways to deploy to Vercel, but I’m going to use the Vercel CLI. Copy Ask AI
```
npm install -g vercel

Copy
Ask AI

run build 

vercel
```
Once deployed, you can find the public URL in the dashboard. 4 Have QStash invoke the endpoint There are two ways we can go about configuring QStash. We can either use the QStash dashboard or the QStash API. In this example, it makes more sense to utilize the dashboard since we only need to set up a singular cronjob.However, you can imagine a scenario where you have a large number of cronjobs and you’d want to automate the process. In that case, you’d want to use the QStash Python SDK.To create the schedule, go to the QStash dashboard and enter the URL of the public endpoint you created. Then, set the type to schedule and change the Upstash-Cron header to run daily at a time of your choosing. Copy Ask AI

URL: https://your-vercel-app.vercel.app/api Type: Schedule Every: every day at midnight (feel free to customize)

QStash console scheduling Once you start the schedule, QStash will invoke the endpoint at the specified time. You can scroll down and verify the job has been created!

Using the SDK

Using the SDK

Now, go ahead and try it out for yourself! Try using some of the other features of QStash, such as callbacks and URL Groups.

Was this page helpful? Suggest edits Raise issue Next.js Cloudflare Workers

### Contribution guidelines ###

* Writing tests
* Code review
* Other guidelines

### Who do I talk to? ###

* Repo owner or admin
* Other community or team contact
* 


