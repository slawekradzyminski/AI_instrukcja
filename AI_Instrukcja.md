# Przygotowanie środowiska do szkolenia z Playwright i AI

Poniżej znajdziecie organizacyjne informacje oraz aktualną instrukcję przygotowania środowiska na szkolenie.
Instrukcja została zweryfikowana 23 sierpnia 2026 r. z repozytorium `playwright-2026`
i lekkim profilem `awesome-localstack` uruchomionym lokalnie.
Proszę o sumienne przygotowanie. Jeśli pojawią się trudności techniczne, niczym się nie martwcie.
Wszystkie problemy rozwiążemy wspólnie na szkoleniu.
Można też skontaktować się ze mną wcześniej mailowo:
[slawomir.radzyminski@gmail.com](mailto:slawomir.radzyminski@gmail.com)

Zalecam pracę na komputerze prywatnym, ze względu na częste ograniczenia bezpieczeństwa na komputerach służbowych.
Prywatny komputer z pełnymi uprawnieniami administratora daje większą elastyczność w rozwiązywaniu problemów.

# Ogólne podejście do szkolenia

Poza teorią podczas szkolenia będziemy pracować na projekcie:

- [https://github.com/slawekradzyminski/playwright-2026](https://github.com/slawekradzyminski/playwright-2026)

Tutaj jest środowisko aplikacyjne, na którym będziemy pracować:

- [https://github.com/slawekradzyminski/awesome-localstack](https://github.com/slawekradzyminski/awesome-localstack)

Podstawowy scenariusz na szkoleniu opiera się na lekkim profilu Dockera (`lightweight`), więc to właśnie ten profil warto uruchomić i sprawdzić przed zajęciami.

# Instalacja oprogramowania

1. [Instalujemy GIT](#sekcja-git)
2. [Wybieramy edytor kodu lub IDE](#sekcja-vscode)
3. [Narzędzia AI do szkolenia](#sekcja-ai-tools)
4. [Instalujemy aktualną wersję LTS Node.js (obecnie 24)](#sekcja-node)
   - [MacOS](#macos)
   - [Windows](#windows)
5. [Pobranie i instalacja testowanej aplikacji + środowiska](#sekcja-srodowisko)
6. [Weryfikacja środowiska](#sekcja-weryfikacja)
7. [Pobranie repozytorium z testami](#sekcja-testy)

<a id="sekcja-git"></a>
# 1. Instalujemy GIT

Potrzebne do synchronizacji kodu w czasie szkolenia.

[https://git-scm.com/book/en/v2/Getting-Started-Installing-Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

Jeśli nie czujecie się pewnie z instalacją przez terminal, w powyższej instrukcji znajdziecie też linki do klasycznych instalatorów dla Windows i Mac.

- macOS: [https://git-scm.com/download/mac](https://git-scm.com/download/mac)
- Windows: [https://git-scm.com/download/win](https://git-scm.com/download/win)

<a id="sekcja-vscode"></a>
# 2. Wybieramy edytor kodu lub IDE

Na szkolenie przygotujcie edytor kodu lub IDE do otwierania projektu, edytowania i zarządzania kodem testów Playwright. Możecie korzystać z dowolnego narzędzia obsługującego JavaScript i TypeScript, także tego, którego używacie już w pracy.

Najpopularniejszym wyborem jest **Visual Studio Code (VS Code)** ([badanie Stack Overflow Developer Survey](https://survey.stackoverflow.co/2025/)). Jeśli nie macie jeszcze swojego ulubionego edytora, polecam zacząć od niego.

Linki do pobrania — wystarczy jedno z tych narzędzi:

- [Visual Studio Code](https://code.visualstudio.com/download)
- [WebStorm (JetBrains)](https://www.jetbrains.com/webstorm/download/)
- [Cursor](https://cursor.com/download)

Po pobraniu repozytorium z testami otwórzcie w wybranym edytorze cały folder `playwright-2026`.
W VS Code służy do tego **File → Open Folder…** (Plik → Otwórz folder…). Wbudowany terminal (**Terminal → New Terminal**) pozwoli uruchamiać komendy i testy z dalszej części instrukcji. W innym IDE użyjcie odpowiednich opcji lub osobnego terminala.

<a id="sekcja-ai-tools"></a>
# 3. Narzędzia AI do szkolenia

Wystarczy jedno działające narzędzie AI. Jeśli macie już sprawdzone narzędzie w pracy, możecie przy nim zostać.
Jeżeli wybieracie narzędzie po raz pierwszy, poniżej znajdziecie moje rekomendacje na ten warsztat.

Rekomendacje i ceny sprawdzone 31 października 2026 r. Podane kwoty dotyczą miesięcznego rozliczenia w USD;
podatki, ceny regionalne, promocje i limity mogą się różnić. Przed zakupem sprawdźcie aktualny cennik.
Abonament nie oznacza nieograniczonej pracy agenta przez cały dzień.

## Polecane: Claude Code i Codex

1. **Claude Code + Claude Pro — 20 USD/mies.**
   To mój pierwszy wybór dla osób zaczynających pracę z agentem na tym warsztacie.
   Plan Pro obejmuje Claude Code; nie trzeba kupować dodatkowo dostępu do API.
   Obowiązują limity użycia współdzielone z innymi funkcjami Claude.
   - [Instalacja i pierwsze uruchomienie](https://code.claude.com/docs/en/quickstart)
   - [Cennik Claude Pro](https://claude.com/pricing)

2. **Codex + ChatGPT Plus — 20 USD/mies.**
   To moja druga polecana opcja. Możecie pracować lokalnie z Codexem w aplikacji, IDE albo terminalu.
   Zalogujcie się kontem ChatGPT z planem Plus. Osobny klucz API nie jest potrzebny;
   użycie klucza API jest rozliczane oddzielnie od abonamentu. Tutaj również obowiązują limity użycia.
   - [Pierwsze kroki z Codexem](https://developers.openai.com/codex/quickstart/)
   - [Plany, ceny i limity Codexa](https://developers.openai.com/codex/pricing/)

To moje rekomendacje do ćwiczeń, a nie gwarancja, że jeden agent będzie najlepszy w każdym zadaniu.
Nie musicie kupować obu abonamentów ani droższych planów na start.

## Inne opcje: Cursor i GitHub Copilot

Oba narzędzia nadają się do warsztatu, szczególnie jeśli już ich używacie.
Przy intensywnej pracy agentowej mogą jednak wychodzić dość drogo: znaczenie ma koszt wybranego modelu,
zużycie kredytów i ewentualne dopłaty, a nie tylko cena samego abonamentu.
Dlatego na start preferuję dwa narzędzia wymienione wyżej.

- **Cursor** — edytor z agentem AI. Dostępne modele i ich rozliczenie zależą od planu;
  użycie modeli zewnętrznych może być naliczane oddzielnie.
  [Cennik i zasady rozliczeń](https://cursor.com/pricing).
- **GitHub Copilot** — integracja z IDE oraz CLI. Pro zaczyna się od **10 USD/mies.**,
  więc sama opłata wejściowa jest niższa niż w polecanych planach.
  Funkcje agentowe zużywają jednak GitHub AI Credits, a dodatkowe użycie może wymagać dopłaty.
  [Cennik](https://github.com/features/copilot/plans),
  [pierwsze kroki](https://docs.github.com/en/copilot/get-started/quickstart).

## Opcje budżetowe i modele chińskich dostawców

Jeżeli priorytetem jest koszt, można rozważyć poniższe rozwiązania.
Niższa cena abonamentu nie oznacza identycznej jakości, limitów ani wygody konfiguracji.

- **Qwen Code + Alibaba Cloud Model Studio Token Plan** — Token Plan Lite ma cenę standardową
  **8 USD/mies.**; obecnie widoczna promocja to **6 USD/mies.** Oferta jest dostępna w regionie Singapore.
  To osobny produkt od dawnego Coding Plan Lite, którego nie można już kupić.
  [Aktualny plan i cennik](https://www.alibabacloud.com/help/en/model-studio/token-plan-overview),
  [konfiguracja Qwen Code](https://qwenlm.github.io/qwen-code-docs/en/users/configuration/auth/).
- **GLM Coding Plan (Z.AI)** — dostęp do modeli GLM w obsługiwanym agencie, np. Claude Code lub OpenCode.
  Standardowa cena Lite zaczyna się od **18 USD/mies.**; promocje mogą ją obniżać.
  Wybranie GLM w Claude Code oznacza korzystanie z usługi Z.AI, z jej własnymi limitami i rozliczeniem.
  [Opis planu i limity](https://docs.z.ai/devpack/overview),
  [konfiguracja](https://docs.z.ai/devpack/quick-start).
- **Kimi Code (Moonshot AI)** — alternatywny agent i modele Kimi.
  Przed zakupem sprawdźcie, czy wybrany plan obejmuje Kimi Code:
  według aktualnej dokumentacji nowe plany wymagają **Plus lub wyższego**, a Go nie obejmuje dostępu do Kimi Code.
  [Dostępność w planach i limity](https://www.kimi.com/code/docs/en/kimi-code/membership.html),
  [pierwsze kroki](https://www.kimi.com/code/docs/en/kimi-code-cli/guides/getting-started.html).
- **OpenCode** — otwarty klient agenta, do którego wybieracie dostawcę i model; sam klient nie zapewnia
  darmowego dostępu do dowolnego modelu. Opcjonalny **OpenCode Go kosztuje 10 USD/mies.**
  i daje dostęp do wybranych modeli w ramach limitów.
  **Raczej nie polecam zaczynać od OpenCode na pierwszy warsztat**:
  trzeba dodatkowo dobrać model, skonfigurować dostawcę i rozumieć jego rozliczenia.
  To opcja dla osób, które chcą świadomie eksperymentować z konfiguracją.
  [Dokumentacja OpenCode](https://opencode.ai/docs/),
  [OpenCode Go: ceny i konfiguracja](https://opencode.ai/docs/go/).

## Sprawdźcie narzędzie przed warsztatem

Agent powinien działać na tym samym komputerze co aplikacja albo mieć możliwość uruchamiania
komend w lokalnym terminalu. Przed zajęciami sprawdźcie, czy otwiera repozytorium, edytuje pliki
i uruchamia komendy oraz testy. Sam czat w przeglądarce nie wystarczy do tych ćwiczeń.
Agent działający wyłącznie w zdalnej chmurze zwykle nie zobaczy Waszego `http://localhost:8081`
bez dodatkowej konfiguracji. Nie wklejajcie kluczy API do promptów, plików repozytorium
ani logów udostępnianych innym osobom.

Nawet gdy klient jest open-source lub ma plan Free, dostęp do modeli może być płatny albo ograniczony.

<a id="sekcja-node"></a>
# 4. Instalujemy aktualną wersję LTS Node.js (obecnie 24)

Potrzebne do pracy z kodem testów.

Gorąco polecam zainstalować `nvm` (Node Version Manager).
To najwygodniejszy sposób zarządzania wersjami Node.js na komputerze.
Na potrzeby szkolenia używamy tylko aktualnej wersji LTS Node.js (obecnie 24).

## macOS

[Oficjalna instrukcja instalacji nvm](https://github.com/nvm-sh/nvm#installing-and-updating)

## Windows

- [https://github.com/coreybutler/nvm-windows](https://github.com/coreybutler/nvm-windows)
- [Najnowsza wersja nvm-windows](https://github.com/coreybutler/nvm-windows/releases/latest)

Jeśli na Windowsie i PowerShellu pojawią się problemy z dostępem lub uruchamianiem komend, może pomóc uruchomienie:

```powershell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
```

**Weryfikacja**

```bash
node --version
```

Oczekiwana wersja:

```bash
v24.x.x
```

<a id="sekcja-srodowisko"></a>
# 5. Pobranie i instalacja testowanej aplikacji + środowiska

Środowisko oparte na Dockerze jest mocno preferowane, bo nie musicie przejmować się uruchamianiem poszczególnych aplikacji.
W razie problemów możemy też zainstalować frontend i backend alternatywną drogą.

Instrukcja instalacji Dockera i Docker Compose:
[https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)

Alternatywą dla Docker Desktop może być Rancher Desktop:
[https://docs.rancherdesktop.io/getting-started/installation/](https://docs.rancherdesktop.io/getting-started/installation/)

```bash
git clone https://github.com/slawekradzyminski/awesome-localstack
cd awesome-localstack
```

Wewnątrz pobranego folderu uruchamiamy lekki profil szkoleniowy:

```bash
docker compose -f lightweight-docker-compose.yml up -d
```

To uruchamia jedną publiczną bramkę aplikacji pod adresem:

- `http://localhost:8081`

Za tą bramką dostępne są:

- frontend
- backend API pod `/api/v1/...`
- Swagger UI
- obrazy produktów pod `/images/...`
- mock LLM pod osobnym porcie `11434`
- Keycloak / SSO pod osobnym portem `8082`

Szczegółowy opis lekkiego profilu, adresów oraz kroków weryfikacyjnych znajdziecie tutaj:

- [awesome-localstack/docs/STUDENT_GUIDE.md](https://github.com/slawekradzyminski/awesome-localstack/blob/main/docs/STUDENT_GUIDE.md)

<a id="sekcja-weryfikacja"></a>
# 6. Weryfikacja środowiska

Po uruchomieniu sprawdźcie:

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

Oczekiwany rezultat:

- serwisy `backend`, `frontend`, `gateway`, `keycloak` i `ollama-mock` są w stanie `Up`
- strona logowania odpowiada `200`
- OpenAPI odpowiada `200`
- obrazek produktu odpowiada `200`
- konfiguracja OpenID Connect w Keycloak odpowiada `200`
- mock LLM odpowiada `200`

Przydatne adresy do sprawdzenia w przeglądarce:

- aplikacja: `http://localhost:8081/login`
- Swagger: `http://localhost:8081/swagger-ui/index.html`
- Keycloak: `http://localhost:8082`

W Windows PowerShell można użyć `curl.exe` zamiast `curl`, jeśli ta nazwa jest
przechwytywana przez alias PowerShella.

Lokalne dane logowania:

- klasyczne logowanie w aplikacji: `client` / `client`
- klasyczne konto administratora: `admin` / `LocalDemoAdmin123!`
- SSO przez Keycloak: `sso-client` / `SsoClient123!`
- Social login (Google mock): `google-user` / `GoogleUser123!`
- Social login (GitHub mock): `github-user` / `GitHubUser123!`
- panel administracyjny Keycloak: `admin` / `admin`

Jeśli coś jeszcze się uruchamia, najpierw sprawdźcie logi:

```bash
docker compose -f lightweight-docker-compose.yml logs -f backend gateway
docker compose -f lightweight-docker-compose.yml logs -f keycloak ollama-mock
```

<a id="sekcja-testy"></a>
# 7. Pobranie repozytorium z testami

```bash
git clone https://github.com/slawekradzyminski/playwright-2026
cd playwright-2026
```

Uruchamiamy:

```bash
npm ci
npx playwright install chromium
npx playwright test
```

![Wynik testow Playwright](images/pw_tests.png)

Testy wymagają uruchomionego środowiska z punktu 5 i przechodzą przez główny adres aplikacji:

- `http://localhost:8081`

Aktualny endpoint logowania API używany w testach to:

- `http://localhost:8081/api/v1/users/signin`

W razie kłopotów zachęcam do kontaktu.
