# Relatório de Testes Final — NaSalinha


| Item | Informação |
|---|---|
| **Projeto** | NaSalinha (Sistema de Check-in Gamificado) |
| **Responsável (QA)** | `_João Vitor Maximiano Vieira` |
| **Período da auditoria** | `04/10/2026` a `07/11/2026` |
| **Ambiente** | Docker Compose local (frontend `localhost:3000`, API `localhost:5001`) |
| **Versão testada** | Commit `2` do repositório `nasalinha-qa-challenge` |
| **Ferramentas** | Navegador (DevTools), Insomnia, logs do Docker, Prisma Studio, Mailtrap |
| **Navegador utilizado** | `Chrome` |

---

## 1. Resumo executivo

| Métrica | Valor |
|---|---|
| Casos de Teste planejados | 34 (31 executáveis + 3 de regressão simulada) |
| Casos de Teste executados | `31` |
| **Passou** | `14` |
| **Falhou** | `12` |
| **Impedido** | `4` |
| Taxa de aprovação (Passou ÷ Executados) | `45`% |
| Regressão simulada (documentada, fora do Passou/Falhou) | 3 |
| Bugs encontrados | 12 |
| **Status final** | **Reprovado (aguardando correções)** |

### Critério para o status final

| Status | Condição |
|---|---|
| **Aprovado** | Nenhum bug aberto de severidade Crítica, Maior ou Média |
| **Aprovado com ressalvas** | Nenhum bug Crítico ou Maior aberto, apenas Médio e/ou Menor |
| **Reprovado (aguardando correções)** | Pelo menos um bug Crítico ou Maior aberto |

**Justificativa:** existem 2 bugs Críticos (BUG-02 e BUG-03) e 6 bugs Maiores em aberto, o que enquadra o sistema como reprovado.

---

## 2. Métricas de execução

### 2.1. Por módulo

| Módulo | Planejados | Executados | Passou | Falhou | Impedido |
|---|---|---|---|---|---|
| Autenticação JWT | 11 | `9` | `6` | `3` | `2` |
| Check-in por Foto | 11 | `11` | `7` | `4` | `0` |
| Sistema de Pontos | 9 | `7` | `4` | `2` | `2` |
| Regressão (simulada) | 3 | simulado | — | — | — |
| **Total** | **34** | `27` | `14` | `12` | `4` |

### 2.2. Por tipo de teste

| Tipo | Planejados | Passou | Falhou | Impedido |
|---|---|---|---|---|
| Funcional (interface) | 13 | `4` | `7` | `2` |
| API (Insomnia) | 18 | `10` | `5` | `2` |
| Regressão (simulada) | 3 | — | — | — |
| *Ad-hoc (já contados nos tipos acima)* | *3* | `0` | `3` | `0` |

> **Impedido** significa que o teste não pôde ser executado por causa de um bug anterior. Esses testes **não são contados como falha**.
> A **regressão** é uma simulação documentada (nenhuma correção real foi feita), por isso não entra na contagem de Passou/Falhou.

---

## 3. Distribuição de severidade dos bugs

### 3.1. Visão geral

| Severidade | Descrição | Quantidade | % |
|---|---|---|---|
| **Crítico** | Trava o sistema, impede um fluxo principal ou é falha grave de segurança | 2 | 16,7% |
| **Maior** | Quebra de regra de negócio ou erro de lógica | 6 | 50% |
| **Médio** | Comportamento anômalo parcial ou resposta HTTP fora do padrão | 3 | 25% |
| **Menor** | Falha visual ou de usabilidade | 1 | 8,3% |
| **Total** | | **12** | 100% |

> O desafio prevê Crítico, Maior e Menor. A categoria **Médio** foi adicionada para separar defeitos de impacto intermediário.

### 3.2. Severidade por módulo

