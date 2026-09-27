# Plano de Testes — Parte 1.2

## 1. Módulo escolhido

**Módulo:** `flaskbb/forum/`

O módulo foi escolhido por possuir funcionalidades relacionadas à criação e atualização de tópicos, gerenciamento de postagens e controle de leitura dos usuários.

## 2. Cobertura inicial

A cobertura inicial foi coletada antes das alterações e refatorações, considerando os módulos analisados no projeto.

| Módulo     | Linhas cobertas | Cobertura de linhas | Branches cobertos | Cobertura de branches |
| ---------- | --------------: | ------------------: | ----------------: | --------------------: |
| Forum      |        417/1272 |              32,78% |           110/284 |                38,73% |


## 3. Meta de cobertura

Aumentar a cobertura de linhas do módulo `forum` em pelo menos 15 pontos percentuais, passando de 32,78% para, no mínimo, 47,78%.

## 4. Cenários inicialmente identificados

| Nº | Cenário                                                                                                                          | Tipo       |
| -- | -------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| 1  | Verificar se a criação de um tópico salva corretamente as informações e atualiza os dados relacionados ao fórum.                 | Happy path |
| 2  | Verificar se a criação de um tópico salva corretamente o primeiro post e atualiza a quantidade de tópicos do fórum.              | Happy path |
| 3  | Verificar se a atualização das informações do último post registra corretamente o post, o autor, o título e a data.              | Happy path |
| 4  | Verificar se as informações do último post são atualizadas corretamente quando o autor do post é diferente do usuário informado. | Limite     |
| 5  | Verificar se a criação de um tópico utiliza corretamente diferentes títulos e mantém o vínculo com o primeiro post.              | Happy path |
| 6  | Verificar se o salvamento de um tópico existente mantém as alterações realizadas.                                                | Happy path |
| 7  | Verificar o comportamento da criação de um tópico quando o usuário ou o fórum não são informados.                                | Erro       |

Os cenários foram utilizados como base para a implementação dos novos testes unitários no módulo `forum/`, incluindo parametrização e uso de dublê para verificar as interações durante a criação de tópicos.
