# Relatório de Refatorações

## 1. Extração do método de criação de tópicos

* **Commit:** `2e3fec1cb4cab438f67e49c126f166ff8ea1fcb8`
* **Arquivo:** `flaskbb/forum/models.py`
* **Code smell:** Long Method.
* **Transformação:** Extract Method.
* **Antes:** A lógica de criação do tópico estava diretamente dentro do método original, concentrando diferentes responsabilidades em um único bloco.
* **Depois:** A criação foi extraída para o método auxiliar `_create_topic`, deixando o método principal mais simples e facilitando a manutenção.

## 2. Simplificação da consulta de tópicos

* **Commit:** `0cec865c727685a45345ea6d0d848982858446b4`
* **Arquivo:** `flaskbb/forum/models.py`
* **Code smell:** Código duplicado (*Duplicate Code*).
* **Transformação:** Simplificação da consulta e eliminação da repetição na construção da query.
* **Antes:** A consulta era construída em blocos separados para usuários autenticados e não autenticados, repetindo parte da lógica de consulta.
* **Depois:** A construção da consulta foi simplificada, mantendo os filtros e a ordenação e reduzindo a repetição de código.

## 3. Extração da atualização do último post

* **Commit:** `9c188be0ec99159d964f90c5d6b153a91e85cf82`
* **Arquivo:** `flaskbb/forum/models.py`
* **Code smell:** Método longo (*Long Method*).
* **Transformação:** Extract Method.
* **Antes:** O método `Post.save` concentrava a lógica de salvamento da publicação e a atualização das informações do último post do fórum.
* **Depois:** A atualização foi extraída para o método auxiliar `_update_forum_last_post`, separando essa responsabilidade e facilitando a leitura do método `Post.save`.

## 4. Simplificação do rastreamento de leitura

* **Commit:** `b4c0eb3ee5e9f9d2783431ba18b6c842d6bf9169`
* **Arquivo:** `flaskbb/forum/models.py`
* **Code smell:** Código duplicado (*Duplicate Code*).
* **Transformação:** Simplificação de código condicional e eliminação de instruções duplicadas.
* **Antes:** Os blocos `if topicsread` e `elif not topicsread` repetiam as instruções de atualização de `last_read` e salvamento do registro. Havia também uma condição `else` inalcançável.
* **Depois:** A criação e a atualização do registro foram mantidas, mas as instruções comuns foram reunidas em um único bloco:

```python
topicsread.last_read = time_utcnow()
topicsread.save()
```

Isso reduz a duplicação e deixa o método `Topic.update_read` mais simples de entender.



