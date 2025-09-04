# Análise de Vendas - Projeto de Portfólio de Business Intelligence de Ponta a Ponta

**Autor:** [Seu Nome Completo]
**LinkedIn:** [https://www.linkedin.com/in/seu-perfil/](https://www.linkedin.com/in/seu-perfil/)
**Data:** Setembro, 2025

---

### **⚠️ Disclaimer Importante**

**Os dados utilizados neste projeto são 100% fictícios** e foram gerados para fins educacionais e de demonstração de habilidades. Eles não representam, de forma alguma, dados reais de vendas, preços ou operações de qualquer empresa. O projeto tem como único objetivo a construção de um portfólio de Business Intelligence.

---

### **Visão Geral do Projeto**

Este projeto de Business Intelligence (BI) de ponta a ponta simula uma análise de desempenho de vendas para uma empresa fictícia do setor de bebidas e snacks. O objetivo foi transformar um conjunto de dados brutos, caóticos e inconsistentes em um dashboard interativo, automatizado e que gerasse insights acionáveis, seguindo rigorosamente as melhores práticas de ETL e modelagem de dados.

O resultado é uma solução robusta que não apenas visualiza dados, mas também garante sua qualidade, performance e facilidade de manutenção.

### **Dashboard Final**

**Página 1: Panorama Geral**
*Oferece uma visão executiva dos KPIs mais importantes e tendências macro.*
![Panorama Geral](https://caminho/para/sua/imagem/01_visao_geral.png)

**Página 2: Relatório Detalhado**
*Aprofunda a análise de performance por Categoria, Vendedor e Dia da Semana, além de um heatmap de Filial vs. Produto.*
![Relatório Detalhado](https://caminho/para/sua/imagem/02_analise_detalhada.png)

**Página 3: Análise de Produtos**
*Uma página de "deep dive" para identificar produtos "estrela" e oportunidades de portfólio através de uma matriz de desempenho.*
![Análise de Produtos](https://caminho/para/sua/imagem/03_analise_produtos.png)

---

### **Como Executar o Projeto**

Este relatório foi construído para ser 100% portátil.

1.  **Baixe o Projeto:** Faça o download do ZIP deste repositório e descompacte em seu computador.
2.  **Abra o Relatório:** Navegue até a pasta `/02_Relatorio_PowerBI/` e abra o arquivo `relatorio_vendas.pbix`.
3.  **Configure o Caminho:** Vá em **Transformar dados > Editar Parâmetros**. No campo `pCaminhoPastaDados`, cole o caminho completo da pasta `01_Dados` em seu computador e clique em OK.
4.  **Atualize:** Clique em **Fechar e Aplicar** e, em seguida, no botão **Atualizar** na página principal.

---

### **Arquitetura da Solução e Habilidades Técnicas**

A solução foi construída sobre uma arquitetura em camadas dentro do Power Query, culminando em um **Modelo Estrela (Star Schema)**.

**1. Extração e Staging:**
   - **Técnica:** Conexão dinâmica com uma **Pasta**, permitindo a ingestão automática de múltiplos arquivos CSV. Isso cria uma tabela de `Staging` que serve como ponto único de entrada de dados brutos.

**2. Transformação (ETL em Power Query - Linguagem M):**
   - **Limpeza Robusta:** Aplicação de `Text.Trim` e `Text.Clean` para remover espaços e caracteres ocultos; substituição de valores para corrigir separadores decimais (`.` para `,`); e filtragem para remover linhas inválidas.
   - **Padronização com Dicionários:** Criação de tabelas de mapeamento "De-Para" para corrigir mais de 50 variações de nomes de produtos e filiais.
   - **Tipagem com Localidade:** Uso de `Table.TransformColumnTypes` com Localidade específica (ex: "en-US") para tratar corretamente datas e valores decimais com ponto, evitando os erros de conversão mais comuns.

   **Código M - Exemplo de Dicionário de Produtos (Anonimizado):**
   ```m
   // Cole aqui o código M completo do seu dicionario_produtos anonimizado.
   ```

**3. Modelagem de Dados (Star Schema):**
   - **Tabelas de Dimensão:** Criação das tabelas `Dim_Produtos`, `Dim_Filiais`, `Dim_Vendedores`, e `Dim_Data` (gerada com DAX via `CALENDARAUTO()`), servindo como fontes únicas da verdade.
   - **Tabela Fato:** Construção de uma `Fato_Vendas` otimizada, contendo apenas chaves estrangeiras (IDs) e métricas numéricas.
   - **Relacionamentos:** Conexões "Um-para-Muitos" (1:*) criadas na Exibição de Modelo, ligando as Dimensões à Fato.

**4. Análise e Cálculos (DAX):**
   - **Medidas Centralizadas:** Criação de uma tabela dedicada `_Medidas` para organizar todos os cálculos, como:

   **Código DAX - Faturamento Total:**
   ```dax
   Faturamento Total = 
   SUMX(
       Fato_Vendas,
       Fato_Vendas[quantidade] * Fato_Vendas[preco_unitario]
   )
   ```

   **Código DAX - Destaque Condicional:**
   ```dax
   Cor Destaque Top Produto = 
   VAR VendasProdutoAtual = [Quantidade Vendida]
   VAR TabelaResumo =
       SUMMARIZE (
           ALLSELECTED ( Fato_Vendas ),
           Dim_Produtos[nome_produto],
           "@SomaVendas", [Quantidade Vendida]
       )
   VAR VendaMaxima = MAXX ( TabelaResumo, [@SomaVendas] )
   RETURN
   IF ( VendasProdutoAtual = VendaMaxima, "#D81B4B", "#2A2A5E" )
   ```
