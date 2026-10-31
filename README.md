# Auditoria de Qualidade - NaSalinha

**Descrição:** Repositório ("Portfólio de Qualidade") destinado à auditoria completa de qualidade no projeto NaSalinha, com o objetivo primordial de identificar, documentar e reportar falhas de software, garantindo a excelência da experiência do utilizador.

## Tecnologias e Ferramentas
- **Docker:** Utilizado para a orquestração e configuração do ambiente local da aplicação.
- **Navegador Web:** Ferramenta central para a execução dos testes manuais de interface e acompanhamento do fluxo do utilizador.
- **Insomnia:** Utilizado para a validação técnica dos endpoints e análise de respostas do servidor na API.
- **GitHub Projects e Issues:** Ferramentas escolhidas para o Kanban, gestão visual dos Casos de Teste e rastreabilidade dos Bug Reports.

## Tipos e Estratégia de Testes
A gestão de todos os artefatos de qualidade seguirá a abordagem da **Opção A (Integrada)**. Os Casos de Teste e o acompanhamento (Kanban) estão estruturados no GitHub Projects, enquanto as falhas encontradas serão documentadas detalhadamente via GitHub Issues. O repositório também inclui a Coleção de API exportada do Insomnia em formato `.json`.

A estratégia de execução está focada na avaliação profunda das três funcionalidades Core do sistema:
1. Autenticação JWT e Gestão de Acesso.
2. Check-in por Foto e Validação de Mídia.
3. Sistema de Pontos, Cálculo e Persistência de Dados.

Durante as rondas de auditoria, a cobertura de testes contemplará obrigatoriamente:
- **Foco Funcional:** Validação rigorosa do comportamento visual e interativo na interface face aos resultados esperados.
- **Validação de API:** Testes estruturais aos endpoints, verificando o tratamento de dados e a exatidão dos Status Codes (ex: 200, 201, 400, 401, 403, 404, 500).
- **Teste de Regressão:** Simulações destinadas a garantir que novas correções de bugs não comprometem as funcionalidades previamente estáveis no fluxo principal.

## Como rodar o projeto
A execução desta auditoria requer que a API e o banco de dados do projeto alvo (NaSalinha) estejam em funcionamento. É estritamente necessário subir o ambiente local do sistema utilizando o Docker.

Os comandos exatos para iniciar os contentores (ex: `docker-compose up`) encontram-se descritos na documentação oficial do repositório da aplicação NaSalinha. Caso o limite de requisições (Rate Limiting) seja atingido durante os testes automatizados via Insomnia, utilize o comando `docker-compose restart backend` no terminal para resetar o contador.
