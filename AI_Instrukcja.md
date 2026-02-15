Poniżej znajdziecie organizacyjne informacje oraz instrukcję instalacji niezbędnego oprogramowania.
Proszę o sumienne przygotowanie. Jeśli pojawią się trudności techniczne, niczym się nie martwcie.
Wszystkie problemy rozwiążemy wspólnie na szkoleniu.
Można też skontaktować się ze mną wcześniej mailowo:
[slawomir.radzyminski@gmail.com](mailto:slawomir.radzyminski@gmail.com)

Zalecam pracę na komputerze prywatnym, ze względu na częste ograniczenia bezpieczeństwa na komputerach służbowych.
Prywatny komputer z pełnymi uprawnieniami administratora daje większą elastyczność w rozwiązywaniu problemów.

# Ogólne podejście do szkolenia

Poza teorią podczas szkolenia będziemy pracować na projekcie:

- [https://github.com/slawekradzyminski/playwright-2025](https://github.com/slawekradzyminski/playwright-2025)

Tutaj jest całe środowisko, które zasymuluje realną sytuację w pracy:

- [https://github.com/slawekradzyminski/awesome-localstack](https://github.com/slawekradzyminski/awesome-localstack)

Poniżej znajdziecie instrukcję, jak sprawdzić, że wszystko działa.

# Instalacja oprogramowania

1. [Instalujemy GIT](#sekcja-git)
2. [Narzędzia AI do szkolenia](#sekcja-ai-tools)
3. [Instalujemy Node.js 24 (LTS)](#sekcja-node)
   - [MacOS](#macos)
   - [Windows](#windows)
4. [Pobranie i instalacja testowanej aplikacji + środowiska](#sekcja-srodowisko)
5. [Pobranie repozytorium z testami](#sekcja-testy)

<a id="sekcja-git"></a>
# 1. Instalujemy GIT

Potrzebne do synchronizacji kodu w czasie szkolenia.

[https://git-scm.com/book/en/v2/Getting-Started-Installing-Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

Jeśli nie czujecie się pewnie z instalacją przez terminal, w powyższej instrukcji znajdziecie też linki do klasycznych instalatorów dla Windows i Mac.

- Mac: [https://sourceforge.net/projects/git-osx-installer/](https://sourceforge.net/projects/git-osx-installer/)
- Windows: [https://git-scm.com/download/win](https://git-scm.com/download/win)

<a id="sekcja-ai-tools"></a>
# 2. Narzedzia AI do szkolenia

Możecie pracować na dowolnym narzędziu, którego używacie w pracy.
Jeśli na co dzień korzystacie z GitHub Copilot, Windsurf, Claude Code albo innego asystenta, zostańcie przy swoim pracowym workflow.

Jeżeli nie macie dostępu do żadnego narzędzia to polecam następujące _płatne_ narzędzia:

1. **Cursor (IDE)**
   - Moja rekomendacja dla większości osób: `Cursor Pro` za ok. `$20/mies.`.
   - Cennik: [https://cursor.com/pricing](https://cursor.com/pricing)

2. **Codex (OpenAI, app/IDE/CLI)**
   - Jeśli wolicie pracę terminalową (`CLI`), to bardzo dobra opcja.
   - W praktyce często najprościej wejść przez plan ChatGPT (`Plus` to ok. `$20/mies.`).
   - Codex: [https://openai.com/codex](https://openai.com/codex)
   - Codex CLI (repo): [https://github.com/openai/codex](https://github.com/openai/codex)
   - Cennik ChatGPT: [https://openai.com/chatgpt/pricing](https://openai.com/chatgpt/pricing)

3. **Claude Code (Anthropic)**
   - Dokumentacja: [https://docs.anthropic.com/en/docs/claude-code/overview](https://docs.anthropic.com/en/docs/claude-code/overview)
   - Zarządzanie kosztami Claude Code: [https://docs.anthropic.com/en/docs/claude-code/costs](https://docs.anthropic.com/en/docs/claude-code/costs)
   - Cennik Claude Code (od ok. `$20/mies.`): [https://claude.com/pricing](https://claude.com/pricing)

Jeśli nie chcecie płacić za wersje Pro, sensowne alternatywy CLI/open-source:
- Kilo Code: [https://kilocode.ai/](https://kilocode.ai/)
- OpenCode: [https://opencode.ai/](https://opencode.ai/)

Ważne: **inference nie jest darmowa**.
Nawet gdy narzędzie ma plan Free albo jest open-source, zwykle i tak pojawiają się limity użycia i/lub koszt modeli (tokeny, kredyty, API).

<a id="sekcja-node"></a>
# 3. Instalujemy Node.js 24 (LTS)

Potrzebne do pracy z kodem testów.

Gorąco polecam zainstalować `nvm` (Node Version Manager).
To najwygodniejszy sposób zarządzania wersjami Node.js na komputerze.
Można łatwo przełączać się pomiędzy wersjami (np. 20, 22).

## MacOS

[https://tecadmin.net/install-nvm-macos-with-homebrew/](https://tecadmin.net/install-nvm-macos-with-homebrew/)

## Windows

- [https://github.com/coreybutler/nvm-windows](https://github.com/coreybutler/nvm-windows)
- [nvm-setup.exe](https://github.com/coreybutler/nvm-windows/releases/download/1.2.2/nvm-setup.exe) (bezpośredni link do instalatora)

**Weryfikacja**

```bash
slawek playwright-2025 (master) $ node --version
v24.11.0
```

<a id="sekcja-srodowisko"></a>
# 4. Pobranie i instalacja testowanej aplikacji + środowiska

Środowisko oparte na Dockerze jest mocno preferowane, bo nie musicie przejmować się uruchamianiem poszczególnych aplikacji.
W razie problemów możemy też zainstalować frontend i backend alternatywną drogą.

Instrukcja instalacji Dockera i Docker Compose:
[https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)

Firmy często korzystają alternatywnie z Ranchera:
[https://ranchermanager.docs.rancher.com/getting-started/installation-and-upgrade](https://ranchermanager.docs.rancher.com/getting-started/installation-and-upgrade)

```bash
git clone https://github.com/slawekradzyminski/awesome-localstack
```

Wewnątrz pobranego folderu uruchamiamy środowisko:

```bash
docker compose -f lightweight-docker-compose.yml up
```

Sprawdzamy, czy działa frontend (port `8081`) i backend (port `4001`):
[https://github.com/slawekradzyminski/awesome-localstack?tab=readme-ov-file#-services--endpoints](https://github.com/slawekradzyminski/awesome-localstack?tab=readme-ov-file#-services--endpoints)

<a id="sekcja-testy"></a>
# 5. Pobranie repozytorium z testami

```bash
git clone https://github.com/slawekradzyminski/playwright-2025
```

Uruchamiamy:

```bash
npm install
npx playwright install
npx playwright test
```

![Wynik testow Playwright](images/pw_tests.png)

Testy wymagają uruchomionego środowiska z punktu 4.

W razie kłopotów zachęcam do kontaktu lub dodawania komentarzy.
