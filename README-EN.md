[简体中文](README.md) · **English**

## User Communication

[Telegram Channel](https://sum4all.one/telegram)

## Buy me a coffee

<a href="https://www.buymeacoffee.com/fatwang2" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>

# Version Updates
- V0.2.6, 20240425, support the searxng search service, support the moonshot API in stream mode
- V0.2.5, 20240425, open source the code for the search api
- V0.2.4, 20240424, support for Groq in Cloudflare Worker
- V0.2.3, 20240423, support for Azure OpenAI in Cloudflare Worker. It also introduces the ability to use an authorization code and customize the user's request key.
- V0.2.2, 20240420, support Moonshot API on unstream mode
- V0.2.1, 20240310, supports Google, Bing, Duckduckgo, Search1API for news-type searches; supports adjusting the number of search results via the MAX_RESULTS environment variable; supports adjusting the number of in-depth searches desired via the CRAWL_RESULTS environment variable.
- V0.2.0，20240310，Optimized openai.js, cloudflare worker version, really faster this time!

For more historical updates, please see [Version History](https://github.com/fatwang2/search2ai/releases)

# S2A

Help your LLM API support networking, search, news, web page summarization, has supported OpenAI, Gemini, Moonshot, the big model will be based on your input to determine whether the network, not every time the network search, do not need to install any plug-ins, do not need to replace the key, directly in your commonly used OpenAI/Gemini three-way client replacement of custom You can directly replace the customized address in your usual OpenAI/Gemini three-way client, and also support self-deployment, which will not affect the use of other functions, such as drawing, voice, etc.

<table>
    <tr>
        <td><img src="pictures/Opencatnews.png" alt="效果示例"></td>
        <td><img src="pictures/BotGem.png" alt="效果示例"></td>
    </tr>
    <tr>
        <td><img src="pictures/Lobehub.png" alt="效果示例"></td>
        <td><img src="pictures/url.png" alt="效果示例"></td>
    </tr>
</table>

# Features

| Model            | Features              | Stream           | Deployments                                         |
| ---------------- | --------------------- | ---------------- | --------------------------------------------------- |
| `OpenAI`       | search, news, crawler | stream, unstream | Zeabur, Local deployment, Cloudflare Worker, Vercel |
| `Azure OpenAI` | search, news, crawler | stream, unstream | Cloudflare Worker                                   |
| `Groq`         | search, news, crawler | stream, unstream | Cloudflare Worker                                   |
| `Gemini`       | search                | stream, unstream | Cloudflare Worker                                   |
| `Moonshot`     | search, news, crawler | stream(only on cf), unstream         | Zeabur, Local deployment, Cloudflare Worker(stream), Vercel |

# Usage

**Replace the custom domain in any client with the following address**

<table>
    <tr>
        <td><img src="pictures/NextChat.png" alt="Effect Example"></td>
    </tr>
</table>

# Deployment

**Zeabur**

Click the button for one-click deployment, switched on your own environment variables

[![Deploy on Zeabur](https://zeabur.com/button.svg)](https://zeabur.com/templates/A4HGYF?referralCode=fatwang2)

To keep the project updated, it is recommended to fork this repository first, then deploy your branch through Zeabur

[![Deployed on Zeabur](https://zeabur.com/deployed-on-zeabur-dark.svg)](https://zeabur.com?referralCode=fatwang2&utm_source=fatwang2&utm_campaign=oss)

**Local Deployment**

1. Clone the repository locally

```
git clone https://github.com/fatwang2/search2ai
```

2. Copy .env.template as .env, configure environment variables
3. Enter the api directory, run the program, and display the log in real-time

```
cd api && nohup node index.js > output.log 2>&1 & tail -f output.log
```

4. Port 3014, the complete address after concatenation is as follows, can be configured according to the client's requirements for the apibase address (if https is required, need to use nginx for reverse proxy, many tutorials online)

```
http://localhost:3014/v1/chat/completions
```

**Cloudflare worker**

1. Copy the code of [search2openai.js](search2openai.js), or [search2gemini.js](search2gemini.js), or [search2groq.js](search2groq.js), no modifications needed! Deploy in cloudflare's worker, after going online, the worker's address can be used as your interface call's custom domain address, note the concatenation, worker address only represents the part before v1
2. Configure variables in the worker（only openai）
   ![Effect Example](pictures/worker.png)
3. Configure triggers - custom domain in the worker, direct access to the worker's address in China might have issues, need to replace with custom domain
   ![Alt text](pictures/域名.png)

**Vercel**

Special note: Vercel project does not support streaming output and has a 10s response limit, actual user experience is poor, released mainly for experts to pull request

One-click deployment

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Ffatwang2%2Fsearch2ai&env=SEARCH_SERVICE&envDescription=%E6%9A%82%E6%97%B6%E6%94%AF%E6%8C%81google%E3%80%81bing%E3%80%81serpapi%E3%80%81serper%E3%80%81duckduckgo%EF%BC%8C%E5%BF%85%E5%A1%AB)

To ensure updates, you can also first fork this project and then deploy it on Vercel yourself

# Environment Variables

This project provides some additional configuration options, which can be set through environment variables:

| Environment Variable | Required    | Description                                                                                                                                                                 | Example                                                                          |
| -------------------- | ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| `SEARCH_SERVICE`   | Yes         | Your search service. The key of the service you choose needs to be configured.                              | `search1api, google, bing, serpapi, serper, duckduckgo, searxng`                        |
| `APIBASE`          | No          | Third-party proxy address.                                                                                                                                                  | `https://api.openai.com, https://api.moonshot.cn, https://api.groq.com/openai` |
| `MAX_RESULTS`      | No          | Number of search results.                                                                                                                                                   | `10`                                                                           |
| `CRAWL_RESULTS`    | No          | The number of deep searches (retrieve the main text of the webpage after searching). Currently only supports search1api, deep search will be slow.                          | `1`                                                                            |
| `SEARCH1API_KEY`   | Conditional | Required if search1api is selected. My own fast and cheap search service. Apply at https://search21api.com.                                                                 | `xxx`                                                                          |
| `BING_KEY`         | Conditional | Required if Bing search is selected. Please search for the tutorial yourself. Apply at https://search2ai.online/bing.                                                       | `xxx`                                                                          |
| `GOOGLE_CX`        | Conditional | Required if Google search is selected. Search engine ID. Please search for the tutorial yourself. Apply at https://search2ai.online/googlecx.                               | `xxx`                                                                          |
| `GOOGLE_KEY`       | Conditional | Required if Google search is selected. API key. Apply at https://search2ai.online/googlekey.                                                                                | `xxx`                                                                          |
| `SERPAPI_KEY`      | Conditional | Required if serpapi is selected. Free 100 times/month. Register at https://search2ai.online/serpapi.                                                                        | `xxx`                                                                          |
| `SERPER_KEY`       | Conditional | Required if serper is selected. Free 2500 times for 6 months. Register at https://search2ai.online/serper.                                                                  | `xxx`                                                                          |
| `SEARXNG_BASE_URL` | Conditional | Required if searxng is selected. Fill in the domain name of the self-built searXNG service, refer to this repo https://github.com/searxng/searxng, plz open the json mode | `https://search.xxx.xxx`                                                                          |
| `OPENAI_TYPE`      | No          | OpenAI provider source, default is openai                                                                                                                                   | `openai, azure`                                                                |
| `RESOURCE_NAME`    | Conditional | Required if azure is selected                                                                                                                                               | `xxxx`                                                                         |
| `DEPLOY_NAME`      | Conditional | Required if azure is selected                                                                                                                                               | `gpt-35-turbo`                                                                 |
| `API_VERSION`      | Conditional | Required if azure is selected                                                                                                                                               | `2024-02-15-preview`                                                           |
| `AZURE_API_KEY`    | Conditional | Required if azure is selected                                                                                                                                               | `xxxx`                                                                         |
| `AUTH_KEYS`        | No          | If you want users to define a separate authorization code as a key when making requests, you need to fill this in. Required if azure is selected                            | `000,1111,2222`                                                                |
| `OPENAI_API_KEY`   | No          | If you want users to define a separate authorization code as a key when requesting openai, you need to fill this in                                                         | `sk-xxx`                                                                       |

# Future Iterations

- Fix streaming output issues in Vercel project
- Improve the speed of streaming output
- Support more vertical searches
