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

// Gerando YAML
let yaml = `---
data: ${tp.date.now("YYYY-MM-DD")}
total_bruto: ${total.toFixed(2)}
taxa: ${taxa.toFixed(2)}
total_com_taxa: ${totalComTaxa.toFixed(2)}
parcelas: ${parcelas}
valor_parcela: ${valorParcela.toFixed(2)}
itens:\n`;

for (let item of items) {
  yaml += `  - quantidade: ${item.quantidade}\n    descricao: "${item.descricao}"\n    preco: ${item.preco.toFixed(2)}\n    subtotal: ${item.subtotal.toFixed(2)}\n`;
}
yaml += `---\n\n`;

let output = `## Fatura\n\n`;
output += `**Data:** ${tp.date.now("DD/MM/YYYY")}\n`;
output += `**Total bruto:** R$ ${total.toFixed(2)}\n`;
output += `**Taxa:** R$ ${taxa.toFixed(2)}\n`;
output += `**Total com taxa:** R$ ${totalComTaxa.toFixed(2)}\n`;
output += `**Parcelado em ${parcelas}x de R$ ${valorParcela.toFixed(2)}**\n\n`;

output += `---\n\n### Detalhamento\n\n`;
output += `| Quantidade | Descrição | Preço Unitário | Total |\n`;
output += `|------------|------------|----------------|--------|\n`;

for (let item of items) {
  output += `| ${item.quantidade} | ${item.descricao} | R$ ${item.preco.toFixed(2)} | R$ ${item.subtotal.toFixed(2)} |\n`;
}

output += `\n---\n\n`;

for (let i = 1; i <= parcelas; i++) {
  output += `- [ ] Parcela ${i}\n`;
}

tR = yaml + output;
%>
