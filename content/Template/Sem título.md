---
aliases: <% tp.system.prompt('Nome de exibição (obrigatório)') %>
tags:
  - contato
  - pessoa
created: <% tp.date.now("YYYY-MM-DD") %>
---

# 👤 Ficha de Contato

## 📛 Identificação
- **Nome Completo:** <% tp.system.prompt('Nome completo') %>
- **Apelido/Nick:** <% tp.system.prompt('Apelido ou nick') %>
- **Cargo/Função:** <% tp.system.prompt('Cargo ou Função') %>
- **Empresa/Organização:** <% tp.system.prompt('Empresa/Organização') %>

---

## 📞 Contato
- **Telefone:** <% tp.system.prompt('Telefone') %>
- **E-mail:** <% tp.system.prompt('E-mail') %>
- **Site/LinkedIn:** <% tp.system.prompt('Site/LinkedIn/Portfólio') %>
- **Endereço:** <% tp.system.prompt('Endereço') %>

---

## 📝 Observações
<% tp.system.prompt('Observações gerais (digite algo ou deixe em branco)') %>

---

## 🔗 Marcações e Relacionamentos
- **Relacionado a projetos:** <% tp.system.prompt('Projetos relacionados') %>
- **Grupo/Categoria:** <% tp.system.prompt('Grupo ou categoria (ex: Cliente, Fornecedor, Amigo, etc.)') %>

---

### 📂 Menções vinculadas
```dataviewjs
let name = dv.current().file.aliases?.[0] ?? dv.current().file.name;
let mentions = dv.pages()
  .where(p => p.file.path != dv.current().file.path) // ignora a própria ficha
  .filter(p => (p.file.inlinks.some(l => l.path == dv.current().file.path) 
             || (p.file.text && p.file.text.includes(name))));

if (mentions.length > 0) {
  dv.list(mentions.file.link);
} else {
  dv.paragraph("🚫 Nenhuma menção encontrada.");
}
```
