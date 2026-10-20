# Analise de rede de grafos Olist

## Integrantes:
Getúlio Faria Segundo · Petrus César Nascimento Melo 

## Vídeo
https://youtu.be/16SBalEZ6a0

## Problema e pergunta:
O Olist é um e-commerce brasileiro com quase 100 mil pedidos no conjunto de dados público. A maior parte das compras traz itens de uma única categoria, mas alguns pedidos misturam categorias, e essas misturas dizem quais categorias costumam ser compradas juntas.

Pergunta: quais categorias de produto fazem a ponte entre grupos de consumo no e-commerce brasileiro, e os hubs mais populares são também os que mais conectam grupos?

## Dados (fonte, como obter, limpeza)
Os dados utilizados estão no https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce

# Como obter:
Baixar o dataset olist_customers_dataset.csv no Kaggle, descompactar e colocar os .csv na pasta data/. Os dados não estão versionados no repositório.

## Limpeza
- Retirada de pedidos que possuiam os status "canceled" e "unavailable"
- Substituição de todas as categorias que estavam com nome em protuguês para inglês para padronizar a análise.
- Itens sem categoria são descartados.

## Modelagem (nó, aresta, peso, direção, o que ficou de fora)
Para a modelagem foi construida uma rede principal inicialmente bipartida de categorias x pedidos.

# Rede principal
A rede principal representa a co-compra de categorias de produto. Cada nó é uma categoria (61 das 73 entram na rede) e cada aresta liga duas categorias que aparecem no mesmo pedido (co-ocorrência), o que dá 243 arestas. Ela nasce de uma rede bipartida de pedidos × categorias, com apenas os 726 pedidos (0,7% dos 96.824 pedidos com categoria) que têm duas ou mais categorias distintas, e é projetada sobre as categorias pela matriz de incidência (BᵀB). A hipótese é que dois produtos no mesmo pedido indicam compra conjunta, então as categorias se relacionam.

As arestas têm dois pesos: a contagem de pedidos em comum (G_cont) e o índice de Jaccard (G_jac), que divide essa contagem pelos pedidos que têm uma ou outra categoria e corrige o viés de popularidade. A rede é não dirigida, porque estar no mesmo pedido é uma relação simétrica e os itens de um pedido chegam juntos, sem ordem entre eles. A análise estrutural usa G_jac, e a visualização (circos e arc plot) usa o backbone G_bb, que mantém as arestas com Jaccard acima da mediana.

## O que ficou de fora
- Pedidos com apenas uma categoria ou sem categoria
- Pedidos cancelados ou indisponíveis

## Conteúdo do curso utilizado
| Conceito | Semana | Onde aparece | Para que serviu |
| :--- | :---: | :--- | :--- |
| modelagem de rede | 2 | Olist_analise.ipynb, cél. 12 | criação da rede bipartida definindo nós e conexões |
| ponderação de arestas | 6 | Olist_analise.ipynb, cél. 14 | ponderando arestas por contagem e jaccard |
| grau e densidade | 2 | Olist_analise.ipynb, cél. 19 a 24 | medir grau e densidade e visualização usando distribuições |
| GCC, clustering e caminhos | 4 | Olist_analise.ipynb, cél. 26 | medir clustering medio e camninho médioda rede, e bucando GCC (Giant connected component) |
| hubs, pontes e centralidades | 4 | Olist_analise.ipynb, cél. 28 e 29 | calculando betweenness e grau, e listando hubs e pontes em uma tabela |
| k-core, core number, onion layers | 5 | Olist_analise.ipynb, cél. 36 | núcleo da rede |
| artefato de clique | 6 | Olist_analise.ipynb, cél. 39 | encontando cliques presentes na rede e listando os nós que os compôe |
| backbone por limiar de peso | 6 | Olist_analise.ipynb, cél. 41 | backbone utilizando uma ponderação de corte |
| visaulização arc plot e circus plot | 4 | Olist_analise.ipynb, cél. 44 e 45 | visualização usando circus plot e arc plot |


## Como executar
1. Baixar o dataset:https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce e descompactar.
2. No notebook, crie a pasta /data e coloque todas as planilhas .csv do dataset na pasta.
3. Clicar em "Executar tudo" e esperar baixar as dependências.

## Resultados
**A rede responde a pergunta?** A análise das métricas mostrou que diferentes categorias respondem a partes diferentes da pergunta. Os hubs, categorias com mais ligações são: furniture_decor (grau 30), bed_bath_table (27), housewares (25), sports_leisure (24), health_beauty (22), computers_accessories (21), auto, baby e garden_tools (20). Mas a categoria que mais fica no caminho entre outras é a baby, com betweenness ponderada por Jaccard de 0,262, apesar de ter só o 7º maior grau. As pontes que não são hubs são cool_stuff, stationery e toys: têm betweenness alta e grau mais baixo (stationery tem grau 11 e betweenness sem peso de apenas 0,018, mas 0,135 na ponderada). Ou seja, quem tem mais ligações não é necessariamente quem conecta mais partes diferentes da rede, embora as duas medidas andem juntas.

**Núcleo da rede** O k-core máximo é 8, e o núcleo mais interno tem 18 categorias. Nenhuma categoria de grau alto fica fora dele, então os hubs formam o miolo da rede. As pontes cool_stuff, stationery e toys também estão no núcleo, mesmo com grau mais baixo, porque o core number depende de os vizinhos também serem bem ligados. 

**Cliques** Há 110 cliques maximais, e o maior tem 8 categorias. Nenhum pedido tem mais de 3 categorias (711 têm 2 e só 15 têm 3), então esse clique nasce da soma de vários pedidos, e não de um pedido grande. O artefato de clique é, portanto, pequeno, e não foi preciso limitar o tamanho do pedido.

**Backbone** O corte na mediana do Jaccard de 50% (limiar 0,0192) reduz as arestas de 243 para 122. Sete categorias perdem todas as ligações, porque só tinham arestas fracas. O circos agrupa as categorias por papel (hub, ponte ou outra) e destaca as arestas que tocam uma ponte, e o arc plot as ordena por grau.

**O que a modelagem mudou.** O uso de pesos nas arestas permitiu um estudo mais elaborado da rede possibilitando um ranqueamento mais sofisticado dos nós. A ponderação por Jaccard, com a distância definida como 1/peso, faz ligações de afinidade forte virarem caminhos curtos. Por isso a betweenness ponderada aponta pontes (baby, cool_stuff, stationery, toys) que a betweenness sem peso não destaca. A escolha da rede não dirigida e do Jaccard muda a resposta, e não só a aparência dos gráficos.

**Conclusão** A rede apontam quais categorias costumam ser compradas juntas e quais ligam grupos diferentes, o que ajuda a criar uma forma eficiente de cruzar as recomendações. Hubs e pontes se sobrepõem em parte (baby é hub e tem a maior betweenness), mas também aparecem pontes que não são hubs (cool_stuff, stationery, toys), e essas são candidatas a recomendações cruzadas entre grupos de categorias.

## Limitações e próximos passos
- A rede usada tinha 0,7% dos pedidos (pedidos com mais de uma categoria).
- Os dados cobrem um único marketplace entre 2016 a 2018.

## Referências
1. Material da disciplina:https://github.com/ivanovitchm/datastructure/tree/main.
2. The Atlas for the Aspiring Network Scientist:https://share.google/2B4XBybzJrTtSmwzs.
