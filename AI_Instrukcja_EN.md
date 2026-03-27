Below you will find the organizational notes and the current setup guide for the training.
Please prepare carefully. If you run into technical issues, do not worry.
We will solve them together during the training.
You can also contact me earlier by email:
[slawomir.radzyminski@gmail.com](mailto:slawomir.radzyminski@gmail.com)

I recommend using a private computer, because corporate machines often have security restrictions.
A private computer with full administrator rights gives much more flexibility when solving setup issues.

# General Training Approach

During the training, in addition to theory, we will work on this test project:

- [https://github.com/slawekradzyminski/playwright-2025](https://github.com/slawekradzyminski/playwright-2025)

This is the application environment used during the course:

- [https://github.com/slawekradzyminski/awesome-localstack](https://github.com/slawekradzyminski/awesome-localstack)

The default classroom workflow is based on the lightweight Docker profile, so this is the profile you should start and verify before the course.

# Software Installation

1. [Install Git](#section-git)
2. [AI tools for the training](#section-ai-tools)
3. [Install Node.js 24 (LTS)](#section-node)
   - [macOS](#section-macos)
   - [Windows](#section-windows)
4. [Clone and start the application environment](#section-environment)
5. [Verify the environment](#section-verification)
6. [Clone the Playwright tests](#section-tests)

<a id="section-git"></a>
# 1. Install Git

Git is needed to synchronize code during the training.

[https://git-scm.com/book/en/v2/Getting-Started-Installing-Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

If you do not feel comfortable installing from the terminal, the guide above also contains links to classic installers for Windows and macOS.

- macOS: [https://sourceforge.net/projects/git-osx-installer/](https://sourceforge.net/projects/git-osx-installer/)
- Windows: [https://git-scm.com/download/win](https://git-scm.com/download/win)

<a id="section-ai-tools"></a>
# 2. AI Tools For The Training

You can use any tool you already use at work.
If you normally work with GitHub Copilot, Windsurf, Claude Code, or another assistant, stay with your existing workflow.

If you do not already have access to a tool, these are all reasonable options:

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

If you do not want to pay for Pro plans, reasonable CLI/open-source alternatives include:
- Kilo Code: [https://kilocode.ai/](https://kilocode.ai/)
- OpenCode: [https://opencode.ai/](https://opencode.ai/)

Important: inference is not free.
Even if a tool offers a free plan or is open source, you will usually still hit usage limits and/or model costs.

<a id="section-node"></a>
# 3. Install Node.js 24 (LTS)

Node.js is needed for the Playwright tests.

I strongly recommend installing `nvm` (Node Version Manager).
It is the most convenient way to manage Node.js versions on your machine.

<a id="section-macos"></a>
## macOS

[https://tecadmin.net/install-nvm-macos-with-homebrew/](https://tecadmin.net/install-nvm-macos-with-homebrew/)

<a id="section-windows"></a>
## Windows

- [https://github.com/coreybutler/nvm-windows](https://github.com/coreybutler/nvm-windows)
- [nvm-setup.exe](https://github.com/coreybutler/nvm-windows/releases/download/1.2.2/nvm-setup.exe)

**Verification**

```bash
node --version
```

Expected version:

```bash
v24.11.0
```

<a id="section-environment"></a>
# 4. Clone And Start The Application Environment

The Docker-based environment is strongly preferred because you do not need to run frontend and backend separately.

Docker and Docker Compose installation:
[https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)

Some companies also use Rancher:
[https://ranchermanager.docs.rancher.com/getting-started/installation-and-upgrade](https://ranchermanager.docs.rancher.com/getting-started/installation-and-upgrade)

```bash
git clone https://github.com/slawekradzyminski/awesome-localstack
cd awesome-localstack
docker compose -f lightweight-docker-compose.yml up -d
```

This starts the lightweight training profile with one main public URL:

- `http://localhost:8081`

Behind that gateway you get:

- frontend
- backend API under `/api/v1/...`
- Swagger UI
- product images under `/images/...`
- mock LLM on separate port `11434`

For a more detailed lightweight-profile walkthrough, URLs, and verification steps, use:

- [awesome-localstack/STUDENT_GUIDE.md](https://github.com/slawekradzyminski/awesome-localstack/blob/main/STUDENT_GUIDE.md)

<a id="section-verification"></a>
# 5. Verify The Environment

Run:

```bash
docker compose -f lightweight-docker-compose.yml ps
curl -i http://localhost:8081/login
curl -i http://localhost:8081/v3/api-docs
curl -i http://localhost:8081/images/iphone.png
curl -i -X POST http://localhost:11434/api/generate \
  -H 'Content-Type: application/json' \
  -d '{"model":"qwen3:0.6b","prompt":"hello"}'
```

Expected result:

- containers are `Up`
- login page returns `200`
- OpenAPI returns `200`
- product image returns `200`
- mock LLM returns `200`

Useful browser URLs:

- app: `http://localhost:8081/login`
- Swagger: `http://localhost:8081/swagger-ui/index.html`

If something is still starting, check logs first:

```bash
docker compose -f lightweight-docker-compose.yml logs -f backend gateway
docker compose -f lightweight-docker-compose.yml logs -f ollama-mock
```

<a id="section-tests"></a>
# 6. Clone The Playwright Tests

```bash
git clone https://github.com/slawekradzyminski/playwright-2025
cd playwright-2025
npm install
npx playwright install
npx playwright test
```

The tests require the environment from section 4 and target the main app URL:

- `http://localhost:8081`

The current login API endpoint used by the tests is:

- `http://localhost:8081/api/v1/users/signin`

If you have any trouble, feel free to contact me before the course.
