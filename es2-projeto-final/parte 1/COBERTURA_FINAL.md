# Tarefa 1.6 — Cobertura Final

## 1. Resultado da cobertura

A cobertura foi executada para os módulos `forum`, `management` e `user`, utilizando medição de linhas e branches.

Apresentou 241 testes aprovados, 1 falha e 1 teste ignorado. A falha ocorreu no teste original `test_flaskbbdomain_translations`, que já apresentava o mesmo problema no baseline: retorno de `NullTranslations` em vez de `Translations`.

## 2. Comparação com o baseline e a meta

O módulo escolhido para os testes foi `flaskbb/user/`.

| Indicador             | Baseline (Tarefa 1.1) |   Final |                 Evolução |
| --------------------- | --------------------: | ------: | -----------------------: |
| Linhas cobertas       |               172/560 | 187/560 |               +15 linhas |
| Cobertura de linhas   |                30,71% |  33,39% | +2,68 pontos percentuais |
| Branches cobertos     |                 42/72 |   44/72 |              +2 branches |
| Cobertura de branches |                58,33% |  61,11% | +2,78 pontos percentuais |

A meta definida na Tarefa 1.2 era atingir 45,71% de cobertura de linhas. O resultado final foi de 33,39%.

## 3. Cenários que continuam descobertos

A análise do relatório de cobertura indica trechos ainda não exercitados nos seguintes arquivos do módulo `user/`:

| Arquivo                  | Cenários remanescentes                                                                  | Sugestão de cobertura futura                                                                                   |
| ------------------------ | --------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| `models.py`              | Comportamentos de modelos e métodos auxiliares ainda não exercitados.                   | Criar testes unitários para métodos e condições não cobertos, incluindo entradas válidas, inválidas e limites. |
| `forms.py`               | Regras de validação e processamento de formulários ainda descobertas.                   | Testar campos obrigatórios, entradas vazias, formatos inválidos e limites de tamanho.                          |
| `views.py`               | Caminhos alternativos das rotas e respostas ainda não cobertos.                         | Criar testes para permissões, usuários não autenticados, entradas inválidas e respostas de erro.               |
| `services/update.py`     | Fluxos de atualização de senha, e-mail, detalhes e configurações ainda não exercitados. | Testar sucesso, falhas de validação e exceções esperadas dos handlers.                                         |
| `services/validators.py` | Regras e condições dos validadores ainda não cobertas.                                  | Acrescentar testes parametrizados para entradas válidas, inválidas e casos de borda.                           |
| `plugins.py`             | Integrações e caminhos relacionados aos plugins ainda não exercitados.                  | Utilizar mocks para testar chamadas, ausência de plugins e resultados inesperados.                             |
| `services/factories.py`  | Parte das fábricas e dos caminhos condicionais permanece descoberta.                    | Testar as demais fábricas, incluindo formulários de alteração de senha, e-mail e detalhes.                     |
