# Coleção de API (Insomnia) — NaSalinha

Esta pasta contém a coleção de testes de API da auditoria do NaSalinha, exportada do **Insomnia**.

## Arquivos

| Arquivo | Formato | Para que serve |
|---|---|---|
| `NaSalinha.yaml` | Insomnia v5 (YAML) | **Coleção principal.** Formato nativo exportado pelo Insomnia; é o recomendado para importar |
| `NaSalinha.json` | JSON | Cópia da mesma coleção em JSON, conforme exige o desafio |
| `My-Enviroment.yaml` | Insomnia v5 (YAML) | Ambiente com a variável `base_url` |

### Por que existem dois formatos da coleção?

O desafio pede o arquivo da coleção em **JSON**. As versões atuais do Insomnia (v5) **não exportam mais em JSON**: as opções de exportação são apenas o formato próprio do Insomnia v5 (YAML) e HAR (um formato de captura de tráfego, que não preserva a organização de pastas nem os ambientes).

Por isso:

* o **YAML** é o arquivo exportado diretamente pelo Insomnia, sem alterações;
* o **JSON** foi gerado a partir do YAML por conversão direta (o YAML é compatível com JSON), mantendo a mesma estrutura: pastas, requisições, passos e ordem.

As duas versões têm o mesmo conteúdo. Se o seu Insomnia não importar o JSON, use o YAML.

## Como importar

1. Abra o Insomnia e use a opção **Import**, escolhendo importar a partir de um arquivo (*From File*).
2. Importe a coleção: `NaSalinha.yaml` (ou `NaSalinha.json`).
3. Importe o ambiente: `My-Enviroment.yaml`.
4. Confirme que o ambiente importado está selecionado no gerenciador de ambientes (canto superior direito) antes de enviar as requisições.

> **Se o ambiente não funcionar** (URL inválida ou variável `base_url` vazia), configure manualmente: no gerenciador de ambientes, crie ou edite o ambiente com o JSON abaixo, salve e selecione-o.
> ```json
> {
>   "base_url": "http://localhost:5001"
> }
> ```

> Antes dos testes, o sistema precisa estar rodando. As instruções estão no [README principal](../README.md#6-como-executar-o-projeto).

## Organização da coleção

* **Pasta `Logins`:** autenticação rápida. Selecione o perfil (`Admin`, `Member` ou `Trainee`) e envie a requisição (**Send**) antes dos testes que exigem login.
* **Pastas por módulo:** `Autenticação JWT`, `Check-in` e `Pontos`.
* **Uma pasta por Caso de Teste de API (`CT-XX`):** por exemplo `CT-06`, `CT-18` e `CT-22`. Algumas têm uma única requisição; outras têm requisições numeradas (`Passo 1`, `Passo 2`, `Passo 3`).
* A issue de cada CT no GitHub detalha quais passos executar e o resultado esperado.

## Testes com upload de arquivo (Check-in)

A exportação do Insomnia não embute arquivos, então o campo `photo` das requisições de Check-in vem sem imagem. Imagens de exemplo estão na pasta `/assets` do repositório. Para anexá-las:

1. **Permissão de acesso a arquivos:** se aparecer o erro *Restricted File Access*, abra as configurações do Insomnia (**Preferences**), procure as opções de acesso a arquivos (aba **General** ou Security/Data, conforme a versão) e permita a leitura da pasta `/assets` (ou do diretório onde o projeto foi clonado).
2. **Anexar a imagem:** abra a requisição de Check-in, vá até a aba **Body** (*Multipart Form*), localize o campo `photo`, mude o tipo do valor de `Text` para **`File`**, clique em **Choose File** e selecione a imagem.
