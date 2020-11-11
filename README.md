# gosu-deeppavlov-wot-parser
Data collection project for media wiki data for world of tanks
Проект для сбора данных для модели диппавлова

## DeepPavlov for question answering system

**Логика работы:** 

Внутри идет выделение именованных сущностей и поиск и в контексте.

Находит в предложениях часть фразу, которая наиболее близка ответу на вопрос. Возвращает часть фразу.

**Как использовать:** 

Изначально нужно запрепроцессить данные, чтобы они в себе содержали обращение к предмету и суть вопроса.

Пример допустим нас интересует персонаж Azir, в GamePedia мы найдем информацию по нему - [https://lol.gamepedia.com/Azir](https://lol.gamepedia.com/Azir)

Игрока интересует скилл - Conquering Sands

В данных геймпедии выглядит так:

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fa829bff-202a-4b8a-9c91-3718cf720c83/__2020-10-08__10.44.30.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fa829bff-202a-4b8a-9c91-3718cf720c83/__2020-10-08__10.44.30.png)

Необходимо следующее преобразование для корректной работы модели:

```jsx
Conquering Sands Cost 70 Mana. 
Conquering Sands Cooldown 15 seconds at level 1.
Conquering Sands Cooldown 13 seconds at level 2.
Conquering Sands Cooldown 11 seconds at level 3.
Conquering Sands Cooldown 9 seconds at level 4.
Conquering Sands Cooldown 7 seconds at level 5.
Conquering Sands Range 740.
Conquering Sands Magic Damage 70 at level 1.
Conquering Sands Magic Damage 90 at level 2.
Conquering Sands Magic Damage 110 at level 3.
Conquering Sands Magic Damage 130 at level 4.
```

Далее можем задавать вопросы даже с разными языками, но часто языки могут некорректно работать, лучше на одном (2 пример). В третьем примере тот же вопрос но на английском ответ корректный:

```jsx
A:130
Q:какой урон of Conquering Sands at level 4?

A: Conquering Sands Cost 70 Mana
Q: какой Cooldown Conquering Sands на 5 уровне?

A: Conquering Sands Cooldown 7 seconds
Q: what cooldown of Conquering Sands at 5 level?

A: Conquering Sands Cost 70 Mana
Q: сколько маны ест Conquering Sands?

A: I don't know
Q: что такое?
```

Интересно, что у него есть заготовленные ответы, если он не знает ответа, но в 90% он что-то ответит даже вполне релевантное

```jsx
A: 15 seconds at level 1
Q: what is cooldown?
```

**Плюсы:** 

- работает из коробки как пулемет, подготовить данные в нужном виде
- Работает с разными языками, можно писать вопрос частично на разных языках и будет ответ

**Минусы:**

- Нужно аккуратно подготовить датасет
- При миксе языков может отвечать некорректно
- Не будет поддерживать цепочку вопросов. второй вопрос люди зададут без сущностей в первом вопросе - например "а какой на 2 уровне?" к последнему вопросу, нужно будет вытащить из первого предложения корректно, либо просить явно задавать предмет вопроса
- На вход вопроса нужно подавать контекст

**Ссылка:** 

[https://demo.deeppavlov.ai/#/en/textqa](https://demo.deeppavlov.ai/#/en/textqa)