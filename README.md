# Relatório de Prontidão Técnica: Onboarding SecOps
**Disciplina:** Engenharia de Produto de Software (FGA0316) - 2026.1
**Aluno:** Ian Lucca Soares Mesquita | **Matrícula:** 211045140

## 1. Configuração do Ambiente (Zero Trust & Isolamento)
Conforme as diretrizes de Soberania Técnica, as seguintes configurações foram aplicadas:
- [X] **Python 3.12:** Instalado e verificado.
- [X] **Poetry:** Configurado para criar `.venv` dentro do projeto (`virtualenvs.in-project true`).
- [X] **Determinismo:** Arquivos `pyproject.toml` e `poetry.lock` gerados com sucesso.

### 1.1 Evidencia de Ambiente

```
ianlucca@Ian:~/secops_pcdf$ poetry env info

Virtualenv
Python:         3.12.13
Implementation: CPython
Path:           /home/ianlucca/secops_pcdf/.venv
Executable:     /home/ianlucca/secops_pcdf/.venv/bin/python
Valid:          True

Base
Platform:   linux
OS:         posix
Python:     3.12.13
Path:       /home/ianlucca/.pyenv/versions/3.12.13
Executable: /home/ianlucca/.pyenv/versions/3.12.13/bin/python3.12
```

## 2. Logs de Auditoria e Qualidade (Security Gate)
Abaixo constam os resumos das execuções dos comandos de segurança:

### 2.1. Auditoria Estática (Bandit)
```
ianlucca@Ian:~/secops_pcdf$ poetry run bandit -r . 
[main]  INFO    Found project level .bandit file: ./.bandit
[main]  INFO    Using command line arg for selected targets
[main]  INFO    profile include tests: None
[main]  INFO    profile exclude tests: None
[main]  INFO    cli include tests: None
[main]  INFO    cli exclude tests: None
[main]  INFO    running on Python 3.12.13
Run started:2026-03-28 15:22:56.654543+00:00

Test results:
        No issues identified.

Code scanned:
        Total lines of code: 9
        Total lines skipped (#nosec): 0

Run metrics:
        Total issues (by severity):
                Undefined: 0
                Low: 0
                Medium: 0
                High: 0
        Total issues (by confidence):
                Undefined: 0
                Low: 0
                Medium: 0
                High: 0
Files skipped (0):
ianlucca@Ian:~/secops_pcdf$ 
```

### 2.2. Verificação de Dependências (Safety)
```
ianlucca@Ian:~/secops_pcdf$ poetry run safety check
+=========================================================================================================================+

                               /$$$$$$            /$$
                              /$$__  $$          | $$
           /$$$$$$$  /$$$$$$ | $$  \__//$$$$$$  /$$$$$$   /$$   /$$
          /$$_____/ |____  $$| $$$$   /$$__  $$|_  $$_/  | $$  | $$
         |  $$$$$$   /$$$$$$$| $$_/  | $$$$$$$$  | $$    | $$  | $$
          \____  $$ /$$__  $$| $$    | $$_____/  | $$ /$$| $$  | $$
          /$$$$$$$/|  $$$$$$$| $$    |  $$$$$$$  |  $$$$/|  $$$$$$$
         |_______/  \_______/|__/     \_______/   \___/   \____  $$
                                                          /$$  | $$
                                                         |  $$$$$$/
  by pyup.io                                              \______/

+=========================================================================================================================+

 REPORT 

  Safety v2.3.5 is scanning for Vulnerabilities...
  Scanning dependencies in your environment:

  -> /home/ianlucca/secops_pcdf/.venv/lib/python3.12/site-packages

  Using non-commercial database
  Found and scanned 30 packages
  Timestamp 2026-03-28 14:03:21
  2 vulnerabilities found
  0 vulnerabilities ignored

+=========================================================================================================================+
 VULNERABILITIES FOUND 
+=========================================================================================================================+

-> Vulnerability found in setuptools version 69.5.1
   Vulnerability ID: 72236
   Affected spec: <70.0.0
   ADVISORY: Affected versions of Setuptools allow for remote code execution via its download functions. These
   functions, which are used to download packages from URLs provided by users or retrieved from package index servers,...
   CVE-2024-6345
   For more information, please visit https://getsafety.com/v/72236/f17


-> Vulnerability found in setuptools version 69.5.1
   Vulnerability ID: 76752
   Affected spec: <78.1.1
   ADVISORY: Affected versions of Setuptools are vulnerable to Path Traversal via PackageIndex.download(). The
   impact is Arbitrary File Overwrite: An attacker would be allowed to write files to arbitrary locations on the...
   CVE-2025-47273
   For more information, please visit https://getsafety.com/v/76752/f17

 Scan was completed. 2 vulnerabilities were found. 

+=========================================================================================================================+
   REMEDIATIONS

  2 vulnerabilities were found in 1 package. For detailed remediation & fix recommendations, upgrade to a commercial 
  license. 

+=========================================================================================================================+
ianlucca@Ian:~/secops_pcdf$ 
``` 

### 2.3. Qualidade e Conformidade (Ruff)
```
ianlucca@Ian:~/secops_pcdf$ poetry run ruff check .
All checks passed!
ianlucca@Ian:~/secops_pcdf$ 

``` 

## 3. Evidência de Integração Contínua (CI)
O pipeline automatizado foi executado com sucesso no GitHub Actions:
- **Link da Action de Sucesso:** [COLE AQUI O LINK DO SEU GITHUB ACTIONS]

## 4. Declaração de Soberania Técnica (CISSP Domain 8)
Eu, [Nome do Aluno], declaro que auditei manualmente as ferramentas e dependências deste projeto. Confirmo que o código gerado via IA (GitHub Copilot) passou pela minha revisão humana (*Human-in-the-loop*), garantindo que não há vazamento de segredos ou falhas lógicas críticas antes da migração para o ecossistema da PCDF.

---
**Data de Entrega:** [DD/MM/2026]

## Observações Adicionais

* O SCA (Safety) identificou 2 vulnerabilidades no pacote setuptools 69.5.1 (CVE-2024-6345 e CVE-2025-47273). Estas vulnerabilidades são decorrentes de uma limitação de ambiente (módulo _sqlite3 não compilado), que forçou o uso de uma versão antiga do setuptools como dependência transitória do safety. A remediação seria atualizar para setuptools>=78.1.1, o que quebra a compatibilidade com safety 2.x neste ambiente.