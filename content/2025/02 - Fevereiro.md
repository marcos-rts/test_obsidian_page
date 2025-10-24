---
tipo: "financeiro"
mês: "Fevereiro"
ano: "2025"
despesas: [
  {
    "categoria": "Neon",
    "valor": 37.79
  },
  {
    "categoria": "Terreno",
    "valor": 490.96
  },
  {
    "categoria": "Casa",
    "valor": 100
  },
  {
    "categoria": "Celular",
    "valor": 30
  },
  {
    "categoria": "Netflix",
    "valor": 44.9
  },
  {
    "categoria": "Santander",
    "valor": 15
  },
  {
    "categoria": "Inter",
    "valor": 106.4
  }
]
planejado: []
receitas: [
  {
    "fonte": "Pagamento",
    "valor": 815.07
  },
  {
    "fonte": "Adiantamento",
    "valor": 730.02
  }
]
total_despesas: 825.05
total_planejado: 0.00
total_receitas: 1545.09
saldo: 720.04
---

# 📊 Controle Financeiro - Fevereiro / 2025

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

const reserva10 = totalReceitas * 0.10;
const saldoFinal = totalReceitas - totalDespesas;
const saldoComReserva = (totalReceitas - reserva10) - totalDespesas;

dv.list([
  `💸 Total Despesas: R$ ${totalDespesas.toFixed(2)}`,
  `📌 Total Planejado: R$ ${totalPlanejado.toFixed(2)}`,
  `💰 Total Receitas: R$ ${totalReceitas.toFixed(2)}`,
  `💼 10% da Receita (reserva): R$ ${reserva10.toFixed(2)}`,
  `📊 Saldo Final: R$ ${saldoFinal.toFixed(2)}`,
  `🛡️ Saldo após reservar 10%: R$ ${saldoComReserva.toFixed(2)}`
]);

```
