import Image from '@theme/IdealImage';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# OpenAI Proxy Server

A simple, fast, and lightweight **OpenAI-compatible server** to call 100+ LLM APIs in the OpenAI Input/Output format

## Endpoints:
- `/chat/completions` - chat completions endpoint to call 100+ LLMs
- `/models` - available models on server

[![Deploy](https://deploy.cloud.run/button.svg)](https://l.linklyhq.com/l/1uHtX)

:::info
We want to learn how we can make the proxy better! Meet the [founders](https://calendly.com/d/4mp-gd3-k5k/berriai-1-1-onboarding-litellm-hosted-version) or
join our [discord](https://discord.gg/wuPM9dRgDw)
::: 


## Usage 

```shell 
$ git clone https://github.com/BerriAI/litellm.git
```
```shell
$ cd ./litellm/openai-proxy
```

```shell
$ uvicorn main:app --host 0.0.0.0 --port 8000
```

## Auth - LLM API keys
- This server allows you to store LLM API keys in the environment variables. Example `OPENAI_API_KEY`, `AZURE_API_KEY`. More Info [required variables for each provider](https://docs.litellm.ai/docs/providers)
- Pass auth params `api_key`,`api_base`, `api_version` etc to the `/chat/completions` endpoint

## Replace openai base
```python 
import openai 
openai.api_base = "http://0.0.0.0:8000" # proxy url
openai.api_key = "does-not-matter"
# call cohere
response = openai.ChatCompletion.create(
    model="command-nightly", 
    messages=[{"role":"user", "content":"Hey!"}],
    api_key="your-cohere-api-key", # enter your key here
)

# call bedrock 
response = openai.ChatCompletion.create(
    model = "bedrock/anthropic.claude-instant-v1",
    messages = [
        {
            "role": "user",
            "content": "Hey!"
        }
    ],
    aws_access_key_id="",
    aws_secret_access_key="",
    aws_region_name="us-west-2",
)

print(response)
``` 

:::info
Looking for the CLI tool/local proxy? It's [here](./proxy_server.md)
::: 

## Deploy on Google Cloud Run
**Click the button** to deploy to Google Cloud Run

[![Deploy](https://deploy.cloud.run/button.svg)](https://l.linklyhq.com/l/1uHtX)

On a successfull deploy your Cloud Run Shell will have this output
<Image img={require('../img/cloud_run0.png')} />

## Testing your deployed proxy
**Assuming the required keys are set as Environment Variables**

https://litellm-7yjrj3ha2q-uc.a.run.app is our example proxy, substitute it with your deployed cloud run app

<Tabs>
<TabItem value="openai" label="OpenAI">

```shell
curl https://litellm-7yjrj3ha2q-uc.a.run.app/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
     "model": "gpt-3.5-turbo",
     "messages": [{"role": "user", "content": "Say this is a test!"}],
     "temperature": 0.7
   }'
```

</TabItem>
<TabItem value="azure" label="Azure">

```shell
curl https://litellm-7yjrj3ha2q-uc.a.run.app/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
     "model": "azure/<your-deployment-name>",
     "messages": [{"role": "user", "content": "Say this is a test!"}],
     "temperature": 0.7
   }'
```

</TabItem>

<TabItem value="anthropic" label="Anthropic">

```shell
curl https://litellm-7yjrj3ha2q-uc.a.run.app/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
     "model": "claude-2",
     "messages": [{"role": "user", "content": "Say this is a test!"}],
     "temperature": 0.7,
   }'
```
</TabItem>

</Tabs>

### Set LLM API Keys
#### Environment Variables 
More info [here](https://cloud.google.com/run/docs/configuring/services/environment-variables#console)

1. In the Google Cloud console, go to Cloud Run: [Go to Cloud Run](https://console.cloud.google.com/run)

2. Click on the **litellm** service
<Image img={require('../img/cloud_run1.png')} />

3. Click **Edit and Deploy New Revision**
<Image img={require('../img/cloud_run2.png')} />

4. Enter your Environment Variables
Example `OPENAI_API_KEY`, `ANTHROPIC_API_KEY`
<Image img={require('../img/cloud_run3.png')} />



