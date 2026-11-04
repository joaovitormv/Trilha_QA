# Portfólio de Qualidade (QA) — Auditoria do Sistema NaSalinha

Bem-vindo(a) ao repositório de auditoria de qualidade do projeto **NaSalinha** (Sistema de Check-in Gamificado), desenvolvido para a trilha de **Quality Assurance (QA) da Comp Júnior**.

Este repositório adota a **Opção A (Documentação Integrada)**, centralizando o planejamento, a execução, a rastreabilidade de Casos de Teste e os Bug Reports diretamente no **GitHub Projects** e nas **GitHub Issues**, acompanhados pela coleção exportada do **Insomnia**.

## 1. Escopo da Auditoria e Estratégia de Testes

A auditoria avalia a experiência do usuário, o cumprimento das regras de negócio e a estabilidade e segurança da API nas **3 Funcionalidades Core** do sistema:

1. **Autenticação JWT:** Registro, login, validade de sessão e controle de acesso baseado em cargos (RBAC: Admin, Member e Trainee).
2. **Check-in por Foto:** Upload de comprovantes de presença, validação de formato/tamanho de mídia, restrição temporal (1 check-in a cada 24h) e isolamento de dados entre usuários.
3. **Sistema de Pontos:** Cálculo automático de pontuação na aprovação ou exclusão de check-ins, persistência íntegra no banco de dados e reflexo no ranking da temporada ativa.

**Tipos de Testes Aplicados:**
* **Testes Funcionais (UI):** Validação do comportamento visual, fluxos de interface e feedback ao usuário no Front-end.
* **Testes de API e Integração:** Validação direta nos endpoints REST (payloads, contratos, Status Codes HTTP, regras de negócio e segurança/IDOR) via Insomnia.
* **Testes Ad-hoc (Exploratórios):** Cenários extras documentados a partir de falhas e comportamentos anômalos descobertos fora do escopo inicial planejado.
* **Testes de Regressão:** Mapeamento de baterias de re-teste para garantir que a correção dos defeitos reportados não introduza efeitos colaterais nos fluxos principais.

## 2. Tecnologias e Ferramentas Utilizadas

* **Infraestrutura e Ambiente Local:** Docker, Docker Compose, Node.js, Prisma ORM e Git.
* **Testes de API:** Insomnia.
* **Testes de Interface e Inspeção:** Navegador Web (DevTools) e monitoramento de logs dos containers Docker.
* **Gestão de Testes e Defeitos:** GitHub Projects (Kanban) e GitHub Issues.

## 3. Como Executar o Ambiente sob Teste

### 3.1. Pré-requisitos
Certifique-se de ter o **Docker** (com Docker Compose) e o **Git** instalados na máquina.

### 3.2. Configuração de Variáveis de Ambiente
1. Acesse o diretório `/backend` e crie o arquivo `.env` a partir do modelo `.env.example`:
   ```bash
   cp backend/.env.example backend/.env
   ```
2. **Configuração Obrigatória de Serviços Externos:** Crie contas gratuitas nos serviços abaixo e preencha as respectivas chaves no arquivo `backend/.env`:
   * **E-mails:** Utilize o **Mailtrap** (Sandbox) para captura de e-mails de recuperação de senha.
   * **Armazenamento de Fotos:** Utilize o **Cloudinary** para o processamento de uploads de imagens.
   * Defina a URL do cliente no arquivo `.env`:
     ```env
     FRONTEND_URL=http://localhost:3000
     ```
   *Nota de Auditoria:* A distinção entre falhas de configuração de ambiente (variáveis ausentes/inválidas) e defeitos reais de código fez parte do escopo de triagem desta auditoria.

### 3.3. Inicialização com Docker
1. Na raiz do projeto, construa e inicie os containers:
   ```bash
   sudo docker compose up --build
   ```
2. Com os containers em execução, aplique as *migrations* e popule o banco de dados (*seed*) executando:
   ```bash
   sudo docker compose exec backend npx prisma migrate dev --name init
   sudo docker compose exec backend npx prisma db seed
   ```
3. Endpoints locais disponíveis:
   * **Aplicação Web (Front-end):** `http://localhost:3000`
   * **Servidor da API (Back-end):** `http://localhost:5001/api`

## 4. Arquitetura do GitHub Projects e Issues (Guia para o Avaliador)

Toda a rastreabilidade entre requisitos, casos de teste e defeitos está estruturada no **GitHub Projects** vinculado a este repositório, dividido em duas visualizações (*Views*) específicas:

### 4.1. Views e Fluxo de Status
* **View `CTs` (Gestão de Casos de Teste):**
  * **Planejado:** Caso de teste modelado aguardando execução.
  * **Em execução:** Cenário atualmente em validação na UI ou no Insomnia.
  * **Concluído:** O sistema comportou-se exatamente conforme o resultado esperado (*Pass*).
  * **Falhou:** O sistema violou a regra de negócio, contrato da API ou comportamento esperado (*Fail*), originando um Bug Report vinculado.
  * **Impedido:** Execução bloqueada (*Blocked*) por um defeito anterior de rota, infraestrutura ou dependência que impede alcançar a etapa de validação.
* **View `Bugs` (Rastreamento de Defeitos):**
  * **Não corrigido:** Defeito identificado, documentado com Análise de Causa Raiz (RCA) e aguardando correção de desenvolvimento.
  * **Corrigido:** Defeito solucionado e validado em re-teste.

