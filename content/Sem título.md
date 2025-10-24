```dataviewjs
// 🧾 Painel de Faturas
const pasta = "Faturas";
const faturas = dv.pages(`"${pasta}"`).where(f => f.fatura);

dv.header(2, "📑 Painel de Faturas");

// Função para contar parcelas pagas a partir do conteúdo
async function getParcelasPagas(file) {
  const content = await app.vault.read(file);
  const pagas = (content.match(/- \[x\] Parcela/g) || []).length;
  const total = (content.match(/- \[[ x]\] Parcela/g) || []).length;
  return { pagas, total };
}

for (let f of faturas) {
  const file = app.vault.getAbstractFileByPath(f.file.path);
  const { pagas, total } = await getParcelasPagas(file);
  const progresso = total > 0 ? (pagas / total) * 100 : 0;

  dv.el("div", `
  <div class="fatura-card">
    <div class="fatura-header">
      <h3>🧾 ${f.file.name.replace(".md", "")}</h3>
      <span class="status">${f.fatura.pagamento?.status || "Pendente"}</span>
    </div>
    <div class="fatura-info">
      <p><b>📅 Data:</b> ${f.fatura.data}</p>
      <p><b>💰 Total Bruto:</b> R$ ${Number(f.fatura.total_bruto).toFixed(2)}</p>
      <p><b>💸 Taxa:</b> R$ ${Number(f.fatura.taxa).toFixed(2)}</p>
      <p><b>🧮 Total com Taxa:</b> R$ ${Number(f.fatura.total_com_taxa).toFixed(2)}</p>
      <p><b>💳 Parcelas:</b> ${f.fatura.parcelas}x de R$ ${Number(f.fatura.valor_parcela).toFixed(2)}</p>
    </div>
    <div class="progress-wrapper">
      <div class="progress-bar" style="width:${progresso}%;"></div>
    </div>
    <p class="progress-text">${pagas}/${total} parcelas pagas (${progresso.toFixed(0)}%)</p>
  </div>
  `);
}

// 🌈 Estilos visuais
dv.el("style", `
.fatura-card {
  background: var(--background-secondary);
  padding: 1rem 1.2rem;
  border-radius: 12px;
  margin-bottom: 1rem;
  box-shadow: 0 2px 6px rgba(0,0,0,0.1);
}
.fatura-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.fatura-header h3 {
  margin: 0;
  font-size: 1.1rem;
}
.status {
  padding: 0.2rem 0.6rem;
  border-radius: 8px;
  background: var(--interactive-accent);
  color: white;
  font-size: 0.8rem;
}
.fatura-info p {
  margin: 0.2rem 0;
  font-size: 0.9rem;
}
.progress-wrapper {
  background: var(--background-modifier-border);
  border-radius: 8px;
  overflow: hidden;
  height: 8px;
  margin-top: 8px;
}
.progress-bar {
  height: 8px;
  background: var(--interactive-accent);
  transition: width 0.3s ease;
}
.progress-text {
  font-size: 0.8rem;
  margin-top: 4px;
  color: var(--text-muted);
}
`);

```
