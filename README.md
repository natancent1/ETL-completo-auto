# Análise de Vendas - Projeto de Portfólio de Business Intelligence de Ponta a Ponta

**Autor:** Natanael (Natan) Vicente
**LinkedIn:** https://www.linkedin.com/in/natanael-vicente-4b3b0a97/
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


<img width="600" height="329" alt="view1" src="https://github.com/user-attachments/assets/dafb487d-721b-4e99-af1f-784b11fe1480" />

---


**Página 2: Relatório Detalhado**
*Aprofunda a análise de performance por Categoria, Vendedor e Dia da Semana, além de uma tabela Filial vs. Produto.*


<img width="476" height="268" alt="view2" src="https://github.com/user-attachments/assets/74fa1a3b-bc85-4d4a-944e-e6ea6892d5dd" />

---


**Página 3: Análise de Produtos**
*Uma página de "deep dive" para identificar produtos "estrela" e oportunidades de portfólio através de uma matriz de desempenho.*


<img width="464" height="260" alt="view3" src="https://github.com/user-attachments/assets/a4939fe9-ffb3-41b4-b020-5cded0d314c3" />


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
  let
    Fonte = #table(
        type table [nome_produto=text, produto_padronizado=text],
        {
            // Variações de Refrigerante Sabor Framboesa 2L
            {"Refrigerante Sabor Framboesa", "Refrigerante Framboesa 2L"},
            {"SABOR FRAMBOESA", "Refrigerante Framboesa 2L"},
            {"Refri Framb.", "Refrigerante Framboesa 2L"},
            {"Sabor Framboesa", "Refrigerante Framboesa 2L"},
            {"REFRI FRMB", "Refrigerante Framboesa 2L"},
            {"refrigerante sabor framboesa", "Refrigerante Framboesa 2L"},
            {"RefrigeranteSaborFramboesa", "Refrigerante Framboesa 2L"},
            {"Refri Frmbsa", "Refrigerante Framboesa 2L"},
            {"Refrígerante Sábor Framboesa", "Refrigerante Framboesa 2L"},
            {"Refrigeránte Sábor Frámboesá", "Refrigerante Framboesa 2L"},
            {"REFRIGERANTE SABOR FRAMBOESA", "Refrigerante Framboesa 2L"},
            {"Framboesa Refri 2L", "Refrigerante Framboesa 2L"},
            {"Refrigerante Framboesa", "Refrigerante Framboesa 2L"},

            // Variações de Soda Limão
            {"Soda Limao", "Soda Limão"},
            {"Soda Limão", "Soda Limão"},
            {"SODA LIMAO", "Soda Limão"},
            {"soda limão", "Soda Limão"},
            {"SODA LIMÃO", "Soda Limão"},
            {"Soda Lim.", "Soda Limão"},
            {"Soda Lmão", "Soda Limão"},
            {"SodaLimão", "Soda Limão"},
            {"Sóda Límão", "Soda Limão"},
            {"Soda Limao Garrafa", "Soda Limão"},
            
            // Variações de Água Tônica Clássica
            {"Agua Tonica", "Água Tônica Clássica"},
            {"Água Tônica", "Água Tônica Clássica"},
            {"TONICA GOLD", "Água Tônica Clássica"},
            {"água tônica", "Água Tônica Clássica"},
            {"Ag. Tônica", "Água Tônica Clássica"},
            {"Aguá Tonica", "Água Tônica Clássica"},
            {"ÁguaTônica", "Água Tônica Clássica"},
            {"Água Tôníca", "Água Tônica Clássica"},
            {"Águá Tônicá", "Água Tônica Clássica"},
            {"ÁGUA TÔNICA", "Água Tônica Clássica"},
            {"Tonica", "Água Tônica Clássica"},
            {"Água Tônica Clássica", "Água Tônica Clássica"},

            // Variações de Refrigerante de Gengibre 2L
            {"Refri Gengibre 2L", "Refrigerante Gengibre 2L"},
            {"REFRI GENGIBRE 2L", "Refrigerante Gengibre 2L"},
            {"Refri de Gengibre", "Refrigerante Gengibre 2L"},
            {"Gengibre 2L", "Refrigerante Gengibre 2L"},
            {"Gengibira", "Refrigerante Gengibre 2L"},
            {"Refri Gengibira", "Refrigerante Gengibre 2L"},
            {"Refrí Gengíbírra 2L", "Refrigerante Gengibre 2L"},
            {"RefriGengibre2L", "Refrigerante Gengibre 2L"},
            {"refri gengibre 2l", "Refrigerante Gengibre 2L"},
            {"Refri Gengibrá 2L", "Refrigerante Gengibre 2L"},
            {"Gengibre Refri", "Refrigerante Gengibre 2L"},
            {"Refrigerante Gengibre", "Refrigerante Gengibre 2L"},
            
            // Variações de Salgadinho de Cebola 100g
            {"Salgadinho Cebola 100g", "Salgadinho Cebola 100g"},
            {"Salgadinho Cebola 100G", "Salgadinho Cebola 100g"},
            {"Salg. Cebola", "Salgadinho Cebola 100g"},
            {"Salgadinho de Cebola", "Salgadinho Cebola 100g"},
            {"SALG CEBOLA", "Salgadinho Cebola 100g"},
            {"Sálgádinho Cebolá 100g", "Salgadinho Cebola 100g"},
            {"Salg Cebola 100g", "Salgadinho Cebola 100g"},
            {"Salg. Ceb.", "Salgadinho Cebola 100g"},
            {"salgadinho cebola 100g", "Salgadinho Cebola 100g"},
            {"SALGADINHO CEBOLA 100G", "Salgadinho Cebola 100g"},
            {"SalgadinhoCebola100g", "Salgadinho Cebola 100g"},
            {"Salgadínho Cebola 100g", "Salgadinho Cebola 100g"},
            {"Cebolitos 100g", "Salgadinho Cebola 100g"},
            
            // Variações de Barra de Chocolate 90g
            {"Chocolate Barra 90g", "Barra de Chocolate 90g"},
            {"Barra Chocolate 90g", "Barra de Chocolate 90g"},
            {"Chocolate em Barra", "Barra de Chocolate 90g"},
            {"Barra de Choc.", "Barra de Chocolate 90g"},
            {"CHOC BARRA 90G", "Barra de Chocolate 90g"},
            {"Chocolate Barra 90G", "Barra de Chocolate 90g"},
            {"chocolate barra 90g", "Barra de Chocolate 90g"},
            {"Choc. 90g", "Barra de Chocolate 90g"},
            {"Chocoláte Bárrá 90g", "Barra de Chocolate 90g"},
            {"ChocolateBarra90g", "Barra de Chocolate 90g"},
            {"CHOCOLATE BARRA 90G", "Barra de Chocolate 90g"},
            {"Barra de Chocolate", "Barra de Chocolate 90g"},

            // Variações de Suco de Laranja 1L
            {"Suco Laranja 1L", "Suco de Laranja 1L"},
            {"Suco de Laranja", "Suco de Laranja 1L"},
            {"SUCO LARANJA", "Suco de Laranja 1L"},
            {"SucoLaranja1L", "Suco de Laranja 1L"},
            {"Suco Lrng 1L", "Suco de Laranja 1L"},
            {"Suco Laranj 1L", "Suco de Laranja 1L"},
            {"suco laranja 1l", "Suco de Laranja 1L"},
            {"Suco Láránjá 1L", "Suco de Laranja 1L"},
            {"SUCO LARANJA 1L", "Suco de Laranja 1L"},
            {"Suco de Laranja Garrafa", "Suco de Laranja 1L"}
        }
    ),
    LinhasDuplicadasRemovidas = Table.Distinct(Fonte, {"nome_produto"}),
    #"Duplicatas Removidas" = Table.Distinct(LinhasDuplicadasRemovidas, {"nome_produto"}),
    #"Duplicatas Removidas1" = Table.Distinct(#"Duplicatas Removidas", {"nome_produto"}),
    #"Linhas em Branco Removidas" = Table.SelectRows(#"Duplicatas Removidas1", each not List.IsEmpty(List.RemoveMatchingItems(Record.FieldValues(_), {"", null}))),
    #"Colocar Cada Palavra Em Maiúscula" = Table.TransformColumns(#"Linhas em Branco Removidas",{{"nome_produto", Text.Proper, type text}}),
    #"Texto Aparado" = Table.TransformColumns(#"Colocar Cada Palavra Em Maiúscula",{{"produto_padronizado", Text.Trim, type text}}),
    #"Texto Limpo" = Table.TransformColumns(#"Texto Aparado",{{"produto_padronizado", Text.Clean, type text}})
