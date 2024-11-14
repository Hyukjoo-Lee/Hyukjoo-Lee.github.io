---
title: "{ Node } Express_API"
date: 2024-11-06 03:00:00 KST
tags: [Node]
---

```javascript
const express = require("express");
const app = express();
const port = 3003;

// JSON 데이터 처리를 위해 필요
app.use(express.json());

// 임시 데이터베이스 역할을 하는 배열
let contacts = [
  { id: 1, name: "Alice", phone: "010-1234-5678" },
  { id: 2, name: "Bob", phone: "010-2345-6789" },
];

// 모든 연락처 가져오기
app.get("/contacts", (req, res) => {
  res.status(200).json(contacts);
});

// 특정 연락처 상세보기
app.get("/contacts/:id", (req, res) => {
  const contactId = parseInt(req.params.id);
  const contact = contacts.find((c) => c.id === contactId);

  if (contact) {
    res.status(200).json(contact);
  } else {
    res.status(404).send("Contact not found");
  }
});

// 새 연락처 추가하기
app.post("/contacts", (req, res) => {
  const newContact = {
    id: contacts.length + 1,
    name: req.body.name,
    phone: req.body.phone,
  };

  contacts.push(newContact);
  res.status(201).json(newContact);
});

// 특정 연락처 수정하기
app.put("/contacts/:id", (req, res) => {
  const contactId = parseInt(req.params.id);
  const contact = contacts.find((c) => c.id === contactId);

  if (contact) {
    contact.name = req.body.name || contact.name;
    contact.phone = req.body.phone || contact.phone;
    res.status(200).json(contact);
  } else {
    res.status(404).send("Contact not found");
  }
});

// 특정 연락처 삭제하기
app.delete("/contacts/:id", (req, res) => {
  const contactId = parseInt(req.params.id);
  const contactIndex = contacts.findIndex((c) => c.id === contactId);

  if (contactIndex !== -1) {
    const deletedContact = contacts.splice(contactIndex, 1);
    res.status(200).json(deletedContact);
  } else {
    res.status(404).send("Contact not found");
  }
});

app.listen(port, () => {
  console.log(`${port} 포트번호 서버 실행중`);
});
```

- `app.use(express.json())`: JSON 형식의 요청 본문을 처리할 수 있게 설정.
- `GET /contacts`: 모든 연락처 데이터를 반환합니다.
- `GET /contacts/:id`: `:id`에 해당하는 특정 연락처를 조회합니다.
- `POST /contacts`: 새 연락처를 추가합니다. `id`는 배열 길이를 기준으로 자동 생성됩니다.
- `PUT /contacts/:id`: 특정 `id`의 연락처 정보를 수정합니다.
- `DELETE /contacts/:id`: 특정 `id`의 연락처를 삭제합니다.
