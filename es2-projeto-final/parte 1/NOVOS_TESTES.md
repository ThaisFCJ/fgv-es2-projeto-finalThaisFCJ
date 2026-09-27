# Tarefa 1.3 — Novos testes unitários

**Módulo escolhido:** `flaskbb/forum/`

**Arquivo de testes:** `tests/unit/forum/test_topic_tests.py`

| Caso de teste                                | Arquivo                                | Tipo  | Verificação principal                                                                                         |
| -------------------------------------------- | -------------------------------------- | ----- | ------------------------------------------------------------------------------------------------------------- |
| `test_updates_forum_last_post` — caso 1      | `tests/unit/forum/test_topic_tests.py` | Feliz | Atualiza as informações do último post no fórum.                                                              |
| `test_updates_forum_last_post` — caso 2      | `tests/unit/forum/test_topic_tests.py` | Feliz | Verifica a atualização com outro usuário e título.                                                            |
| `test_updates_forum_last_post` — caso 3      | `tests/unit/forum/test_topic_tests.py` | Feliz | Verifica a atualização com diferentes dados de entrada.                                                       |
| `test_updates_last_post_with_different_user` | `tests/unit/forum/test_topic_tests.py` | Borda | Confere se o autor do post e o usuário informado são tratados corretamente.                                   |
| `test_create_topic_saves_first_post`         | `tests/unit/forum/test_topic_tests.py` | Feliz | Verifica a criação do tópico, o salvamento do primeiro post e o incremento da quantidade de tópicos do fórum. |
| `test_topic_save_without_user_or_forum`      | `tests/unit/forum/test_topic_tests.py` | Erro  | Verifica o retorno quando os dados necessários para criar um tópico não são informados.                       |
| `test_topic_save_updates_existing_topic`     | `tests/unit/forum/test_topic_tests.py` | Feliz | Verifica a atualização e o salvamento de um tópico existente.                                                 |
| `test_topic_creation_sets_title` — caso 1    | `tests/unit/forum/test_topic_tests.py` | Feliz | Verifica a criação do tópico com o primeiro título parametrizado e o vínculo com o primeiro post.             |
| `test_topic_creation_sets_title` — caso 2    | `tests/unit/forum/test_topic_tests.py` | Feliz | Verifica a criação do tópico com o segundo título parametrizado e o vínculo com o primeiro post.              |

## Execução dos testes

Comando utilizado:

`pytest tests/unit/forum/test_topic_tests.py -v`


## 1.4 — Teste parametrizado

Foram implementados testes parametrizados utilizando `pytest.mark.parametrize` no arquivo `tests/unit/forum/test_topic_tests.py`.

A parametrização permite verificar o comportamento das funções com diferentes títulos e dados de entrada, evitando a repetição de código e ampliando a cobertura dos testes.

## 1.5 — Teste com dublê (mock)

O teste `test_create_topic_saves_first_post`, localizado em `tests/unit/forum/test_topic_tests.py`, utiliza `monkeypatch` para substituir temporariamente o método `save` do post por uma função que registra a chamada e executa o método original.

O recurso foi utilizado para verificar se o primeiro post é salvo durante a criação do tópico e se a operação recebe o usuário e o tópico corretos. Dessa forma, a interação é observada sem impedir a execução real do salvamento no banco de dados.
A verificação é realizada por meio da lista `calls`, garantindo que o método foi chamado com os argumentos esperados.
