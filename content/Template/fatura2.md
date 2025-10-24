<%*
const n = parseInt(await tp.system.prompt("Quantos itens deseja adicionar?"));
const items = [];
let total = 0;

for (let i = 0; i < n; i++) {
  const quantidade = parseFloat(await tp.system.prompt(`Quantidade do item ${i + 1}`));
  const descricao = await tp.system.prompt(`Descrição do item ${i + 1}`);
  const preco = parseFloat(await tp.system.prompt(`Preço unitário do item ${i + 1}`));
  const subtotal = quantidade * preco;
  total += subtotal;
  items.push({ quantidade, descricao, preco, subtotal });
}

const taxa = parseFloat(await tp.system.prompt("Valor da taxa (R$)"));
const parcelas = parseInt(await tp.system.prompt("Número de parcelas"));
const totalComTaxa = total + taxa;
const valorParcela = totalComTaxa / parcelas;

// Gerando YAML com estilo
let yaml = `---
fatura:
  data: ${tp.date.now("YYYY-MM-DD")}
  total_bruto: ${total.toFixed(2)}
  taxa: ${taxa.toFixed(2)}
  total_com_taxa: ${totalComTaxa.toFixed(2)}
  parcelas: ${parcelas}
  valor_parcela: ${valorParcela.toFixed(2)}
  parcelas_faltando: ${parcelas}
  itens:
`;

for (let item of items) {
  yaml += `    - quantidade: ${item.quantidade}\n`;
  yaml += `      descricao: "${item.descricao}"\n`;
  yaml += `      preco_unitario: ${item.preco.toFixed(2)}\n`;
  yaml += `      subtotal: ${item.subtotal.toFixed(2)}\n`;
}

yaml += `  pagamento:\n    metodo: \n    banco:\n    status: Pendente\n---\n\n`;

// Geração do corpo visual
let output = `# 🧾 Fatura\n\n`;
output += `**📅 Data:** ${tp.date.now("DD/MM/YYYY")}\n`;
output += `**💰 Total Bruto:** R$ ${total.toFixed(2)}\n`;
output += `**💸 Taxa:** R$ ${taxa.toFixed(2)}\n`;
output += `**🧮 Total com Taxa:** R$ ${totalComTaxa.toFixed(2)}\n`;
output += `**💳 Parcelado em ${parcelas}x de R$ ${valorParcela.toFixed(2)}**\n\n`;

output += `---\n\n## 📋 Detalhamento\n\n`;
output += `| # | Descrição | Qtd | Preço Unit. | Subtotal |\n`;
output += `|:-:|------------|:---:|:------------:|----------:|\n`;

for (let i = 0; i < items.length; i++) {
  const item = items[i];
  output += `| ${i + 1} | ${item.descricao} | ${item.quantidade} | R$ ${item.preco.toFixed(2)} | R$ ${item.subtotal.toFixed(2)} |\n`;
}

output += `\n---\n\n## 💵 Controle de Parcelas\n\n`;

for (let i = 1; i <= parcelas; i++) {
  output += `- [ ] Parcela ${i}\n`;
}

output += `\n> 🧠 **Resumo:** ${parcelas} parcelas de R$ ${valorParcela.toFixed(2)} = **R$ ${totalComTaxa.toFixed(2)}**\n`;

tR = yaml + output;
%>
