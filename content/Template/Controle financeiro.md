<%*
let mes = await tp.system.prompt("Digite o mês");
let ano = tp.date.now("YYYY");

// Inserir despesas
let despesas = [];
let numDespesas = await tp.system.prompt("Quantas despesas você quer adicionar?");
numDespesas = parseInt(numDespesas);

for (let i = 0; i < numDespesas; i++) {
    let categoria = await tp.system.prompt(`Despesa ${i + 1} - Categoria:`);
    let valor = await tp.system.prompt(`Despesa ${i + 1} - Valor (R$):`);
    despesas.push({ categoria, valor: parseFloat(valor) });
}

// Inserir planejamentos
let planejado = [];
let numPlanejado = await tp.system.prompt("Quantos planejamentos você quer adicionar?");
numPlanejado = parseInt(numPlanejado);

for (let i = 0; i < numPlanejado; i++) {
    let categoria = await tp.system.prompt(`Planejamento ${i + 1} - Categoria:`);
    let valor = await tp.system.prompt(`Planejamento ${i + 1} - Valor (R$):`);
    planejado.push({ categoria, valor: parseFloat(valor) });
}

// Inserir receitas
let receitas = [];
let numReceitas = await tp.system.prompt("Quantas receitas você quer adicionar?");
numReceitas = parseInt(numReceitas);

for (let i = 0; i < numReceitas; i++) {
    let fonte = await tp.system.prompt(`Receita ${i + 1} - Fonte:`);
    let valor = await tp.system.prompt(`Receita ${i + 1} - Valor (R$):`);
    receitas.push({ fonte, valor: parseFloat(valor) });
}

// Calcular totais
let total_despesas = despesas.reduce((sum, d) => sum + d.valor, 0);
let total_planejado = planejado.reduce((sum, d) => sum + d.valor, 0);
let total_receitas = receitas.reduce((sum, d) => sum + d.valor, 0);
let saldo = total_receitas - total_despesas;
%>

---
tipo: "financeiro"
mês: "<% mes %>"
ano: "<% ano %>"
despesas: <% JSON.stringify(despesas, null, 2) %>
planejado: <% JSON.stringify(planejado, null, 2) %>
receitas: <% JSON.stringify(receitas, null, 2) %>
total_despesas: <% total_despesas.toFixed(2) %>
total_planejado: <% total_planejado.toFixed(2) %>
total_receitas: <% total_receitas.toFixed(2) %>
saldo: <% saldo.toFixed(2) %>
---

# 📊 Controle Financeiro - <% mes %> / <% ano %>

## 🏷️ Despesas  
```dataviewjs
const despesas = dv.current().despesas;
dv.table(["Categoria", "Valor"], despesas.map(d => [d.categoria, `R$ ${d.valor.toFixed(2)}`]));
```

## 📌 Planejado

```dataviewjs
const planejado = dv.current().planejado;
dv.table(["Categoria", "Valor"], planejado.map(d => [d.categoria, `R$ ${d.valor.toFixed(2)}`]));
```

## 💰 Receitas

```dataviewjs
const receitas = dv.current().receitas;
dv.table(["Fonte", "Valor"], receitas.map(d => [d.fonte, `R$ ${d.valor.toFixed(2)}`]));
```

## 📊 Resumo

```dataviewjs
const totalReceitas = dv.current().total_receitas;
const totalDespesas = dv.current().total_despesas;
const totalPlanejado = dv.current().total_planejado;

const reserva10 = totalReceitas * 0.1050;
const saldoFinal = totalReceitas - totalDespesas;
const saldoComReserva = (totalReceitas - reserva10) - totalDespesas;

dv.list([
  `💸 Total Despesas: R$ ${totalDespesas.toFixed(2)}`,
  `📌 Total Planejado: R$ ${totalPlanejado.toFixed(2)}`,
  `💰 Total Receitas: R$ ${totalReceitas.toFixed(2)}`,
  `💼 10,5% da Receita (reserva): R$ ${reserva10.toFixed(2)}`,
  `📊 Saldo Final: R$ ${saldoFinal.toFixed(2)}`,
  `🛡️ Saldo após reservar 10,5%: R$ ${saldoComReserva.toFixed(2)}`
]);

```
