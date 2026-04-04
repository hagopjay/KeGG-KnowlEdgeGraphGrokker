https://googleapis.github.io/python-genai/

Google Gen AI SDK
pypi

https://github.com/googleapis/python-genai

google-genai is an initial Python client library for interacting with Google’s Generative AI APIs.

Google Gen AI Python SDK provides an interface for developers to integrate Google’s generative models into their Python applications. It supports the Gemini Developer API and Vertex AI APIs.

Installation
pip install google-genai
Imports
from google import genai
from google.genai import types
Create a client
Please run one of the following code blocks to create a client for different services (Gemini Developer API or Vertex AI). Feel free to switch the client and run all the examples to see how it behaves under different APIs.

from google import genai

# Only run this block for Gemini Developer API
client = genai.Client(api_key='GEMINI_API_KEY')
from google import genai

# Only run this block for Vertex AI API
client = genai.Client(
    vertexai=True, project='your-project-id', location='us-central1'
)
(Optional) Using environment variables:

You can create a client by configuring the necessary environment variables. Configuration setup instructions depends on whether you’re using the Gemini Developer API or the Gemini API in Vertex AI.

Gemini Developer API: Set GOOGLE_API_KEY as shown below:

export GOOGLE_API_KEY='your-api-key'
Gemini API in Vertex AI: Set GOOGLE_GENAI_USE_VERTEXAI, GOOGLE_CLOUD_PROJECT and GOOGLE_CLOUD_LOCATION, as shown below:

export GOOGLE_GENAI_USE_VERTEXAI=true
export GOOGLE_CLOUD_PROJECT='your-project-id'
export GOOGLE_CLOUD_LOCATION='us-central1'
from google import genai

client = genai.Client()
API Selection
By default, the SDK uses the beta API endpoints provided by Google to support preview features in the APIs. The stable API endpoints can be selected by setting the API version to v1.

To set the API version use http_options. For example, to set the API version to v1 for Vertex AI:

from google import genai
from google.genai import types

client = genai.Client(
    vertexai=True,
    project='your-project-id',
    location='us-central1',
    http_options=types.HttpOptions(api_version='v1')
)
To set the API version to v1alpha for the Gemini Developer API:

from google import genai
from google.genai import types

# Only run this block for Gemini Developer API
client = genai.Client(
    api_key='GEMINI_API_KEY',
    http_options=types.HttpOptions(api_version='v1alpha')
)
Faster async client option: Aiohttp
By default we use httpx for both sync and async client implementations. In order to have faster performance, you may install google-genai[aiohttp]. In Gen AI SDK we configure trust_env=True to match with the default behavior of httpx. Additional args of aiohttp.ClientSession.request() (see _RequestOptions args) can be passed through the following way:

http_options = types.HttpOptions(
    async_client_args={'cookies': ..., 'ssl': ...},
)

client=Client(..., http_options=http_options)
Proxy
Both httpx and aiohttp libraries use urllib.request.getproxies from environment variables. Before client initialization, you may set proxy (and optional SSL_CERT_FILE) by setting the environment variables:

export HTTPS_PROXY='http://username:password@proxy_uri:port'
export SSL_CERT_FILE='client.pem'
If you need socks5 proxy, httpx supports socks5 proxy if you pass it via args to httpx.Client(). You may install httpx[socks] to use it. Then you can pass it through the following way:

http_options = types.HttpOptions(
    client_args={'proxy': 'socks5://user:pass@host:port'},
    async_client_args={'proxy': 'socks5://user:pass@host:port'},
)

client=Client(..., http_options=http_options)
Types
Parameter types can be specified as either dictionaries(TypedDict) or Pydantic Models. Pydantic model types are available in the types module.

Models
The client.models modules exposes model inferencing and model getters. See the ‘Create a client’ section above to initialize a client.

Generate Content
with text content
response = client.models.generate_content(
    model='gemini-2.0-flash-001', contents='Why is the sky blue?'
)
print(response.text)
with uploaded file (Gemini Developer API only)
download the file in console.

!wget -q https://storage.googleapis.com/generativeai-downloads/data/a11.txt
python code.

file = client.files.upload(file='a11.txt')
response = client.models.generate_content(
    model='gemini-2.0-flash-001',
    contents=['Could you summarize this file?', file]
)
print(response.text)
How to structure contents argument for generate_content
The SDK always converts the inputs to the contents argument into list[types.Content]. The following shows some common ways to provide your inputs.

Provide a list[types.Content]



