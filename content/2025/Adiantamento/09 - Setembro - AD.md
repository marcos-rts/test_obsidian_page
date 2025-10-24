---
tipo: "financeiro"
mês: "Setembro"
ano: "2025"
despesas: [
  {
    "categoria": "Casa",
    "valor": 100
  },
  {
    "categoria": "Celular",
    "valor": 30
  },
  {
    "categoria": "Inter",
    "valor": 150
  },
  {
    "categoria": "Academia",
    "valor": 100
  },
  {
    "categoria": "Torra torra",
    "valor": 119
  }
]
planejado: []
receitas: [
  {
    "fonte": "Adiantamento",
    "valor": 1020.46
  }
]
total_despesas: 499.00
total_planejado: 0.00
total_receitas: 1020.46
saldo: 521.46
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
