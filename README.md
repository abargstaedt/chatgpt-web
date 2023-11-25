# ChatGPT Web

> Statement: This project is only released on GitHub, based on the MIT license, free and used as open source learning. And there will not be any form of selling accounts, paid services, discussion groups, discussion groups, etc. Beware of being scammed.

![cover](./docs/c1.png)
![cover2](./docs/c2.png)

- [ChatGPT Web](#chatgpt-web)
	- [Introduction](#INTRODUCTION)
	- [Route to be implemented](#Route to be implemented)
	- [Prerequisites](#Prerequisites)
		- [Node](#node)
		- [PNPM](#pnpm)
		- [Fill in the key](#fill in the key)
	- [Install dependencies](#install dependencies)
		- [Backend](#backend)
		- [Front-end](#front-end)
	- [Run in test environment](#Run in test environment)
		- [Backend Service](#BackendService)
		- [front-end web page](#front-end web page)
	- [Environment variables](#environment variables)
	- [package](#package)
		- [Use Docker](#use-docker)
			- [Docker parameter example](#docker-parameter example)
			- [Docker build \& Run](#docker-build--run)
			- [Docker compose](#docker-compose)
			- [Prevent crawlers from crawling](#Prevent crawlers from crawling)
		- [Use Railway Deployment](#use-railway-deployment)
			- [Railway environment variables](#railway-environment variables)
		- [Deploy using Sealos](#use-sealos-deployment)
		- [Manual packaging](#manual packaging)
			- [Backend Service](#BackendService-1)
			- [front-end web page](#front-end web page-1)
	- [FAQ](#FAQ)
	- [Participate in contribution](#Participate in contribution)
	- [Sponsorship](#Sponsorship)
	- [License](#license)
## introduce

Supports dual models and provides two unofficial `ChatGPT API` methods

| How | Free? | Reliability | Quality |
|------------------------------------------------ | --- --- | ---------- | ---- |
| `ChatGPTAPI(gpt-3.5-turbo-0301)` | No | Reliable | Relatively dumb |
| `ChatGPTUnofficialProxyAPI(web accessToken)` | Yes | Relatively unreliable | Smart |

Compared:
1. `ChatGPTAPI` uses `gpt-3.5-turbo` to call `ChatGPT` through `OpenAI` official `API`
2. `ChatGPTUnofficialProxyAPI` uses an unofficial proxy server to access the backend `API` of `ChatGPT`, bypassing `Cloudflare` (depends on third-party servers and has rate limits)

warn:
1. You should use the `API` method first
2. When using `API`, if the network is not connected, it means that the country is blocked. You need to build your own proxy. Never use other people's public proxy. It is dangerous.
3. When using the `accessToken` method, the reverse proxy will expose your access token to a third party. This should not have any adverse effects, but please consider the risks before using this method.
4. When using `accessToken`, whether you are a domestic or foreign machine, a proxy will be used. The default proxy is [pengzhile](https://github.com/pengzhile) `https://ai.fakeopen.com/api/conversation`. This is not a backdoor or a monitor, unless you have the ability to go through it yourself. `CF` verification, please inform before use. [Community Agent](https://github.com/transitive-bullshit/chatgpt-api#reverse-proxy) (Note: Only these two are recommended, please identify other third-party sources by yourself)
5. When publishing the project to the public network, you should set the `AUTH_SECRET_KEY` variable to add your password access rights, and you should also modify the `title` in `index.html` to prevent it from being searched by keywords.

Switching method:
1. Enter the `service/.env.example` file and copy the content to the `service/.env` file
2. To use `OpenAI API Key` please fill in the `OPENAI_API_KEY` field [(get apiKey)](https://platform.openai.com/overview)
3. To use `Web API` please fill in the `OPENAI_ACCESS_TOKEN` field [(get accessToken)](https://chat.openai.com/api/auth/session)
4. When both exist, `OpenAI API Key` takes precedence.

Environment variables:

Please view all parameter variables or [here](#environment variables)

```
/service/.env.example
```

## Route to be implemented
[✓] Dual model

[✓] Multi-session storage and contextual logic

[✓] Formatting and beautifying processing of message types such as code

[✓] Access permission control

[✓] Data import and export

[✓] Save message to local image

[✓] Multi-language interface

[✓] Interface theme

[✗] More...

## Prerequisites

### Node

`node` requires `^16 || ^18 || ^19` version (`node >= 14` needs to install [fetch polyfill](https://github.com/developit/unfetch#usage-as-a-polyfill )), use [nvm](https://github.com/nvm-sh/nvm) to manage multiple local `node` versions

```shell
node -v
```

###PNPM
If you have not installed `pnpm`
```shell
npm install pnpm -g
```

### Fill in the key
Get `Openai Api Key` or `accessToken` and fill in the local environment variables [Jump](#Introduction)

```
# service/.env file

# OpenAI API Key - https://platform.openai.com/overview
OPENAI_API_KEY=

# change this to an `accessToken` extracted from the ChatGPT site's `https://chat.openai.com/api/auth/session` response
OPENAI_ACCESS_TOKEN=
```

## Install dependencies

> In order to simplify the understanding burden of `back-end developers`, the front-end `workspace` mode is not used, but is stored in folders. If you only need secondary development of the front-end page, delete the `service` folder.

### rear end

Enter the folder `/service` and run the following command

```shell
pnpm install
```

### front end
Run the following command in the root directory
```shell
pnpm bootstrap
```

## Test environment operation
### Backend services

Enter the folder `/service` and run the following command

```shell
pnpm start
```

### Front-end web page
Run the following command in the root directory
```shell
pnpmdev
```

## Environment variables

`API` available:

- Choose one of `OPENAI_API_KEY` and `OPENAI_ACCESS_TOKEN`
- `OPENAI_API_MODEL` Set model, optional, default: `gpt-3.5-turbo`
- `OPENAI_API_BASE_URL` Set the interface address, optional, default: `https://api.openai.com`
- `OPENAI_API_DISABLE_DEBUG` sets the interface to close the debug log, optional, default: empty, does not close

`ACCESS_TOKEN` is available:

- Choose one of `OPENAI_ACCESS_TOKEN` and `OPENAI_API_KEY`. When they exist at the same time, `OPENAI_API_KEY` takes precedence.
- `API_REVERSE_PROXY` Set reverse proxy, optional, default: `https://ai.fakeopen.com/api/conversation`, [Community](https://github.com/transitive-bullshit/chatgpt-api# reverse-proxy) (Note: Only these two are recommended, please identify other third-party sources by yourself)

General:

- `AUTH_SECRET_KEY` access rights key, optional
- `MAX_REQUEST_PER_HOUR` Maximum number of requests per hour, optional, unlimited by default
- `TIMEOUT_MS` timeout in milliseconds, optional
- `SOCKS_PROXY_HOST` and `SOCKS_PROXY_PORT` take effect together, optional
- `SOCKS_PROXY_PORT` and `SOCKS_PROXY_HOST` take effect together, optional
- `HTTPS_PROXY` supports `http`, `https`, `socks5`, optional
- `ALL_PROXY` supports `http`, `https`, `socks5`, optional

## Pack

### Using Docker

#### Docker parameter example

![docker](./docs/docker.png)

#### Docker build & Run

```bash
docker build -t chatgpt-web .

# Run in the foreground
docker run --name chatgpt-web --rm -it -p 127.0.0.1:3002:3002 --env OPENAI_API_KEY=your_api_key chatgpt-web

# Background process
docker run --name chatgpt-web -d -p 127.0.0.1:3002:3002 --env OPENAI_API_KEY=your_api_key chatgpt-web

# Running address
http://localhost:3002/
```

#### Docker compose

[Hub address](https://hub.docker.com/repository/docker/chenzhaoyu94/chatgpt-web/general)

```yml
version: '3'

services:
  app:
    image: chenzhaoyu94/chatgpt-web # Always use latest, and re-pull the tag image when updating.
    ports:
      - 127.0.0.1:3002:3002
    environment:
      # pick one of two
      OPENAI_API_KEY:sk-xxx
      # pick one of two
      OPENAI_ACCESS_TOKEN: xxx
      #API interface address, optional, available when OPENAI_API_KEY is set
      OPENAI_API_BASE_URL: xxx
      # API model, optional, available when OPENAI_API_KEY is set, https://platform.openai.com/docs/models
      # gpt-4, gpt-4-1106-preview, gpt-4-0314, gpt-4-0613, gpt-4-32k, gpt-4-32k-0314, gpt-4-32k-0613, gpt-3.5 -turbo-16k, gpt-3.5-turbo-16k-0613, gpt-3.5-turbo, gpt-3.5-turbo-0301, gpt-3.5-turbo-0613, text-davinci-003, text-davinci-002, code -davinci-002
      OPENAI_API_MODEL: xxx
      # Reverse proxy, optional
      API_REVERSE_PROXY: xxx
      # Access permission key, optional
      AUTH_SECRET_KEY: xxx
      # Maximum number of requests per hour, optional, unlimited by default
      MAX_REQUEST_PER_HOUR: 0
      # Timeout in milliseconds, optional
      TIMEOUT_MS: 60000
      # Socks proxy, optional, effective when combined with SOCKS_PROXY_PORT
      SOCKS_PROXY_HOST: xxx
      # Socks proxy port, optional, effective when combined with SOCKS_PROXY_HOST
      SOCKS_PROXY_PORT: xxx
      # HTTPS proxy, optional, supports http, https, socks5
      HTTPS_PROXY: http://xxx:7890
```
- `OPENAI_API_BASE_URL` optional, available when `OPENAI_API_KEY` is set
- `OPENAI_API_MODEL` optional, available when `OPENAI_API_KEY` is set

#### Prevent crawlers from crawling

**nginx**

Fill in the following configuration into the nginx configuration file. You can refer to the method of adding anti-crawlers in the `docker-compose/nginx/nginx.conf` file.

```
    # Prevent crawlers from crawling
    if ($http_user_agent ~* "360Spider|JikeSpider|Spider|spider|bot|Bot|2345Explorer|curl|wget|webZIP|qihoobot|Baiduspider|Googlebot|Googlebot-Mobile|Googlebot-Image|Mediapartners-Google|Adsbot-Google|Feedfetcher -Google|Yahoo! Slurp|Yahoo! Slurp China|YoudaoBot|Sosospider|Sogou spider|Sogou web spider|MSNBot|ia_archiver|Tomato Bot|NSPlayer|bingbot")
    {
      return 403;
    }
```

### Deploy using Railway

[![Deploy on Railway](https://railway.app/button.svg)](https://railway.app/new/template/yytmgc)

#### Railway environment variables

| Environment variable name | Required | Remarks |
| -------------------------- | -------------------------- | ---- -------------------------------------------------- -------------------------------------------------- |
| `PORT` | Required | Default `3002`
| `AUTH_SECRET_KEY` | Optional | Access key |
| `MAX_REQUEST_PER_HOUR` | Optional | Maximum number of requests per hour, optional, default unlimited |
| `TIMEOUT_MS` | Optional | Timeout in milliseconds |
| `OPENAI_API_KEY` | `OpenAI API` Choose one of two | `apiKey` required to use `OpenAI API` [(Get apiKey)](https://platform.openai.com/overview) |
| `OPENAI_ACCESS_TOKEN` | `Web API` Choose one of two | `accessToken` required to use `Web API` [(get accessToken)](https://chat.openai.com/api/auth/session) |
| `OPENAI_API_BASE_URL` | Optional, available when `OpenAI API` | `API` interface address |
| `OPENAI_API_MODEL` | Optional, available when `OpenAI API` | `API` model |
| `API_REVERSE_PROXY` | Optional, available when using `Web API` | `Web API` reverse proxy address [Details](https://github.com/transitive-bullshit/chatgpt-api#reverse-proxy) |
| `SOCKS_PROXY_HOST` | Optional, effective when combined with `SOCKS_PROXY_PORT` | Socks proxy |
| `SOCKS_PROXY_PORT` | Optional, effective when combined with `SOCKS_PROXY_HOST` | Socks proxy port |
| `SOCKS_PROXY_USERNAME` | Optional, effective when combined with `SOCKS_PROXY_HOST` | Socks proxy username |
| `SOCKS_PROXY_PASSWORD` | Optional, effective when combined with `SOCKS_PROXY_HOST` | Socks proxy password |
| `HTTPS_PROXY` | Optional | HTTPS proxy, supports http, https, socks5 |
| `ALL_PROXY` | Optional | All proxies, support http, https, socks5 |

> Note: Modifying environment variables of `Railway` will re-`Deploy`

### Deploy using Sealos

[![](https://raw.githubusercontent.com/labring-actions/templates/main/Deploy-on-Sealos.svg)](https://cloud.sealos.io/?openapp=system-fastdeploy% 3FtemplateName%3Dchatgpt-web)

>Environment variables are consistent with Docker environment variables

### Manual packaging
#### Backend services
> If you don’t need the `node` interface of this project, you can omit the following operations

Copy the `service` folder to the server where you have the `node` service environment.

```shell
# Install
pnpm install

# Pack
pnpm build

# run
pnpm prod
```

PS: Without packaging, you can also run `pnpm start` directly on the server.

#### Front-end web page

1. Modify the `VITE_GLOB_API_URL` in the `.env` file in the root directory to your actual backend interface address

2. Run the following command in the root directory, and then copy the files in the `dist` folder to the root directory of your website service

[Reference information](https://cn.vitejs.dev/guide/static-deploy.html#building-the-app)

```shell
pnpm build
```

## common problem
Q: Why does `Git` always report an error when submitting?

A: Because there is submission information verification, please follow the [Commit Guide](./CONTRIBUTING.md)

Q: If I only use the front-end page, where do I change the request interface?

A: The `VITE_GLOB_API_URL` field in the `.env` file in the root directory.

Q: Will all the files explode when saved?

A: `vscode` Please install the project recommended plugin, or manually install the `Eslint` plugin.

Q: There is no typewriter effect on the front end?

A: One possible reason is that after Nginx reverse proxy and the buffer is turned on, Nginx will try to buffer a certain size of data from the backend before sending it to the browser. Please try adding `proxy_buffering off;` after the anti-parameter, and then reload Nginx. The configuration of other web servers is the same.

## Participate and contribute

Please read the [Contribution Guidelines](./CONTRIBUTING.md) before contributing.

Thanks to everyone who contributed!

<a href="https://github.com/Chanzhaoyu/chatgpt-web/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=Chanzhaoyu/chatgpt-web" />
</a>

## Sponsorship

If you think this project is helpful to you, and if the situation allows, you can give me a little support. Anyway, thank you very much for your support ~

<div style="display: flex; gap: 20px;">
	<div style="text-align: center">
		<img style="max-width: 100%" src="./docs/wechat.png" alt="WeChat" />
		<p>WeChat Pay</p>
	</div>
	<div style="text-align: center">
		<img style="max-width: 100%" src="./docs/alipay.png" alt="Alipay" />
		<p>Alipay</p>
	</div>
</div>

## License
MIT © [ChenZhaoYu](./license)
