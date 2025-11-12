# 🧠 Guia Prático Lodash no Cypress (`Cypress._`)

> 📘 **Objetivo:**  
> Centralizar as funções Lodash mais úteis para automação de testes com Cypress.  
> O Lodash já vem embutido no Cypress e pode ser acessado via `Cypress._`.

---

## ⚙️ O que é Lodash?

O **Lodash** é uma biblioteca utilitária para manipulação de **arrays**, **objetos** e **coleções**.  
No Cypress, ela está disponível nativamente como `Cypress._`, sem necessidade de instalação.

💡 **Vantagem:**  
Deixa o código de testes mais limpo, legível e performático — especialmente para manipular massa de dados em testes de API.

---

## 🧩 Comparativo: Lodash vs JavaScript Puro

| Objetivo | Lodash (`Cypress._`) | JavaScript Puro | Diferença prática no QA |
|-----------|----------------------|------------------|--------------------------|
| 🔁 **Repetir N vezes uma ação** | ```js\nCypress._.times(3, (i) => {\n  cy.log(`Camada ${i + 1}`);\n  cy.sendRequestV2(...);\n});\n``` | ```js\nfor (let i = 0; i < 3; i++) {\n  cy.log(`Camada ${i + 1}`);\n  cy.sendRequestV2(...);\n}\n``` | Lodash é **mais declarativo** e evita `for`. Ideal pra gerar massa de teste. |
| 🧱 **Remover campos do objeto** | ```js\nconst body = Cypress._.omit(obj, 'active');\n``` | ```js\nconst { active, ...body } = obj;\n``` | Ambos criam novo objeto sem `active`, mas Lodash é **mais limpo** e aceita lista dinâmica (`omit(obj, ['a','b'])`). |
| 🎯 **Selecionar só alguns campos** | ```js\nconst resumo = Cypress._.pick(obj, ['name','active']);\n``` | ```js\nconst resumo = { name: obj.name, active: obj.active };\n``` | Lodash é **curto e genérico**, ótimo pra comparar payloads esperados. |
| 🔍 **Encontrar 1 item em um array** | ```js\nconst camada2 = Cypress._.find(groups, { order: 2 });\n``` | ```js\nconst camada2 = groups.find((g) => g.order === 2);\n``` | Mesmo resultado, mas Lodash é **mais legível**. |
| 🧮 **Filtrar vários itens** | ```js\nconst ativos = Cypress._.filter(groups, { active: true });\n``` | ```js\nconst ativos = groups.filter((g) => g.active === true);\n``` | Lodash tem **sintaxe mais curta** e **sem repetição**. |
| 🧾 **Ordenar por campo** | ```js\nconst ordenados = Cypress._.sortBy(groups, 'order');\n``` | ```js\nconst ordenados = [...groups].sort((a,b) => a.order - b.order);\n``` | Lodash é **mais limpo e confiável**. |
| 🔢 **Extrair uma coluna de um array** | ```js\nconst ids = Cypress._.map(groups, 'id');\n``` | ```js\nconst ids = groups.map((g) => g.id);\n``` | Mesmo resultado, mas Lodash é **mais direto**. |
| 🔄 **Clonar sem afetar o original** | ```js\nconst clone = Cypress._.cloneDeep(obj);\n``` | ```js\nconst clone = JSON.parse(JSON.stringify(obj));\n``` | Lodash é **mais seguro e performático**, preserva tipos. |
| 🧩 **Mesclar objetos (override)** | ```js\nconst merged = Cypress._.merge(base, override);\n``` | ```js\nconst merged = { ...base, ...override };\n``` | Lodash faz **merge profundo**, JS puro só o 1º nível. |
| ✅ **Comparar objetos profundamente** | ```js\nCypress._.isEqual(a, b);\n``` | ```js\nJSON.stringify(a) === JSON.stringify(b);\n``` | Lodash é **mais confiável**, ignora ordem de chaves. |
| 🧹 **Remover duplicados** | ```js\nconst unicos = Cypress._.uniqBy(users, 'user_id');\n``` | ```js\nconst unicos = users.filter((v,i,a)=>a.findIndex(t=>t.user_id===v.user_id)===i);\n``` | Lodash é **muito mais simples**. |
| 🎲 **Gerar número aleatório** | ```js\nconst id = Cypress._.random(1000, 9999);\n``` | ```js\nconst id = Math.floor(Math.random() * (9999 - 1000)) + 1000;\n``` | Lodash é direto e **excelente pra massa randômica**. |
| 🧮 **Agrupar por campo** | ```js\nconst porStatus = Cypress._.groupBy(groups, 'proposal_status_id');\n``` | ```js\nconst porStatus = groups.reduce((acc,g)=>{\n acc[g.proposal_status_id] = acc[g.proposal_status_id] || [];\n acc[g.proposal_status_id].push(g);\n return acc;\n}, {});\n``` | Lodash é **mais legível e expressivo**. |
| 🧭 **Acessar campo aninhado com segurança** | ```js\nconst id = Cypress._.get(resp, 'body.data.id', null);\n``` | ```js\nconst id = resp?.body?.data?.id ?? null;\n``` | Ambos seguros, mas Lodash aceita **path dinâmico**. |
| 🧱 **Definir campo aninhado dinamicamente** | ```js\nCypress._.set(body, 'metadata.author', 'QA');\n``` | ```js\nbody.metadata = { author: 'QA' };\n``` | Lodash permite **paths complexos** (`'a.b.c'`). |

