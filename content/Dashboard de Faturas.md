
## Faturas Pendentes

```dataview
table Vencimento as "Vencimento", Banco, total_bruto as "Total Bruto", taxa as "Taxa", total_com_taxa as "Total com Taxa", aliases, file.link as "Link"
from "Faturas"
where status != "Paga"
sort total_com_taxa asc
```

## Total de Faturas Pendentes

```dataview
table parcelas_faltando as "Total de parcelas Pendentes", parcelas as "Parcelas", valor_parcela as "Valor Parcela", total_com_taxa as "Total", Banco
from "Faturas"
where status != "Paga"
sort valor_parcela asc
```

## Total de Faturas Pagas

```dataview
table total_bruto as "Total Bruto", taxa as "Taxa", total_com_taxa as "Total com Taxa", file.link as "Link"
from "Faturas"
where status = "Paga"
```

---

```dataview
table status, data_infracao as "Infração", vencimento as "Vencimento", valor as "Valor (R$)", valor_pago as "Pago (R$)", file.link as "Nota"
from "Carro multa"
where type = "multa"
sort vencimento asc
```


```dataview
table length(rows) as "Quantidade", sum(rows.valor) as "Valor total (R$)"
from "Carro multa"
where type = "multa"
group by status

```

```dataview
table file.link, vencimento, (date(vencimento) - date(today)).days as "Dias até o venc."
from "Carro multa"
where type = "multa" and date(vencimento) >= date(today) and date(vencimento) <= date(today) + dur(30 days)
sort vencimento asc

```


```dataviewjs
const rows = dv.pages('"Carro multa"').where(p => p.type == "multa");
const result = rows.map(p => {
  const venc = p.vencimento ? dv.date(p.vencimento) : null;
  const dias = venc ? (venc - dv.date("today")).days : null;
  return {
    file: p.file.link,
    status: p.status,
    vencimento: venc ? venc.toISODate() : "—",
    dias: dias===null ? "—" : dias,
    valor: p.valor ?? 0
  };
});
// sort by dias (nulls last)
result.sort((a,b)=> {
  if(a.dias==="—") return 1;
  if(b.dias==="—") return -1;
  return a.dias - b.dias;
});

dv.table(["Nota","Status","Vencimento","Dias","Valor (R$)"],
  result.map(r=>[r.file, r.status, r.vencimento, r.dias, r.valor]));

```
