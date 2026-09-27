# Tarefa 1.6 — Cobertura Final

## 1. Resultado da cobertura

A cobertura foi executada para o módulo `flaskbb/forum/`, utilizando medição de linhas e branches.

A execução apresentou **45 testes aprovados em 10,07 segundos**, incluindo os 9 casos de teste novos desenvolvidos na Tarefa 1.3.


| Arquivo       | Linhas cobertas | Cobertura |
| ------------- | --------------: | --------: |
| `__init__.py` |             0/2 |        0% |
| `forms.py`    |            0/91 |        0% |
| `locals.py`   |            9/29 |       35% |
| `models.py`   |         364/638 |       59% |
| `utils.py`    |            0/10 |        0% |
| `views.py`    |          34/499 |        6% |
| **Total**     |    **407/1269** |   **33%** |

O arquivo `models.py` apresentou a maior cobertura individual, com 59%, enquanto `views.py` apresentou 6%.

## 2. Comparação com o baseline e a meta

O módulo escolhido para os novos testes foi `flaskbb/forum/`.

| Indicador             | Baseline (Tarefa 1.1) |    Final |               Evolução |
| --------------------- | --------------------: | -------: | ---------------------: |
| Linhas cobertas       |              417/1272 | 407/1269 |             -10 linhas |
| Cobertura de linhas   |                32,78% |   32,07% | -0,71 ponto percentual |
| Branches cobertos     |               110/284 |  106/284 |            -4 branches |
| Cobertura de branches |                38,73% |   37,32% | -1,41 ponto percentual |

A meta definida na Tarefa 1.2 era atingir 45,71% de cobertura de linhas. O resultado final foi de 32,07%, ficando 13,64 pontos percentuais abaixo da meta.


## 3. Cenários que continuam descobertos


| Arquivo       | Cenários remanescentes                                                           | Sugestão de cobertura futura                                                                            |
| ------------- | -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| `models.py`   | Métodos e condições de modelos de fóruns, tópicos e posts ainda não exercitados. | Criar testes para os caminhos alternativos de criação, atualização, leitura e persistência dos modelos. |
| `views.py`    | A maior parte das rotas e respostas ainda não foi exercitada.                    | Testar permissões, usuários não autenticados, entradas inválidas e respostas de erro.                   |
| `forms.py`    | Regras de validação e processamento dos formulários ainda não cobertas.          | Testar campos obrigatórios, entradas inválidas e limites de tamanho.                                    |
| `locals.py`   | Variáveis locais e condições de contexto ainda não exercitadas.                  | Criar testes para os diferentes contextos de requisição e usuários.                                     |
| `utils.py`    | Funções auxiliares ainda não cobertas.                                           | Adicionar testes unitários para os comportamentos e condições dessas funções.                           |
| `__init__.py` | Código de inicialização do módulo ainda não exercitado.                          | Verificar os caminhos de inicialização e registro de componentes do módulo.                             |

