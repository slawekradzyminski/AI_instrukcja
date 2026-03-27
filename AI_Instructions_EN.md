Below you will find organizational information and installation instructions for the required software.
Please prepare diligently. If you run into technical difficulties, don’t worry.
We will solve all issues together during the training.
You can also contact me in advance by email:
[slawomir.radzyminski@gmail.com](mailto:slawomir.radzyminski@gmail.com)

I recommend working on a personal computer because corporate machines often have security restrictions.
A personal computer with full administrator privileges gives more flexibility when solving problems.

# General Training Approach

In addition to theory, during the training we will work on this project:

- [https://github.com/slawekradzyminski/playwright-2025](https://github.com/slawekradzyminski/playwright-2025)

Here is the full environment that simulates a real work situation:

- [https://github.com/slawekradzyminski/awesome-localstack](https://github.com/slawekradzyminski/awesome-localstack)

Below you will find instructions on how to verify that everything works.

# Software Installation

1. [Install Git](#git-section)
2. [AI tools for training](#ai-tools-section)
3. [Install Node.js 24 (LTS)](#node-section)
   - [macOS](#macos)
   - [Windows](#windows)
4. [Download and install the tested app + environment](#environment-section)
5. [Download the test repository](#tests-section)

<a id="git-section"></a>
# 1. Install Git

Required to sync code during the training.

[https://git-scm.com/book/en/v2/Getting-Started-Installing-Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

If you are not comfortable with terminal-based installation, the guide above also includes links to classic installers for Windows and Mac.

- Mac: [https://sourceforge.net/projects/git-osx-installer/](https://sourceforge.net/projects/git-osx-installer/)
- Windows: [https://git-scm.com/download/win](https://git-scm.com/download/win)

<a id="ai-tools-section"></a>
# 2. AI Tools for Training

You can work with any tool you already use at work.
If you normally use GitHub Copilot, Windsurf, Claude Code, or another assistant, keep your existing workflow.

If you do not have access to any tool, I recommend the following _paid_ tools:

1. **Cursor (IDE)**
   - My recommendation for most people: `Cursor Pro` for about `$20/month`.
   - Pricing: [https://cursor.com/pricing](https://cursor.com/pricing)

2. **Codex (OpenAI, app/IDE/CLI)**
   - If you prefer terminal work (`CLI`), this is a very good option.
   - In practice, the simplest path is often via a ChatGPT plan (`Plus` is about `$20/month`).
   - Codex: [https://openai.com/codex](https://openai.com/codex)
   - Codex CLI (repo): [https://github.com/openai/codex](https://github.com/openai/codex)
   - ChatGPT pricing: [https://openai.com/chatgpt/pricing](https://openai.com/chatgpt/pricing)

3. **Claude Code (Anthropic)**
   - Documentation: [https://docs.anthropic.com/en/docs/claude-code/overview](https://docs.anthropic.com/en/docs/claude-code/overview)
   - Claude Code cost management: [https://docs.anthropic.com/en/docs/claude-code/costs](https://docs.anthropic.com/en/docs/claude-code/costs)
   - Claude Code pricing (from about `$20/month`): [https://claude.com/pricing](https://claude.com/pricing)

If you do not want to pay for Pro versions, sensible CLI/open-source alternatives:
- Kilo Code: [https://kilocode.ai/](https://kilocode.ai/)
- OpenCode: [https://opencode.ai/](https://opencode.ai/)

Important: **inference is not free**.
Even when a tool has a Free plan or is open source, there are usually still usage limits and/or model costs (tokens, credits, API).

<a id="node-section"></a>
# 3. Install Node.js 24 (LTS)

Required for working with the test code.

I strongly recommend installing `nvm` (Node Version Manager).
It is the most convenient way to manage Node.js versions on your computer.
You can easily switch between versions (e.g., 20, 22).

## macOS

[https://tecadmin.net/install-nvm-macos-with-homebrew/](https://tecadmin.net/install-nvm-macos-with-homebrew/)

## Windows

- [https://github.com/coreybutler/nvm-windows](https://github.com/coreybutler/nvm-windows)
- [nvm-setup.exe](https://github.com/coreybutler/nvm-windows/releases/download/1.2.2/nvm-setup.exe) (direct installer link)

**Verification**

```bash
slawek playwright-2025 (master) $ node --version
v24.11.0
```

<a id="environment-section"></a>
# 4. Download and Install the Tested App + Environment

A Docker-based environment is strongly preferred because you do not need to worry about starting individual applications.
If needed, we can also install frontend and backend in an alternative way.

Docker and Docker Compose installation guide:
[https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)

Companies often use Rancher as an alternative:
[https://ranchermanager.docs.rancher.com/getting-started/installation-and-upgrade](https://ranchermanager.docs.rancher.com/getting-started/installation-and-upgrade)

```bash
git clone https://github.com/slawekradzyminski/awesome-localstack
```

Inside the downloaded folder, start the environment:

```bash
docker compose -f lightweight-docker-compose.yml up
```

Check whether frontend (port `8081`) and backend (port `4001`) are working:
[https://github.com/slawekradzyminski/awesome-localstack?tab=readme-ov-file#-services--endpoints](https://github.com/slawekradzyminski/awesome-localstack?tab=readme-ov-file#-services--endpoints)

<a id="tests-section"></a>
# 5. Download the Test Repository

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

The tests require the environment from point 4 to be running.

If you have any issues, feel free to contact me or leave comments.
