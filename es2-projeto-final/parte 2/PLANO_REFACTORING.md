# Plano de Refatoração

## 2. Smells selecionados

### 2.1. Long Method — `Topic.save`

* **Localização:** `flaskbb/forum/models.py`, método `Topic.save` (aproximadamente linhas 829–881).
* **Refatoração de Fowler:** Extract Method.
* **Proposta:** Extrair a lógica de criação de um tópico para um método auxiliar, separando-a da lógica de atualização de um tópico existente.
* **Resultado esperado:** Reduzir a complexidade do método `save`, facilitando a leitura, os testes e a manutenção.
* **Riscos antecipados:** Alterar a ordem das operações no banco de dados ou dos eventos do Pluggy pode afetar a criação e a atualização de tópicos.

### 2.2. Long Method — `Forum.get_topics`

* **Localização:** `flaskbb/forum/models.py`, método `Forum.get_topics` (aproximadamente linhas 1403–1467).
* **Refatoração de Fowler:** Extract Method.
* **Proposta:** Extrair a construção das consultas para métodos auxiliares, separando o tratamento dos usuários autenticados e não autenticados.
* **Resultado esperado:** Tornar o método mais simples de compreender, isolando a montagem das consultas e facilitando sua manutenção.
* **Riscos antecipados:** Alterações nos filtros, nas junções ou na paginação podem modificar os tópicos retornados e afetar a exibição do fórum.

### 2.3. Dead Code — `Topic.update_read`

* **Localização:** `flaskbb/forum/models.py`, método `Topic.update_read` (aproximadamente linhas 763–782).
* **Refatoração de Fowler:** Inline Method / simplificação da estrutura condicional.
* **Proposta:** Remover o bloco `else` inalcançável após as condições `if topicsread` e `elif not topicsread`, simplificando a estrutura condicional.
* **Resultado esperado:** Eliminar código desnecessário e deixar explícitos os caminhos efetivamente executados pelo método.
* **Riscos antecipados:** A remoção deve preservar o valor final de `updated` e a chamada a `forum.update_read`, evitando alterações no registro de leitura dos tópicos.

### 2.4. Duplicated Code — Lógica de persistência em `Topic.save` e `Post.save`

* **Localização:** `flaskbb/forum/models.py`, métodos `Topic.save` (aproximadamente linhas 829–881) e `Post.save` (aproximadamente linhas 294–340).
* **Refatoração de Fowler:** Extract Method.
* **Proposta:** Avaliar a extração de operações de persistência que sejam comprovadamente duplicadas nos dois métodos, mantendo separadas as regras específicas de cada entidade.
* **Resultado esperado:** Reduzir a repetição de código compartilhado, facilitando futuras alterações na lógica comum de persistência.
* **Riscos antecipados:** A extração indevida pode misturar regras específicas de tópicos e publicações, afetando contadores, relacionamentos, eventos ou transações no banco de dados.

