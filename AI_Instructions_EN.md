Below you will find organizational information and the current environment setup guide for the training.
Please prepare carefully. If you run into technical issues, do not worry.
We will solve all problems together during the training.
You can also contact me earlier by email:
[slawomir.radzyminski@gmail.com](mailto:slawomir.radzyminski@gmail.com)

I recommend working on a personal computer because corporate machines often have security restrictions.
A personal computer with full administrator privileges gives more flexibility when solving problems.

# General Training Approach

In addition to theory, during the training we will work on this project:

- [https://github.com/slawekradzyminski/playwright-2025](https://github.com/slawekradzyminski/playwright-2025)

This is the application environment we will use during the course:

- [https://github.com/slawekradzyminski/awesome-localstack](https://github.com/slawekradzyminski/awesome-localstack)

The default classroom scenario is based on the lightweight Docker profile, so this is the profile you should start and verify before the course.

# Software Installation

1. [Install Git](#git-section)
2. [AI tools for the training](#ai-tools-section)
3. [Install Node.js 24 (LTS)](#node-section)
   - [macOS](#macos)
   - [Windows](#windows)
4. [Download and install the tested app + environment](#environment-section)
5. [Verify the environment](#verification-section)
6. [Download the test repository](#tests-section)

<a id="git-section"></a>
# 1. Install Git

Git is required to synchronize code during the training.

[https://git-scm.com/book/en/v2/Getting-Started-Installing-Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

If you are not comfortable with terminal-based installation, the guide above also includes links to classic installers for Windows and macOS.

- macOS: [https://sourceforge.net/projects/git-osx-installer/](https://sourceforge.net/projects/git-osx-installer/)
- Windows: [https://git-scm.com/download/win](https://git-scm.com/download/win)

<a id="ai-tools-section"></a>
# 2. AI Tools for the Training

You can work with any tool you already use at work.
If you normally use GitHub Copilot, Windsurf, Claude Code, or another assistant, stay with your existing workflow.

If you do not currently have access to any tool, these are all reasonable options:

1. **GitHub Copilot**
   - Quickstart: [https://docs.github.com/en/copilot/get-started/quickstart](https://docs.github.com/en/copilot/get-started/quickstart)
   - If you choose Copilot, I also recommend setting up the CLI:
     [https://docs.github.com/en/copilot/how-tos/copilot-cli/cli-getting-started](https://docs.github.com/en/copilot/how-tos/copilot-cli/cli-getting-started)

2. **Cursor**
   - Product page and pricing: [https://cursor.com/pricing](https://cursor.com/pricing)
   - If you prefer terminal-based work, Cursor CLI is also a good option:
     [https://docs.cursor.com/tools/cli](https://docs.cursor.com/tools/cli)

3. **Windsurf**
   - Editor: [https://windsurf.com/windsurf](https://windsurf.com/windsurf)

4. **Codex (OpenAI, app/IDE/CLI)**
   - Codex: [https://openai.com/codex/](https://openai.com/codex/)
   - Codex app: [https://openai.com/index/introducing-the-codex-app/](https://openai.com/index/introducing-the-codex-app/)
   - Codex CLI: [https://github.com/openai/codex](https://github.com/openai/codex)
   - ChatGPT pricing: [https://openai.com/chatgpt/pricing](https://openai.com/chatgpt/pricing)

5. **Claude Code (Anthropic)**
   - Documentation: [https://docs.anthropic.com/en/docs/claude-code/overview](https://docs.anthropic.com/en/docs/claude-code/overview)
   - Cost management: [https://docs.anthropic.com/en/docs/claude-code/costs](https://docs.anthropic.com/en/docs/claude-code/costs)
   - Pricing: [https://claude.com/pricing](https://claude.com/pricing)

If you do not want to pay for Pro versions, reasonable CLI/open-source alternatives include:
- Kilo Code: [https://kilocode.ai/](https://kilocode.ai/)
- OpenCode: [https://opencode.ai/](https://opencode.ai/)

Important: **inference is not free**.
Even when a tool has a free plan or is open source, there are usually still usage limits and/or model costs.

<a id="node-section"></a>
# 3. Install Node.js 24 (LTS)

Node.js is required for the test code.

I strongly recommend installing `nvm` (Node Version Manager).
It is the most convenient way to manage Node.js versions on your computer.
You can easily switch between versions, for example 20 and 22.

## macOS

[https://tecadmin.net/install-nvm-macos-with-homebrew/](https://tecadmin.net/install-nvm-macos-with-homebrew/)

## Windows

- [https://github.com/coreybutler/nvm-windows](https://github.com/coreybutler/nvm-windows)
- [nvm-setup.exe](https://github.com/coreybutler/nvm-windows/releases/download/1.2.2/nvm-setup.exe) (direct installer link)

If you run into access or command execution issues on Windows with PowerShell, running this command may help:

```powershell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Unrestricted
```

**Verification**

```bash
node --version
```

Expected version:

```bash
v24.11.0
```

<a id="environment-section"></a>
# 4. Download and Install the Tested App + Environment

A Docker-based environment is strongly preferred because you do not need to worry about starting individual applications.
If necessary, we can also install the frontend and backend in an alternative way.

Docker and Docker Compose installation guide:
[https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)

Some companies also use Rancher:
[https://ranchermanager.docs.rancher.com/getting-started/installation-and-upgrade](https://ranchermanager.docs.rancher.com/getting-started/installation-and-upgrade)

```bash
git clone https://github.com/slawekradzyminski/awesome-localstack
```

Inside the downloaded folder, start the lightweight training profile:

```bash
docker compose -f lightweight-docker-compose.yml up -d
```

This starts one public application gateway at:

- `http://localhost:8081`

Behind this gateway you have:

- frontend
- backend API under `/api/v1/...`
- Swagger UI
- product images under `/images/...`
- mock LLM on a separate port `11434`
- Keycloak / SSO on a separate port `8082`

For a more detailed lightweight-profile walkthrough, URLs, and verification steps, use:

- [awesome-localstack/docs/STUDENT_GUIDE.md](https://github.com/slawekradzyminski/awesome-localstack/blob/main/docs/STUDENT_GUIDE.md)

<a id="verification-section"></a>
# 5. Verify the Environment

After startup, run:

```bash
docker compose -f lightweight-docker-compose.yml ps
curl -i http://localhost:8081/login
curl -i http://localhost:8081/v3/api-docs
curl -i http://localhost:8081/images/iphone.png
curl -i http://localhost:8082/realms/awesome-testing/.well-known/openid-configuration
curl -i -X POST http://localhost:11434/api/generate \
  -H 'Content-Type: application/json' \
  -d '{"model":"qwen3.5:2b","prompt":"hello"}'
```

Expected result:

- containers are in the `Up` state
- the login page returns `200`
- OpenAPI returns `200`
- the product image returns `200`
- the Keycloak OpenID Connect configuration returns `200`
- the mock LLM returns `200`

Useful browser URLs:

- app: `http://localhost:8081/login`
- Swagger: `http://localhost:8081/swagger-ui/index.html`
- Keycloak: `http://localhost:8082`

Local credentials:

- standard app login: `client` / `client`
- standard administrator account: `admin` / `LocalDemoAdmin123!`
- SSO through Keycloak: `sso-client` / `SsoClient123!`
- Social login (Google mock): `google-user` / `GoogleUser123!`
- Social login (GitHub mock): `github-user` / `GitHubUser123!`
- Keycloak admin console: `admin` / `admin`

If something is still starting, check logs first:

```bash
docker compose -f lightweight-docker-compose.yml logs -f backend gateway
docker compose -f lightweight-docker-compose.yml logs -f keycloak ollama-mock
```

<a id="tests-section"></a>
# 6. Download the Test Repository

```bash
git clone https://github.com/slawekradzyminski/playwright-2025
```

Run:

```bash
npm install
npx playwright install
npx playwright test
```

![Playwright test results](images/pw_tests.png)

The tests require the environment from section 4 and go through the main application URL:

- `http://localhost:8081`

The current login API endpoint used by the tests is:

- `http://localhost:8081/api/v1/users/signin`

If you have any trouble, feel free to contact me.