in
    #"Texto Limpo"
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

## Visualização e Design

### Tema Personalizado
- Foi criado um **tema JSON personalizado** para garantir consistência visual:  
  - Paleta de cores da marca  
  - Fontes padrão  
  - Bordas arredondadas e sombras aplicadas em todos os visuais
 

<img width="292" height="275" alt="view5" src="https://github.com/user-attachments/assets/9af54ead-32b8-46ab-870c-50546a28ddf1" />
<img width="285" height="274" alt="view4" src="https://github.com/user-attachments/assets/a51562c6-9ac0-473c-8e0a-b54c034ee1aa" />


### Navegação Intuitiva
- Utilização de **Navegador de Páginas** no Power BI  
- As abas padrão foram ocultadas, criando uma experiência mais fluida e semelhante a um **aplicativo**  

### UX Focado em Insights
- Visuais escolhidos e posicionados estrategicamente para **responder perguntas de negócio chave**  
- **KPIs no topo** → visão rápida de desempenho  
- Abaixo → análises de **tendência** e **rankings**  
- Uso de **formatações condicionais** (heatmaps, destaques em gráficos) para guiar o olhar do usuário para os **insights mais importantes**

---

# ✅ Conclusão e Próximos Passos

Este projeto representa uma **simulação completa do ciclo de vida de uma solução de Business Intelligence**, desde a **ingestão de dados caóticos** até a entrega de um **dashboard analítico, interativo e acionável**.  

A arquitetura implementada (**Staging, Modelo Estrela, Dicionários de Dados**) garante que a solução não seja apenas um **relatório estático**, mas um sistema de análise:  
- Dinâmico  
- Escalável  
- De fácil manutenção  

---

## 🔮 Possíveis Melhorias Futuras
- **Criação de Dataflows** → Migrar o processo de ETL do Power Query para o Power BI Service, criando uma **fonte de dados centralizada e reutilizável**  
- **Análise de Rentabilidade** → Incorporar uma `Dim_Custos` para calcular a **margem de lucro por produto e filial**, indo além do faturamento  
- **Atualização Incremental** → Configurar incremental refresh na tabela de **Fato**, otimizando o tempo de carregamento com **grandes volumes de dados** (recurso Premium/PPU)  

---

📌 **Obrigado por conferir este projeto!**  
Sinta-se à vontade para entrar em contato através do meu LinkedIn para qualquer dúvida ou feedback.

https://www.linkedin.com/seu-perfil](https://www.linkedin.com/in/natanael-vicente-4b3b0a9
