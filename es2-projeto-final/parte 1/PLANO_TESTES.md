# Plano de Testes — Parte 1.2

## 1. Módulo escolhido

**Módulo:** `flaskbb/user/`

Modelo escolhido por possuir funcionalidades relacionadas a formulários, validações e atualização de dados. 
O módulo também possui funções de criação de formulários e handlers que podem ser exploradas nos testes, como em `services/factories.py`.

## 2. Cobertura inicial

| Métrica               | Resultado |
| --------------------- | --------: |
| Linhas cobertas       |   172/560 |
| Cobertura de linhas   |    30,71% |
| Branches cobertos     |     42/72 |
| Cobertura de branches |    58,33% |

## 3. Meta de cobertura

Aumentar a cobertura de linhas em pelo menos 15 pontos percentuais, passando de 30,71% para, no mínimo, 45,71%.

## 4. Cenários inicialmente identificados

| Nº | Cenário                                                                                                                    | Tipo        |
| -- | -------------------------------------------------------------------------------------------------------------------------- | ----------- |
| 1  | Verificar se a factory de atualização de detalhes reúne os validadores dos plugins e retorna o handler correspondente.     | Happy path  |
| 2  | Verificar se a factory de atualização de senha reúne os validadores e retorna o handler correspondente.                    | Happy path  |
| 3  | Verificar se a factory de atualização de e-mail reúne os validadores e retorna o handler correspondente.                   | Happy path  |
| 4  | Verificar se a factory de configurações retorna o handler de configurações sem reunir validadores.                         | Happy path  |
| 5  | Verificar se a factory de formulário de configurações adiciona a opção padrão de tema e carrega os idiomas disponíveis.    | Happy path  |
| 6  | Verificar se o formulário de configurações mantém os dados enviados quando a validação é bem-sucedida.                     | Happy path  |
| 7  | Verificar se o formulário de configurações restaura os valores atuais do usuário quando não há envio ou a validação falha. | Limite/erro |
