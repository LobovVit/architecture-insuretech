# Task5 — GraphQL API for client-info

## Цель

Перевести REST API сервиса `client-info` на GraphQL для:
- снижения количества HTTP-запросов
- уменьшения RPS
- исключения over-fetching и under-fetching данных
- гибкого выбора полей клиентом

Исходный REST-контракт:

- GET /clients/{id}
- GET /clients/{id}/documents
- GET /clients/{id}/relatives

---

# Примеры использования GraphQL

## 1. Получение базовой информации о клиенте
(аналог GET /clients/{id})

```graphql
query {
  client(id: "123") {
    id
    name
    age
  }
}
```

---

## 2. Получение клиента вместе с документами
(вместо двух REST-запросов)

```graphql
query {
  client(id: "123") {
    id
    name
    documents {
      id
      type
      number
      issueDate
      expiryDate
    }
  }
}
```

---

## 3. Получение клиента с родственниками и документами одним запросом
(главное преимущество GraphQL)

```graphql
query {
  client(id: "123") {
    id
    name
    documents {
      type
      number
    }
    relatives {
      relationType
      name
    }
  }
}
```

---

## 4. Получение только документов клиента
(аналог GET /clients/{id}/documents)

```graphql
query {
  clientDocuments(clientId: "123") {
    id
    type
    number
    issueDate
    expiryDate
  }
}
```

---

## 5. Получение только родственников клиента
(аналог GET /clients/{id}/relatives)

```graphql
query {
  clientRelatives(clientId: "123") {
    id
    relationType
    name
    age
  }
}
```

---

# Преимущества GraphQL в данной задаче

- Клиент запрашивает **только нужные поля**
- Несколько REST-запросов заменяются **одним GraphQL-запросом**
- Снижается нагрузка на сервис `client-info`
- Повышается гибкость API для разных сценариев (web, core-app, партнеры)
