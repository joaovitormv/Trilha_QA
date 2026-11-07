# Portfólio de Qualidade (QA) — Auditoria do Sistema NaSalinha

Bem-vindo(a) ao repositório de auditoria de qualidade do projeto **NaSalinha** (Sistema de Check-in Gamificado), desenvolvido para a trilha de **Quality Assurance (QA) da Comp Júnior**.

Este repositório adota a **Opção A (Documentação Integrada)**, centralizando o planejamento, a execução, a rastreabilidade de Casos de Teste e os Bug Reports diretamente no **GitHub Projects** e nas **GitHub Issues**, acompanhados pela coleção exportada do **Insomnia**.

## 1. Escopo da Auditoria

A auditoria avalia a experiência do usuário, o cumprimento das regras de negócio e a estabilidade e segurança da API nas **3 Funcionalidades Core** do sistema:

1. **Autenticação JWT:** Registro, login, validade de sessão e controle de acesso baseado em cargos (RBAC: Admin, Member e Trainee).
2. **Check-in por Foto:** Upload de comprovantes de presença, validação de formato/tamanho de mídia, restrição temporal (1 check-in a cada 24h) e isolamento de dados entre usuários.
3. **Sistema de Pontos:** Cálculo automático de pontuação na aprovação ou exclusão de check-ins, persistência íntegra no banco de dados e reflexo no ranking da temporada ativa.

## 2. Tecnologias e Ferramentas Utilizadas

- **Infraestrutura e Ambiente Local:** Docker, Docker Compose, Node.js, Prisma ORM e Git.
- **Testes de API:** Insomnia.
- **Testes de Interface e Inspeção:** Navegador Web (DevTools) e monitoramento de logs dos containers Docker.
- **Gestão de Testes e Defeitos:** GitHub Projects (Kanban) e GitHub Issues.

## 3. Como Executar o Projeto

### 3.1. Preparação
Certifique-se de ter o **Docker** e o **Git** instalados em sua máquina.

### 3.2. Configuração de Variáveis
1. Vá até a pasta `/backend` e copie o arquivo `.env.example` para um novo arquivo `.env`:
   ```bash
   cp backend/.env.example backend/.env
   ```
2. **Configuração de Chaves Obrigatória:** Para que o sistema funcione, você deve criar suas próprias contas (gratuitas) nos serviços abaixo e preencher as credenciais no seu `.env`:
   * **E-mails:** Utilize o **Mailtrap** (Sandbox).
   * **Fotos:** Utilize o **Cloudinary**.
   * Certifique-se também de definir a URL do frontend no `.env`:
     ```env
     FRONTEND_URL=http://localhost:3000
     ```

### 3.3. Execução com Docker
1. Na raiz do projeto, suba os containers:
   ```bash
   sudo docker compose up --build
   ```
2. Rode os comandos para executar as migrations e seeders do backend:
   ```bash
   sudo docker compose exec backend npx prisma migrate dev --name init
   sudo docker compose exec backend npx prisma db seed
   ```
3. Acesse os serviços:
   * **Frontend:** `http://localhost:3000`
   * **Backend (API):** `http://localhost:5001/api`

## 4. Padrões do GitHub Projects e Issues

Toda a rastreabilidade entre requisitos, casos de teste e defeitos está estruturada no **GitHub Projects** vinculado a este repositório, dividido em duas visualizações (*Views*):

### 4.1. Views e Fluxo de Status
- **View `CTs` (Gestão de Casos de Teste):**
  * **Planejado:** Caso de teste modelado aguardando execução.
  * **Em execução:** Cenário atualmente em validação na interface ou no Insomnia.
  * **Concluído:** O sistema comportou-se exatamente conforme o resultado esperado (*Pass*).
  * **Falhou:** O sistema violou a regra de negócio, contrato da API ou comportamento esperado (*Fail*), gerando um Bug Report.
  * **Impedido:** Execução bloqueada (*Blocked*) por um defeito anterior que impede alcançar a etapa de validação.
- **View `Bugs` (Rastreamento de Defeitos):**
  * **Não corrigido:** Defeito identificado, documentado e aguardando correção.
  * **Corrigido:** Defeito solucionado e validado em re-teste.

### 4.2. Padrão de Labels (Tags)
As *Issues* e *Cards* são classificados combinando etiquetas de módulo, tipo de teste e severidade:
- **Módulos:**
  * `jwt` — Autenticação e controle de acesso.
  * `checkin` — Upload de mídia e gestão de check-ins.
  * `pontos` — Cálculo de saldo, temporadas e ranking.
