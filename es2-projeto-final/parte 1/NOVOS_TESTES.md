# Tarefa 1.3 — Novos testes unitários

**Módulo escolhido:** `flaskbb/user/`

**Arquivo de testes:** `tests/unit/user/test_factories.py`

Foram adicionados 10 casos de teste, incluindo testes parametrizados, mocks e verificações das fábricas de formulários e handlers.

| Caso de teste                                                           | Arquivo                             | Tipo  | Verificação principal                                                |
| ----------------------------------------------------------------------- | ----------------------------------- | ----- | -------------------------------------------------------------------- |
| `test_settings_update_handler_factory_returns_handler`                  | `tests/unit/user/test_factories.py` | Feliz | Retorna o handler de configurações esperado.                         |
| `test_details_update_factory_returns_handler`                           | `tests/unit/user/test_factories.py` | Feliz | Retorna o handler de detalhes e chama o mock de validadores.         |
| `test_password_update_handler_returns_handler`                          | `tests/unit/user/test_factories.py` | Feliz | Retorna o handler de senha esperado.                                 |
| `test_email_update_handler_returns_handler`                             | `tests/unit/user/test_factories.py` | Feliz | Retorna o handler de e-mail esperado.                                |
| `test_settings_form_factory_adds_default_theme`                         | `tests/unit/user/test_factories.py` | Borda | Inclui a opção de tema padrão nas escolhas.                          |
| `test_settings_form_factory_loads_available_languages`                  | `tests/unit/user/test_factories.py` | Feliz | Carrega os idiomas disponibilizados pela dependência.                |
| `test_settings_form_factory_uses_current_settings_when_needed` — caso 1 | `tests/unit/user/test_factories.py` | Borda | Formulário não enviado e inválido: recupera as configurações atuais. |
| `test_settings_form_factory_uses_current_settings_when_needed` — caso 2 | `tests/unit/user/test_factories.py` | Borda | Formulário não enviado e válido: recupera as configurações atuais.   |
| `test_settings_form_factory_uses_current_settings_when_needed` — caso 3 | `tests/unit/user/test_factories.py` | Erro  | Formulário enviado e inválido: recupera as configurações atuais.     |
| `test_settings_form_factory_uses_current_settings_when_needed` — caso 4 | `tests/unit/user/test_factories.py` | Feliz | Formulário enviado e válido: não substitui os dados validados.       |

## Execução dos testes

Comando utilizado:

`uv run pytest tests/unit/user/test_factories.py -v`

## 1.4 — Teste parametrizado

Foi implementado o teste parametrizado `test_settings_form_factory_uses_current_settings_when_needed`, no arquivo `tests/unit/user/test_factories.py`.

O teste utiliza `pytest.mark.parametrize` para verificar quatro combinações de envio e validação do formulário, incluindo situações válidas e inválidas.

## 1.5 — Teste com dublê (mock)

O teste `test_details_update_factory_returns_handler`, localizado em `tests/unit/user/test_factories.py`, utiliza `mocker.patch` para substituir a chamada ao hook que reúne os validadores de atualização de detalhes.

O mock foi necessário para isolar essa dependência externa e testar a fábrica sem depender da execução dos plugins reais. A interação é verificada com `assert_called_once()`, garantindo que o hook foi chamado exatamente uma vez.





