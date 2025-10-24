---
tipo: financeiro
mês: Maio
ano: "2025"
despesas:
  - categoria: Ex-terreno pt1
    valor: 250
  - categoria: Ex-terreno pt2
    valor: 250
  - categoria: Santander
    valor: 49.5
  - categoria: Neon
    valor: 128.42
  - categoria: Netflix
    valor: 44.9
  - categoria: Celular
    valor: 30
  - categoria: Inter
    valor: 78.83
  - categoria: Casa
    valor: 100
planejado:
  - categoria: "[[F0010 - Trasnferido]]"
    valor: 100
  - categoria: Gasolina-Pai
    valor: 50
receitas:
  - fonte: Salario
    valor: 1317.66
  - fonte: Adiantamento
    valor: 970.02
total_despesas: 931.65
total_planejado: 150
total_receitas: 2287.68
saldo: 1356.03
---

# 📊 Controle Financeiro - Maio / 2025

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