- **Tipo de Teste:**
  * `funcional` — Testes executados via interface gráfica (Front-end).
  * `api` — Testes executados diretamente nos endpoints HTTP (Insomnia).
  * `regressao` — Cenários de garantia de estabilidade pós-correção.
  * `ad-hoc` — Casos de teste extras descobertos de forma exploratória.
- **Severidade de Bugs:**
  * `critico` — Falhas graves de segurança (ex: quebra de isolamento/IDOR), perda de dados ou travamento do sistema.
  * `maior` — Quebra direta de regra de negócio ou erro de lógica central.
  * `medio` — Comportamentos anômalos parciais ou respostas HTTP fora do padrão (ex: Status Code incorreto) que não comprometem o banco de dados.
  * `menor` — Falhas visuais de interface ou erros de usabilidade sem impacto funcional.

## 5. Como Utilizar a Coleção do Insomnia (Testes de API)

O arquivo JSON exportado do Insomnia encontra-se no repositório em: `<CAMINHO_DO_ARQUIVO.json>`.

A coleção foi estruturada para que o avaliador possa reproduzir cada teste de API sem precisar montar requisições manualmente:

- **Autenticação Rápida (Pasta `Logins`):** Em determinados casos de teste de API, será solicitado realizar o login como administrador (`Admin`) ou como membro comum (`Trainee`/`Member`). Na pasta `Logins`, você faz isso automaticamente apenas selecionando o perfil correspondente e enviando a requisição (**Send**).
- **Um Caso de Teste por Pasta (`CT-XX`):** Dentro das pastas de cada módulo (`Autenticação JWT`, `Check-in`, `Pontos`), cada Caso de Teste de API é representado por uma pasta própria com o seu código (ex: `CT-06`, `CT-18`, `CT-22`).
- **Execução Guiada:** Algumas pastas de CT possuem apenas uma requisição pré-configurada, enquanto outras contam com várias requisições numeradas (`Passo 1`, `Passo 2`, `Passo 3`). O próprio card do Caso de Teste (ou Bug Report) no GitHub detalha exatamente quais passos e requisições da pasta devem ser executados para validar o cenário.

### 5.1. Configuração da Variável de Ambiente (`base_url`)
Ao exportar a coleção, o Insomnia preserva as requisições, mas **não salva o valor das variáveis de ambiente**. Antes de iniciar os testes, configure a URL base:
1. No canto superior esquerdo do Insomnia, clique no gerenciador de ambientes (geralmente escrito "Base Environment") e vá em **Manage Environments**.
2. Certifique-se de que o JSON do ambiente contenha a variável:
   ```json
   {
     "base_url": "http://localhost:5001"
   }
   ```

### 5.2. Upload de Arquivos e a pasta `/assets`
O formato de exportação JSON do Insomnia salva a estrutura da requisição, mas não embute arquivos físicos. Por isso, ao executar os testes de **Check-in**, o campo `photo` no *Multipart Form* não conterá o arquivo original. 

Disponibilizamos imagens de exemplo na pasta `/assets` deste repositório para facilitar os testes. Siga os passos abaixo para anexá-las no Insomnia:

**1. Permissão de Acesso a Arquivos (Erro de Segurança do Insomnia):**
Por padrão, o Insomnia bloqueia a leitura de arquivos locais nas requisições. Se você tentar enviar e receber um erro de bloqueio (*Restricted File Access*):
- Vá nas configurações do Insomnia (Ícone de engrenagem ou **Preferences**).
- Na aba **General** (ou Security/Data, dependendo da versão), procure pelas configurações de acesso a arquivos.
- Conceda permissão manualmente para que o aplicativo possa ler os arquivos da pasta `/assets` (ou do diretório onde você clonou o projeto).

**2. Como anexar a imagem na requisição pré-configurada:**
- Abra a requisição de Check-in e vá até a aba **Body** (que já estará formatada como *Multipart Form*).
- Localize a linha onde o campo **Name** é `photo`.
- Na coluna da direita (Value), clique na pequena seta ao lado do campo de texto e mude o tipo de `Text` para **`File`**.
- O botão **Choose File** aparecerá. Clique nele, navegue até a pasta `/assets` do repositório (ou qualquer pasta do seu PC) e selecione a imagem desejada.
