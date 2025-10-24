---
tipo: financeiro
mês: Setembro
ano: "2025"
despesas:
  - categoria: Neon
    valor: 300
  - categoria: Academia
    valor: 100
  - categoria: Santander
    valor: 50
  - categoria: Netflix
    valor: 45
  - categoria: "[[F0019 - transferido]]"
    valor: 25
planejado:
  - categoria: Carro
    valor: 150
receitas:
  - fonte: Salario
    valor: 1170.78
total_despesas: 520
total_planejado: 150
total_receitas: 1170.78
saldo: 650.78
---

# 📊 Controle Financeiro - Setembro / 2025

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
