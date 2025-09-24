# Connect-pkg-packager
Connect-pkg-packager

What is this repository for?

    Quick summary
    Version
    Learn Markdown

How do I get set up?

    Summary of set up
    Configuration
    Dependencies
    Database configuration
    How to run tests
    Deployment instructions
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