| Módulo | Crítico | Maior | Médio | Menor | Total |
|---|---|---|---|---|---|
| Autenticação JWT | 2 | 0 | 1 | 0 | 3 |
| Check-in por Foto | 0 | 5 | 1 | 1 | 7 |
| Sistema de Pontos | 0 | 1 | 1 | 0 | 2 |
| **Total** | **2** | **6** | **3** | **1** | **12** |

---

## 4. Bugs encontrados

| ID | Issue | Módulo | Severidade | Descrição | Status |
|---|---|---|---|---|---|
| BUG-01 | #31 | JWT | Médio | Falta de validação de limite mínimo (nome/senha) no cadastro e erro genérico na interface | Não corrigido |
| BUG-02 | #32 | JWT | Crítico | E-mail de verificação não gera token JWT | Aguardando reteste (simulado) |
| BUG-03 | #33 | JWT | Crítico | Ausência do estado PENDING na tabela `User` | Aguardando reteste (simulado) |
| BUG-04 | #35 | Check-in | Médio | Sistema permite envio de foto repetida | Não corrigido |
| BUG-05 | #36 | Check-in | Maior | Permite múltiplos check-ins consecutivos em menos de 24h | Aguardando reteste (simulado) |
| BUG-06 | #37 | Check-in | Maior | Novos check-ins criados como aprovados automaticamente | Aguardando reteste (simulado) |
| BUG-07 | #38 | Check-in | Maior | Exposição de check-ins de terceiros na tela inicial para perfis restritos | Não corrigido |
| BUG-08 | #40 | Check-in | Maior | Criação de check-in (API) ignora status PENDING e aprova automaticamente | Aguardando reteste (simulado) |
| BUG-09 | #42 | Check-in | Maior | Falha de isolamento de dados (API) no acesso a check-ins | Não corrigido |
| BUG-10 | #43 | Check-in | Menor | Retorna 500 ao enviar formato de arquivo inválido | Não corrigido |
| BUG-11 | #44 | Pontos | Maior | Falha na restrição de intervalo de 24h para múltiplos check-ins | Aguardando reteste (simulado) |
| BUG-12 | #47 | Pontos | Médio | Quebra de RBAC: perfil TRAINEE acessa a rota de ranking via API | Não corrigido |

---

## 5. Riscos, limitações e observações

* **Regressão simulada:** os três casos de regressão são documentais. Os bugs de referência seguem abertos e a eficácia real das correções não foi verificada.
* **Testes impedidos:** `4` casos não puderam ser executados por dependerem de funcionalidades com defeito (CT-01, CT-09, CT-16 e CT-27). Eles devem ser executados assim que os bugs forem corrigidos.
* **Cobertura:** a auditoria cobre somente as 3 áreas Core.
* **Ambiente:** testes realizados em ambiente local com serviços externos (Mailtrap e Cloudinary) em contas de teste.
* **Observações adicionais:** ``

---

## 6. Conclusão e recomendação

**Status final: Reprovado (aguardando correções).**

A auditoria de qualidade aponta que o sistema NaSalinha ainda não está apto para liberação em produção devido a falhas críticas de arquitetura e segurança, sendo o módulo de Check-in a área mais afetada por quebras de regras de negócio e exposição indevida de dados (IDOR). 
Antes de qualquer deploy, é mandatório corrigir os bloqueadores do fluxo de autenticação (BUG-02 e BUG-03), a aprovação automática de envios sem moderação (BUG-06 e BUG-08), a ausência de trava temporal de 24 horas (BUG-05 e BUG-11) e as falhas de controle de acesso e isolamento na API (BUG-07, BUG-09 e BUG-12). 
Como próximos passos, recomenda-se a refatoração imediata desses pontos pela equipe de desenvolvimento, seguida pelo reteste das Issues corrigidas, o desbloqueio e execução dos casos de teste atualmente Impedidos e a execução real da bateria de testes de regressão (CT-33, CT-34 e CT-35).

---
