# Análise de Vendas de Restaurante

> 🚧 **Projeto em andamento.** Etapa atual: limpeza dos dados.

Relatório de vendas para restaurantes, feito em Python com pandas. O objetivo é transformar o histórico de vendas em respostas práticas para o dono do restaurante: o que mais vende, quando o movimento é maior e como o faturamento está evoluindo.

Este projeto usa um dataset **fictício** como demonstração.

## O problema

Muitos restaurantes pequenos registram as vendas no sistema do caixa, mas não usam esses dados para tomar decisões. Perguntas simples, como "qual prato traz mais dinheiro?" ou "qual dia precisa de mais funcionários?", ficam sem resposta, mesmo com a informação já disponível.

## Os dados

O arquivo [`vendas_restaurante.csv`](vendas_restaurante.csv) traz 3 meses de vendas (agosto a outubro de 2026) de um restaurante que funciona de terça a domingo, com **8.506 linhas** e **10 colunas**.

Cada linha representa **um item dentro de um pedido**. Um mesmo pedido pode ter várias linhas.

| Coluna | Descrição |
|---|---|
| `id_pedido` | Identificador do pedido |
| `data` | Data da venda |
| `hora` | Horário da venda |
| `canal` | Salão, Balcão ou Delivery iFood |
| `item` | Nome do produto |
| `categoria` | Pratos, Bebidas, Porções ou Sobremesas |
| `quantidade` | Quantidade do item no pedido |
| `preco_unitario` | Preço de uma unidade |
| `valor_total` | Quantidade × preço unitário |
| `forma_pagamento` | Como o cliente pagou |

Assim como acontece em dados reais, o dataset tem problemas de qualidade que precisam ser tratados antes da análise.

## Perguntas de negócio

1. Quais itens mais vendem e quais mais faturam?
2. Quais dias da semana e horários são mais fortes?
3. Qual é o ticket médio (faturamento dividido pelo número de pedidos)?
4. O faturamento está subindo ou caindo ao longo dos meses?
5. Curva ABC: quais poucos itens trazem a maior parte do faturamento?

## Etapas

- [x] **1. Exploração inicial**: entender a estrutura, os tipos e a qualidade dos dados
- [ ] **2. Limpeza**: corrigir os problemas encontrados na exploração *(em andamento)*
- [ ] **3. Análise**: responder às 5 perguntas de negócio
- [ ] **4. Painel**: visualização interativa no Looker Studio
- [ ] **5. Conclusões**: recomendações para o restaurante

## Resultados da exploração inicial

> Valores **preliminares**, calculados antes da limpeza. Podem mudar depois que os problemas dos dados forem corrigidos.

| Indicador | Valor |
|---|---|
| Linhas (itens vendidos) | 8.506 |
| Pedidos distintos | 4.243 |
| Faturamento do período | R$ 205.868,00 |
| Ticket médio | R$ 48,52 |

### O que aprendemos sobre os dados

- **Uma linha não é um pedido.** Cada linha é um item, e um pedido tem em média 2 itens. Por isso, o número de pedidos é calculado contando os valores distintos de `id_pedido`, e não as linhas.
- **Esse cuidado muda o resultado.** Se as linhas fossem contadas como pedidos, o ticket médio cairia para R$ 24,20, cerca de metade do valor real.
- **Datas e horários vieram como texto.** As colunas `data` e `hora` precisam ser convertidas para que seja possível analisar meses, dias da semana e horários.
- **Há formas de pagamento faltando.** A coluna `forma_pagamento` tem 15 valores vazios. Essas vendas aconteceram, então as linhas **não** devem ser apagadas, para não reduzir o faturamento.

## Limpeza dos dados *(em andamento)*

Regras seguidas: o arquivo original nunca é alterado, cada problema é investigado antes de ser corrigido, e toda correção é verificada.

| Problema | Decisão | Situação |
|---|---|---|
| **Nomes de itens escritos de formas diferentes**: 34 nomes para 18 itens, por espaços sobrando e letras minúsculas (ex.: `'Refrigerante Lata '` e `'refrigerante lata'`) | Remover espaços com `strip()` e corrigir as 8 versões em minúsculo com um dicionário. O `title()` foi descartado porque estragava nomes como "Porção De Calabresa" | ✅ Resolvido |
| **Linhas duplicadas**: 6 itens registrados duas vezes, que inflavam o faturamento em R$ 123,00 | Remover as cópias. Como duas unidades de um item ficam numa só linha (`quantidade` = 2), linhas idênticas não são vendas reais | ✅ Resolvido |
| **`data` e `hora` como texto** | Converter a `data` para o tipo de data. A `hora` não foi convertida direto, porque o pandas inventaria uma data para completá-la: em vez disso, criei a coluna `hora_cheia` (10, 11, ..., 22), ideal para mostrar os horários de pico no dashboard. Validação: nenhuma venda numa segunda-feira, dia em que o restaurante fecha | ✅ Resolvido |
| **15 formas de pagamento vazias** | Manter as linhas, porque as vendas aconteceram | ⏳ Pendente |

Após as correções feitas até agora, a base tem **8.500 linhas**, **4.243 pedidos**, faturamento de **R$ 205.745,00** e ticket médio de **R$ 48,49** (ainda preliminares).

## Tecnologias

- Python 3.13
- pandas
- Jupyter Notebook
- Looker Studio *(próxima etapa)*

## Como rodar

```bash
git clone https://github.com/LufeCovizzi/analise-vendas-restaurante.git
cd analise-vendas-restaurante
python -m venv .venv
.venv\Scripts\activate        # Windows
pip install -r requirements.txt
```

Depois, abra o `analise.ipynb` no Jupyter ou no VS Code e execute as células em ordem.

## Autor

**Luiz Fernando**: [GitHub](https://github.com/LufeCovizzi)
