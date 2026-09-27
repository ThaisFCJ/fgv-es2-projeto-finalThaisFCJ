
# Catálogo de Code Smells — Parte 2

## 1. Long Method 

**Localização:** `flaskbb/forum/models.py:829–881`  

### Trecho de código

```python
def save(
    self,
    user: "User | None" = None,
    forum: "Forum | None" = None,
    post: Post | None = None,
):
    pluggy.hook.flaskbb_event_topic_save_before(topic=self)

    if self.id:
        db.session.add(self)
        db.session.commit()
        pluggy.hook.flaskbb_event_topic_save_after(topic=self, is_new=False)
        return self

    if forum is None or user is None:
        logger.error("Cant create a topic without a user or forum")
        return

    with db.session.no_autoflush:
        self.forum = forum
        self.user = user
        self.username = user.username
        self.date_created = self.last_updated = time_utcnow()

        db.session.add(self)
        db.session.commit()

        if post is not None:
            self._post = post

        self._post.save(user, self)

        self.last_post = self.first_post = self._post
        forum.topic_count += 1

    db.session.commit()
    pluggy.hook.flaskbb_event_topic_save_after(topic=self, is_new=True)
    return self
```

### Por que é um smell?

O método concentra várias responsabilidades: atualizar tópicos existentes, criar novos tópicos, associar o post inicial, atualizar contadores e executar hooks. Essa concentração dificulta acompanhar o fluxo e testar as operações separadamente.
---

## 2. Duplicated Code

**Localização:** `flaskbb/forum/models.py:1403–1467`  

### Trecho de código

```python
if user.is_authenticated:
    stmt = (
        db.select(Topic, Post, TopicsRead)
        .outerjoin(
            TopicsRead,
            db.and_(
                TopicsRead.topic_id == Topic.id,
                TopicsRead.user_id == user.id,
            ),
        )
        .outerjoin(Post, Topic.last_post_id == Post.id)
        .where(Topic.forum_id == forum_id)
        .order_by(Topic.important.desc(), Topic.last_updated.desc())
    )
    hidden(stmt)
    topics = paginate(stmt, page=page, per_page=per_page)
else:
    stmt = (
        db.select(Topic, Post)
        .outerjoin(Post, Topic.last_post_id == Post.id)
        .where(Topic.forum_id == forum_id)
        .order_by(Topic.important.desc(), Topic.last_updated.desc())
    )
    stmt = hidden(stmt)
    topics = paginate(stmt, page=page, per_page=per_page)
    topics.items = [
        (topic, last_post, None) for topic, last_post in topics.items
    ]
```

### Por que é um smell?

Os dois caminhos repetem partes da construção da consulta, incluindo o relacionamento com `Post`, o filtro pelo fórum, a ordenação e a paginação. Embora existam diferenças necessárias entre usuários autenticados e anônimos, a repetição torna futuras alterações mais trabalhosas.

---

## 3. Long Method

**Localização:** `flaskbb/forum/models.py:1275–1303`  

### Trecho de código

```python
def recalculate(self, last_post: bool = False):
    topic_count_stmt = db.select(db.func.count(Topic.id)).where(
        Topic.forum_id == self.id, Topic.hidden.is_(False)
    )

    post_count_stmt = (
        db.select(db.func.count(Post.id))
        .join(Topic, Post.topic_id == Topic.id)
        .where(
            Topic.forum_id == self.id,
            Post.hidden.is_(False),
            Topic.hidden.is_(False),
        )
    )

    self.topic_count = db.session.scalar(topic_count_stmt)
    self.post_count = db.session.scalar(post_count_stmt)

    if last_post:
        self.update_last_post()

    self.save()
    return self
```

### Por que é um smell?

O método monta consultas, calcula estatísticas, atualiza opcionalmente o último post e salva o fórum. A combinação de cálculo, atualização e persistência dificulta compreender e testar cada responsabilidade de forma isolada.

---

## 4. Long Method

**Localização:** `flaskbb/forum/models.py:294–340`  

### Trecho de código

```python
def save(self, user: "User | None" = None, topic: "Topic | None" = None):
    pluggy.hook.flaskbb_event_post_save_before(post=self)

    if self.id:
        db.session.add(self)
        db.session.commit()
        pluggy.hook.flaskbb_event_post_save_after(post=self, is_new=False)
        return self

    if user and topic:
        with db.session.no_autoflush:
            created = time_utcnow()
            self.user = user
            self.username = user.username
            self.topic = topic
            self.date_created = created

            if not topic.hidden:
                topic.last_updated = created
                topic.last_post = self

                topic.forum.last_post = self
                topic.forum.last_post_user = self.user
                topic.forum.last_post_title = topic.title
                topic.forum.last_post_username = user.username
                topic.forum.last_post_created = created

                user.post_count += 1
                topic.post_count += 1
                topic.forum.post_count += 1

        db.session.add(self)
        db.session.commit()
        pluggy.hook.flaskbb_event_post_save_after(post=self, is_new=True)
        return self
```

### Por que é um smell?

Além de salvar o post, o método atualiza informações do tópico e do fórum, datas e contadores de diferentes entidades. Essa concentração aumenta o acoplamento e dificulta identificar os efeitos colaterais da operação.
---

## 5. Redundant Conditional — Condicional redundante

**Localização:** `flaskbb/forum/models.py:763–782`  

### Trecho de código

```python
if topicsread:
    logger.debug("Updating existing TopicsRead '{}' object.".format(topicsread))
    topicsread.last_read = time_utcnow()
    topicsread.save()
    updated = True

elif not topicsread:
    logger.debug("Creating new TopicsRead object.")
    topicsread = TopicsRead()
    topicsread.user = user
    topicsread.topic = self
    topicsread.forum = self.forum
    topicsread.last_read = time_utcnow()
    topicsread.save()
    updated = True

else:
    updated = False
```

### Por que é um smell?

O `else` final é inalcançável, pois as condições anteriores cobrem os dois resultados possíveis da avaliação de `topicsread`. Esse bloco não acrescenta comportamento e aumenta o ruído visual do código.
---

## 6. Incorrect Accumulation of Results

**Localização:** `flaskbb/forum/models.py:1354–1362`  

### Trecho de código

```python
def move_topics_to(self, topics: list[Topic]):
    """Moves a bunch a topics to the forum. Returns ``True`` if all
    topics were moved successfully to the forum.

    :param topics: A iterable with topic objects.
    """
    status = False
    for topic in topics:
        status = topic.move(self)
    return status
```

### Por que é um smell?

A documentação informa que o método deve retornar `True` quando todos os tópicos forem movidos com sucesso. Entretanto, a variável `status` é sobrescrita a cada iteração, fazendo com que o resultado final represente apenas a última movimentação.

Por exemplo, se uma movimentação anterior falhar e a última tiver sucesso, o método retornará `True`, mesmo sem todos os tópicos terem sido movidos.


---