---

## 🧠 Conclusões rápidas

| Caso de uso | Melhor opção |
|--------------|---------------|
| Criar massa repetida | `Cypress._.times` |
| Testar campos obrigatórios | `Cypress._.omit` |
| Comparar payloads parciais | `Cypress._.pick` |
| Validar lista ordenada | `Cypress._.sortBy` |
| Garantir ausência de duplicidade | `Cypress._.uniqBy` |
| Validar agrupamentos | `Cypress._.groupBy` |
| Acessar dados profundos sem quebrar | `Cypress._.get` |
| Gerar massa randômica | `Cypress._.random` |

---

## 💡 Dica rápida no Cypress

```js
cy.log(Object.keys(Cypress._))
```

Para testar direto no runner:

```js
cy.then(() => {
  const exemplo = Cypress._.omit({ a: 1, b: 2 }, 'a');
  console.log(exemplo); // { b: 2 }
});
```

---

## 🚀 Por que usar Lodash no QA

✅ Deixa os testes mais **limpos e padronizados**  
✅ Facilita manipulação de massa de dados dinâmica  
✅ Reduz duplicação e código repetitivo  
✅ Evita erros de comparação e manipulação manual  
✅ Já vem embutido no Cypress (sem dependências extras)

---

## 🧩 Exemplos práticos aplicados a QA

### 🧪 Criar múltiplas camadas de aprovação

```js
Cypress._.times(3, (i) => {
  const user = { name: `Usuário ${i + 1}`, active: true };
  cy.request("POST", "/api/users", user);
});

```

---

### 🔒 Gerar payload sem campo obrigatório (testar erro 422)

```js
const bodySemEmail = Cypress._.omit(
  { name: "Lídia QA", email: "qa@teste.com" },
  "email"
);

cy.request({
  method: "POST",
  url: "/api/users",
  body: bodySemEmail,
  failOnStatusCode: false, // evita falha automática no 422
}).then((resp) => {
  expect(resp.status).to.eq(422);
  expect(resp.body.status).to.eq("error");
});

```

---

### 🧹 Garantir que não há usuários duplicados

```js
cy.request("GET", "/api/users").then((resp) => {
  const users = resp.body.data;
  const unicos = Cypress._.uniqBy(users, "id");
  expect(unicos.length).to.eq(users.length);
});
```

---

### 🧭 Acessar campo profundo de resposta sem erro

```js
cy.request("GET", "/api/users/1").then((resp) => {
  const nome = Cypress._.get(resp, "body.data.name", "Nome não encontrado");
  cy.log(nome);
});

```

---

### 🎯 Filtrar e validar grupos de um fluxo específico

```js
cy.request("GET", "/api/users").then((resp) => {
  const ativos = Cypress._.filter(resp.body.data, { active: true });
  expect(ativos.length).to.be.greaterThan(0);
});

```

---

### 🔢 Garantir ordem correta por ID

```js
cy.request("GET", "/api/users").then((resp) => {
  const ordenados = Cypress._.sortBy(resp.body.data, "id");
  const ids = Cypress._.map(ordenados, "id");
  expect(ids).to.deep.equal([1, 2, 3]);
});

```

---

### 🧮 Agrupar registros por status

```js
cy.request("GET", "/api/users").then((resp) => {
  const agrupados = Cypress._.groupBy(resp.body.data, "status");
  cy.log(Object.keys(agrupados)); // Exibe os status existentes
});

```

---

## 🧩 Boas práticas com Lodash no Cypress

1. **Prefira Lodash** sempre que estiver manipulando massa de dados complexa.  
2. Use **factories** para gerar objetos e **Lodash** para transformá-los.  
3. Centralize utilitários frequentes (como `omit`, `pick`, `sortBy`) em **comandos customizados**.  
4. Combine `Cypress._` com aliases (`@`) e factories para manter os testes consistentes.  
5. Evite `for` e `while` — prefira `Cypress._.times()` para gerar dados previsíveis.

---

💬 **Resumo Final:**  
> Lodash no Cypress é uma ferramenta essencial para manter os testes:  
> - mais **limpos**  
> - mais **performáticos**  
> - e com **menos código repetido**
