# Portfólio de Qualidade (QA) — Auditoria do Sistema NaSalinha

Repositório de auditoria de qualidade do projeto **NaSalinha** (Sistema de Check-in Gamificado), desenvolvido para a trilha de **Quality Assurance (QA) da Comp Júnior**.

Este repositório adota a **Opção A (Documentação Integrada)**: o planejamento, a execução, os Casos de Teste e os Bug Reports estão no **GitHub Issues** e no **GitHub Projects**, acompanhados pela coleção exportada do **Insomnia**.

---

## Sumário

1. [Guia rápido para o avaliador](#1-guia-rápido-para-o-avaliador)
2. [Escopo da auditoria](#2-escopo-da-auditoria)
3. [Tecnologias e ferramentas](#3-tecnologias-e-ferramentas)
4. [Estratégia de testes](#4-estratégia-de-testes)
5. [Resumo da auditoria](#5-resumo-da-auditoria)
6. [Como executar o projeto](#6-como-executar-o-projeto)
7. [Padrões do GitHub Projects e Issues](#7-padrões-do-github-projects-e-issues)
8. [Teste de regressão (simulado)](#8-teste-de-regressão-simulado)
9. [Como utilizar a coleção do Insomnia](#9-como-utilizar-a-coleção-do-insomnia-testes-de-api)
10. [Índice de rastreabilidade](#10-índice-de-rastreabilidade)

---

## 1. Guia rápido para o avaliador

Onde encontrar cada item exigido pelo desafio:

| Requisito do desafio | Onde está |
|---|---|
| README (descrição, ferramentas e tipos de teste) | Este arquivo |
| Coleção de API (Insomnia) | `<CAMINHO_DO_ARQUIVO.json>` (ver [seção 9](#9-como-utilizar-a-coleção-do-insomnia-testes-de-api)) |
| Casos de Teste (31 CTs + 3 de regressão) | [Issues](../../issues) com o label `Tipo: CT`, ou a view **CTs** do GitHub Projects |
| Bug Reports (12 bugs, mínimo de 2 por área) | [Issues](../../issues) com o label `Tipo: Bug`, ou a view **Bugs** do GitHub Projects |
| Casos Ad-hoc (bugs extras) | Issues com o label `Ad-hoc` (CT-23, CT-25 e CT-28) |
| Teste de regressão simulado | [Seção 8](#8-teste-de-regressão-simulado): CT-33, CT-34 e CT-35 |
| Relatório de Testes Final | `<LINK_DO_RELATORIO>` |
| Vídeo de demonstração (máx. 10 min) | `<LINK_DO_VIDEO>` |

**Convenção importante:** os Casos de Teste e os bugs são identificados por **ID próprio** (`CT-XX` e `BUG-XX`), que aparece no título de cada issue. Esse ID **não é o número da issue no GitHub** (por exemplo, `BUG-12` é a issue `#47`). A [seção 10](#10-índice-de-rastreabilidade) traz a tabela de correspondência completa.

---

## 2. Escopo da auditoria

A auditoria avalia a experiência do usuário, o cumprimento das regras de negócio e a estabilidade e segurança da API nas **3 Funcionalidades Core** do sistema:

1. **Autenticação JWT:** registro, verificação de conta por e-mail, login, validade de sessão e controle de acesso baseado em cargos (RBAC: Admin, Member e Trainee).
2. **Check-in por Foto:** upload de comprovantes de presença, validação de formato/tamanho de mídia, restrição temporal (1 check-in a cada 24h), moderação por Administrador e isolamento de dados entre usuários.
3. **Sistema de Pontos:** cálculo automático de pontuação na aprovação ou exclusão de check-ins, restrição de 24h entre check-ins (que impede pontuação duplicada), persistência no banco de dados e reflexo no ranking da temporada ativa.

---

## 3. Tecnologias e ferramentas

| Finalidade | Ferramenta |
|---|---|
| Infraestrutura e ambiente local | Docker, Docker Compose, Node.js, Prisma ORM e Git |
| Testes de API | Insomnia |
| Testes de interface e inspeção | Navegador Web (DevTools) e logs dos containers Docker |
| Verificação de e-mails (cadastro) | Mailtrap (Sandbox) |
| Gestão de testes e defeitos | GitHub Projects (Kanban) e GitHub Issues |

---

## 4. Estratégia de testes

### 4.1. Tipos de teste aplicados

| Tipo | Foco | Quantidade |
|---|---|---|
| **Funcional** | Comportamento na interface (front-end) comparado ao esperado | 13 CTs |
| **API** | Endpoints, status codes e regras de negócio do backend (Insomnia) | 18 CTs |
| **Ad-hoc** | Cenários descobertos durante a exploração, registrados para manter a rastreabilidade (já contados acima) | 3 CTs |
| **Regressão (simulada)** | Garantia de que a correção de um bug não gera efeitos colaterais | 3 CTs (um por área) |

Os casos incluem cenários **positivos** (o sistema aceita o que deve aceitar) e **negativos** (o sistema recusa o que deve recusar), identificados pelos labels `Positivo` e `Negativo`.

### 4.2. Ciclo de vida seguido

1. **Planejar:** os Casos de Teste foram escritos com base no documento de requisitos, cada um com resultado esperado e requisito associado (RF/RNF).
2. **Executar:** interface no navegador, endpoints no Insomnia e logs no terminal do Docker para identificar falhas invisíveis na tela.
3. **Documentar:** cada falha gerou imediatamente um Bug Report com passos, resultado obtido, resultado esperado e evidência.
4. **Reportar:** os resultados foram consolidados no Relatório de Testes Final.

### 4.3. Casos Ad-hoc

Erros encontrados fora do planejamento inicial foram registrados como **Caso de Teste Ad-hoc** (para manter a rastreabilidade) e depois documentados em um Bug Report com evidência.

---

## 5. Resumo da auditoria

### 5.1. Casos de Teste

| Planejados | Executados | Passou | Falhou | Impedido |
|---|---|---|---|---|
| 34 (31 + 3 de regressão) | `__` | `__` | `__` | `__` |

> Os testes **impedidos** não são contados como falha. Eles não puderam ser executados por causa de um bug anterior.
> Os 3 casos de regressão são uma **simulação** documentada (ver [seção 8](#8-teste-de-regressão-simulado)).

### 5.2. Bugs por severidade

| Severidade | Quantidade | Bugs |
|---|---|---|
| Crítico | 2 | BUG-02, BUG-03 |
| Maior | 6 | BUG-05, BUG-06, BUG-07, BUG-08, BUG-09, BUG-11 |
| Médio | 3 | BUG-01, BUG-4, BUG-12 |
| Menor | 1 | BUG-10 |
| **Total** | **12** | |

### 5.3. Bugs por módulo

| Módulo | Quantidade | Bugs |
|---|---|---|
| Autenticação JWT | 3 | BUG-01, BUG-02, BUG-03 |
| Check-in por Foto | 7 | BUG-04 a BUG-10 |
| Sistema de Pontos | 2 | BUG-11, BUG-12 |

---

## 6. Como executar o projeto

### 6.1. Obtenção do projeto (obrigatório)

O código do sistema **NaSalinha não está neste repositório**, que contém apenas a documentação da auditoria. Para executar os testes, é necessário **clonar o repositório oficial do desafio**:

```bash
git clone https://github.com/Gustavohmmagalhaes/nasalinha-qa-challenge.git
```

> O tutorial abaixo é apenas **complementar** às instruções desse repositório, que continuam sendo a referência principal para subir o ambiente.

### 6.2. Preparação
Certifique-se de ter o **Docker** e o **Git** instalados em sua máquina.

### 6.3. Configuração de variáveis
1. Vá até a pasta `/backend` e copie o arquivo `.env.example` para um novo arquivo `.env`:
   ```bash
   cp backend/.env.example backend/.env
   ```
2. **Configuração de chaves obrigatória:** para que o sistema funcione, crie suas próprias contas (gratuitas) nos serviços abaixo e preencha as credenciais no `.env`:
   * **E-mails:** utilize o **Mailtrap** (Sandbox). É nele que chegam os e-mails de verificação de conta.
   * **Fotos:** utilize o **Cloudinary**.
   * Defina também a URL do frontend no `.env`:
     ```env
     FRONTEND_URL=http://localhost:3000
     ```

### 6.4. Execução com Docker
1. Na raiz do projeto, suba os containers:
   ```bash
   sudo docker compose up --build
   ```
2. Execute as migrations e os seeders do backend:
   ```bash
   sudo docker compose exec backend npx prisma migrate dev --name init
   sudo docker compose exec backend npx prisma db seed
   ```
3. Para inspecionar o banco de dados (útil nos testes de persistência, como o hash de senha, o status da conta e o saldo de pontos), abra o Prisma Studio:
   ```bash
   sudo docker compose exec backend npx prisma studio
   ```
   Por padrão, ele abre em `http://localhost:5555`.
4. Acesse os serviços:
   * **Frontend:** `http://localhost:3000`
   * **API (backend):** `http://localhost:5001`

---

## 7. Padrões do GitHub Projects e Issues

Toda a rastreabilidade entre requisitos, casos de teste e defeitos está no **GitHub Projects** vinculado a este repositório, dividido em duas visualizações (*Views*).

### 7.1. Nomenclatura dos títulos

| Item | Padrão | Exemplo |
|---|---|---|
| Caso de Teste | `[CT-XX] Módulo - Descrição (RF-XX)` | `[CT-04] Check-in - Validação do status inicial PENDING (RF-05)` |
| Bug Report | `[BUG-XX] Módulo - Descrição (RF-XX)` | `[BUG-06] Check-in - Novos check-ins criados como aprovados automaticamente (RF-05)` |

### 7.2. Views e fluxo de status

A informação de **etapa** (onde o item está) fica no **Status do Project**. Os labels classificam o item, mas não indicam a etapa.

**View `CTs` (gestão de Casos de Teste):**

| Status | Significado |
|---|---|
| Planejado | Caso modelado, aguardando execução |
| Em execução | Cenário em validação na interface ou no Insomnia |
| Concluído | O sistema se comportou conforme o esperado (*Pass*) |
| Falhou | O sistema violou a regra de negócio, o contrato da API ou o comportamento esperado (*Fail*), gerando um Bug Report |
| Impedido | Execução bloqueada (*Blocked*) por um defeito anterior que impede chegar à etapa de validação |

**View `Bugs` (rastreamento de defeitos):**

| Status | Significado |
|---|---|
| Não corrigido | Defeito identificado e documentado, aguardando correção |
| Aguardando reteste | A correção foi entregue (aqui, **simulada**) e o QA ainda precisa verificá-la |
| Corrigido | Defeito solucionado e validado em reteste |

### 7.3. Padrão de labels

| Categoria | Labels |
|---|---|
| **Tipo de issue** | `Tipo: CT`, `Tipo: Bug` |
| **Módulo** | `Módulo: JWT`, `Módulo: Check-in`, `Módulo: Pontos` |
| **Tipo de teste** | `Funcional`, `API`, `Regressão`, `Ad-hoc` |
| **Natureza do cenário** | `Positivo`, `Negativo` |
| **Severidade do bug** | `Bug: Crítico`, `Bug: Maior`, `Bug: Médio`, `Bug: Menor` |

### 7.4. Critérios de severidade

| Severidade | Critério |
|---|---|
| **Crítico** | Trava o sistema, impede um fluxo principal por completo (ex.: nenhuma conta nova consegue ser verificada), causa perda de dados ou representa uma **falha grave de segurança** (ex.: quebra de isolamento/IDOR) |
| **Maior** | Quebra direta de regra de negócio ou erro de lógica central (ex.: restrição de 24h ignorada, check-in criado como aprovado) |
| **Médio** | Comportamento anômalo parcial ou resposta HTTP fora do padrão (ex.: status code incorreto), sem comprometer o banco de dados |
| **Menor** | Falha visual de interface ou erro de usabilidade, sem impacto funcional |

> O desafio prevê três níveis (Crítico, Maior e Menor). A categoria **Médio** foi adicionada para separar defeitos de impacto intermediário, e é apresentada separadamente no relatório.

---

## 8. Teste de regressão (simulado)

O desafio pede que, para cada área Core, se simule a correção de um bug reportado e se descreva quais testes seriam repetidos para garantir que a correção não gerou efeitos colaterais.

**Os três casos são uma simulação documentada.** Nenhum código foi alterado e os bugs envolvidos continuam, de fato, abertos.

| Caso | Área | Bug(s) de referência | Correção simulada |
|---|---|---|---|
| **CT-33** | JWT | BUG-02 e BUG-03 | A tabela `User` passa a ter o estado `PENDING` e o e-mail de verificação passa a conter o token |
| **CT-34** | Check-in | BUG-06 e BUG-08 | Todo novo check-in é criado como `PENDING` e os pontos só são creditados na aprovação |
| **CT-35** | Pontos | BUG-11 e BUG-05 | O backend bloqueia um novo check-in dentro do intervalo mínimo definido no RF-09 |

### 8.1. Como cada caso está organizado

* **Reteste:** os testes que originaram o bug (falharam ou foram impedidos por ele). Respondem à pergunta: *"o bug foi corrigido?"*
* **Regressão, impacto direto (prioridade alta):** testes que já passavam e usam diretamente o código alterado.
* **Regressão, impacto indireto (prioridade média):** testes de outras funcionalidades que dependem do resultado da correção.

Um teste que estava **impedido** pelo bug não entra como regressão, porque nunca foi executado. Com a correção, ele passa a ser executável e é tratado como **reteste**.

### 8.2. Reflexo no GitHub Projects

* Os bugs da simulação ficam em **Aguardando reteste** (a correção "foi entregue", mas o QA ainda não verificou), com um comentário na issue indicando o CT de regressão associado e que não houve alteração real de código.
* Nenhum bug é movido para **Corrigido**, pois não houve correção real.
* Os CTs citados mantêm o status real (por exemplo, os impedidos continuam em **Impedido**).

---

## 9. Como utilizar a coleção do Insomnia (testes de API)

O arquivo JSON exportado do Insomnia está em: `<CAMINHO_DO_ARQUIVO.json>`.

A coleção foi estruturada para que o avaliador reproduza cada teste de API sem montar requisições manualmente:

* **Autenticação rápida (pasta `Logins`):** vários CTs pedem login como administrador (`Admin`) ou como membro comum (`Trainee`/`Member`). Na pasta `Logins`, basta selecionar o perfil e enviar a requisição (**Send**).
* **Um Caso de Teste por pasta (`CT-XX`):** dentro das pastas de cada módulo (`Autenticação JWT`, `Check-in`, `Pontos`), cada CT de API tem uma pasta com o seu código (ex.: `CT-06`, `CT-18`, `CT-22`).
* **Execução guiada:** algumas pastas têm uma única requisição pré-configurada, enquanto outras têm várias requisições numeradas (`Passo 1`, `Passo 2`, `Passo 3`). A issue do CT no GitHub detalha quais passos executar.

### 9.1. Configuração da variável de ambiente (`base_url`)

A exportação do Insomnia preserva as requisições, mas **não salva o valor das variáveis de ambiente**. Antes de iniciar os testes, configure a URL base:

1. No canto superior direito do Insomnia, clique no gerenciador de ambientes (geralmente "Base Environment") e crie o ambiente.
2. Certifique-se de que o JSON do ambiente contenha a variável:
   ```json
   {
     "base_url": "http://localhost:5001"
   }
   ```

### 9.2. Upload de arquivos e a pasta `/assets`

O formato JSON do Insomnia salva a estrutura da requisição, mas não embute arquivos físicos. Por isso, nos testes de **Check-in**, o campo `photo` do *Multipart Form* não contém o arquivo original.

Imagens de exemplo estão na pasta `/assets` deste repositório. Para anexá-las no Insomnia:

**1. Permissão de acesso a arquivos (erro de segurança do Insomnia):**
Por padrão, o Insomnia bloqueia a leitura de arquivos locais. Se aparecer o erro *Restricted File Access*:
* Abra as configurações do Insomnia (engrenagem ou **Preferences**).
* Na aba **General** (ou Security/Data, conforme a versão), procure as configurações de acesso a arquivos.
* Conceda permissão para que o aplicativo leia a pasta `/assets` (ou o diretório onde você clonou o projeto).

**2. Como anexar a imagem na requisição pré-configurada:**
* Abra a requisição de Check-in e vá até a aba **Body** (já formatada como *Multipart Form*).
* Localize a linha onde o campo **Name** é `photo`.
* Na coluna da direita (Value), clique na seta ao lado do campo de texto e mude o tipo de `Text` para **`File`**.
* Clique em **Choose File**, navegue até a pasta `/assets` (ou qualquer pasta do seu computador) e selecione a imagem.

---

## 10. Índice de rastreabilidade

Correspondência entre o **ID do documento** e o **número da issue** no GitHub.

### 10.1. Casos de Teste

| ID | Issue | Módulo | Tipo | Descrição |
|---|---|---|---|---|
| CT-01 | #8 | JWT | Funcional | Login com conta pendente (RF-02) |
| CT-02 | #9 | JWT | API | Acesso a endpoint restrito sem privilégio (RF-03) |
| CT-03 | #10 | Check-in | Funcional | Envio de foto repetida (RF-04) |
| CT-04 | #11 | Check-in | API | Validação do status inicial PENDING (RF-05) |
| CT-05 | #12 | Pontos | Funcional | Atualização automática de pontos (RF-07) |
| CT-06 | #13 | Pontos | API | Subtração de saldo ao deletar check-in (RF-08) |
| CT-07 | #14 | JWT | Funcional | Ativação de conta com código inválido (RF-01) |
| CT-08 | #15 | JWT | API | Validação de unicidade de e-mail no cadastro (RF-01) |
| CT-09 | #16 | JWT | API | Bloqueio de accessToken para conta não verificada (RF-02) |
| CT-10 | #17 | JWT | Funcional | Acesso forçado via URL às telas de ADMIN por MEMBER/TRAINEE (RF-03) |
| CT-11 | #18 | Check-in | API | Rejeição de formato de arquivo inválido (RF-04) |
| CT-12 | #19 | Check-in | Funcional | Usuário comum tentando alterar status do check-in (RF-05) |
| CT-13 | #20 | Check-in | Funcional | Visualização ou exclusão de check-in alheio pela interface (RF-06) |
| CT-14 | #21 | Check-in | API | Isolamento na listagem de check-ins próprios (RF-06) |
| CT-15 | #22 | Pontos | API | Dupla aprovação ou soma indevida em check-in pendente/rejeitado (RF-07) |
| CT-16 | #23 | Pontos | Funcional | Subtração indevida de saldo ao excluir check-in PENDING (RF-08) |
| CT-18 | #25 | Pontos | API | Burla da janela de 24h entre check-ins na virada do dia (RF-09) |
| CT-19 | #26 | JWT | Funcional | Registro, envio de código e ativação (RF-01) |
| CT-20 | #27 | JWT | API | Acesso de ADMINISTRADOR a endpoint de moderação (RF-03) |
| CT-21 | #28 | Check-in | Funcional | Moderação e alteração de status por ADMINISTRADOR (RF-05) |
| CT-22 | #29 | Pontos | API | Soma de pontos (RF-07) |
| CT-23 | #30 | JWT | Funcional (Ad-hoc) | Validação de inputs no formulário de registro (RF-01) |
| CT-24 | #39 | Pontos | Funcional | Ordenação decrescente de usuários no ranking (RF-09) |
| CT-25 | #41 | Check-in | API (Ad-hoc) | Isolamento de dados via acesso direto por ID (RF-06) |
| CT-26 | #34 | Check-in | Funcional | Limite de intervalo menor que 24h (RF-04) |
| CT-27 | #45 | Pontos | API | Atribuição de pontos com check-in PENDING (RF-05 / RF-07) |
| CT-28 | #46 | Pontos | API (Ad-hoc) | Acesso ao ranking com perfil TRAINEE (RF-09) |
| CT-29 | #48 | JWT | API | Hashing de senha no banco após cadastro (RNF-01) |
| CT-30 | #49 | JWT | API | Omissão do campo de senha nas consultas de usuário (RNF-01) |
| CT-31 | #50 | Check-in | API | Remoção automática do arquivo temporário após upload (RNF-02) |
| CT-32 | #51 | Check-in | API | Limpeza da pasta temporária ao recusar arquivo inválido (RNF-02) |
| CT-33 | #`__` | JWT | Regressão | Regressão do fluxo de registro e verificação de conta |
| CT-34 | #`__` | Check-in | Regressão | Regressão do fluxo de criação e moderação |
| CT-35 | #`__` | Pontos | Regressão | Regressão da trava de 24h entre check-ins |

### 10.2. Bug Reports

| ID | Issue | Módulo | Severidade | Descrição |
|---|---|---|---|---|
| BUG-01 | #31 | JWT | Médio | Falta de validação de limite mínimo (nome/senha) no cadastro e erro genérico na interface |
| BUG-02 | #32 | JWT | Crítico | E-mail de verificação não gera token JWT |
| BUG-03 | #33 | JWT | Crítico | Ausência do estado PENDING na tabela `User` (relacionado ao BUG-02) |
| BUG-04 | #35 | Check-in | Médio | Sistema permite envio de foto repetida (RF-04) |
| BUG-05 | #36 | Check-in | Maior | Permite múltiplos check-ins consecutivos em menos de 24h (RF-04) |
| BUG-06 | #37 | Check-in | Maior | Novos check-ins criados como aprovados automaticamente (RF-05) |
| BUG-07 | #38 | Check-in | Maior | Exposição de check-ins de terceiros na tela inicial para perfis restritos (RF-06) |
| BUG-08 | #40 | Check-in | Maior | Criação de check-in (API) ignora status PENDING e aprova automaticamente (RF-05) |
| BUG-09 | #42 | Check-in | Maior | Falha de isolamento de dados (API) no acesso a check-ins |
| BUG-10 | #43 | Check-in | Menor | Retorna 500 ao enviar formato de arquivo inválido |
| BUG-11 | #44 | Pontos | Maior | Falha na restrição de intervalo de 24h para múltiplos check-ins (RF-09) |
| BUG-12 | #47 | Pontos | Médio | Quebra de RBAC: perfil TRAINEE acessa a rota de ranking via API (RF-09) |