### 4.2. Padrão de Labels (Tags)
As *Issues* são classificadas combinando etiquetas de módulo, natureza do teste e severidade:
* **Módulos (Áreas Core):**
  * `jwt` — Autenticação e controle de acesso.
  * `checkin` — Upload de mídia e gestão de check-ins.
  * `pontos` — Cálculo de saldo, temporadas e ranking.
* **Natureza / Tipo de Teste:**
  * `funcional` — Testes executados via interface gráfica (Front-end).
  * `api` — Testes executados diretamente nos endpoints HTTP (Insomnia).
  * `regressao` — Cenários de garantia de estabilidade pós-correção.
  * `ad-hoc` — Casos de teste extras derivados de testes exploratórios.
* **Severidade de Bugs:**
  * `critico` — Falhas graves de segurança (ex: quebra de isolamento/IDOR), perda de dados ou indisponibilidade total da funcionalidade.
  * `maior` — Quebra direta de regra de negócio central (ex: falha no bloqueio de múltiplos check-ins diários ou erro no cálculo de pontos).
  * `medio` — Comportamentos anômalos parciais ou respostas HTTP fora do padrão que afetam a integração sem comprometer o banco de dados.
  * `menor` — Falhas visuais de interface, textos desalinhados ou erros de usabilidade sem impacto funcional.

## 5. Arquitetura da Coleção do Insomnia (Testes de API)

O arquivo JSON exportado do Insomnia encontra-se na raiz/pasta do repositório em:
* **Arquivo:** `<PREENCHER_CAMINHO_DO_ARQUIVO_JSON_QUANDO_EXPORTAR>`

Para garantir reprodução imediata durante a avaliação sem necessidade de reconfigurar parâmetros manualmente, o workspace do Insomnia (`NaSalinha`) foi organizado no seguinte padrão:

1. **Pasta `Logins`:** Contém as requisições `Login Trainee` (usuário comum) e `Login Admin`. Basta disparar a requisição desejada para obter o `accessToken` atualizado para os testes.
2. **Pastas de Módulos (`Autenticação JWT`, `Check-in`, `Pontos`):** Agrupam os testes de acordo com a respectiva funcionalidade Core.
3. **Casos de Teste de Disparo Único (`CT-XX Nome do Cenário`):** Quando o caso de teste exige apenas uma chamada HTTP (ex: `CT-04`, `CT-11`, `CT-14`, `CT-25`), a requisição fica diretamente dentro da pasta do módulo, pré-configurada com o método, URL, headers e payload do teste.
4. **Pastas de Casos de Teste Multi-etapas (`📁 CT-XX`):** Para cenários que exigem verificar o estado inicial, executar uma mutação e validar o estado final (como `CT-06` e `CT-22` em Pontos) ou disparos repetidos (`CT-18`), foi criada uma subpasta com o código do CT contendo as requisições ordenadas (`Passo 1`, `Passo 2`, `Passo 3`), permitindo ao avaliador executar o fluxo completo em sequência.

## 6. Planejamento de Testes de Regressão

Conforme os requisitos da auditoria, abaixo estão descritos os cenários simulados de regressão para garantir que a correção dos defeitos reportados não introduza efeitos colaterais nos fluxos principais:

* **1. Módulo Autenticação JWT:**
  * *Cenário Simulado:* `<PREENCHER: Caso Ex: X autenticação/permissão bug corrigido... na o seja>`
  * *Bateria de Testes a Repetir:* `<PREENCHER: CT-02, CT-08, CT-20 CTs Listar ex: os relacionados,>`
* **2. Módulo Check-in por Foto:**
  * *Cenário Simulado:* `<PREENCHER: (CT-11) (CT-14) Caso Ex: IDOR a arquivo code corrigidos... de e falha inválido isolamento o sejam status>`
  * *Bateria de Testes a Repetir:* `<PREENCHER: CT-04, CT-11, CT-14, CT-25 CTs Listar ex: os relacionados,>`
* **3. Módulo Sistema de Pontos:**
  * *Cenário Simulado:* `<PREENCHER: (CT-18) 24h Caso Ex: a check-ins corrigidos... cálculo de e entre o pontos restrição sejam>`
  * *Bateria de Testes a Repetir:* `<PREENCHER: CT-06, CT-18, CT-22 CTs Listar ex: os relacionados,>`

## 7. Relatório de Testes Final (Visão Geral e Métricas)

Resumo consolidado da qualidade do sistema **NaSalinha** após a execução da bateria de testes:

### 7.1. Métricas de Execução de Casos de Teste
* **Total de Casos de Teste Planejados:** `<PREENCHER>` (Incluindo `<PREENCHER>` casos Ad-hoc extras)
* **Total de Casos de Teste Executados:** `<PREENCHER>`
* **Concluídos com Sucesso (Pass):** `<PREENCHER>`
* **Falharam (Fail):** `<PREENCHER>`
* **Impedidos (Blocked):** `<PREENCHER>`

### 7.2. Distribuição de Bugs por Severidade
* **Total de Bugs Reportados:** `<PREENCHER>`
  * **Críticos (`critico`):** `<PREENCHER>`
  * **Maiores (`maior`):** `<PREENCHER>`
  * **Médios (`medio`):** `<PREENCHER>`
  * **Menores (`menor`):** `<PREENCHER>`

### 7.3. Parecer de Qualidade
`<PREENCHER: (IDOR), 24h API, Breve Postman a antes ausência check-ins, como conclusão correção da dados de deploy divergências do documentação e em encontrados, entre falha isolamento múltiplos os para parágrafo principais produção. reais recomendação resumindo riscos rotas trava>`
