# Analise de rede de grafos Olist

## Integrantes:
Getúlio Faria Segundo · Petrus César Nascimento Melo 

## Vídeo
https://youtu.be/... (até 15 min no total)

## Problema e pergunta:
O Olist é um e-commerce brasileiro com mais 100 mil pedidos no conjunto de dados público. A maior parte das compras traz itens de uma única categoria, mas alguns pedidos misturam categorias, e essas misturas dizem quais categorias costumam ser compradas juntas.

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
Para a modelagem serão utilizados duas redes, uma rede principal inicialmente bipartida de categorias x pedidos e uma rede complementar de vendedores × categorias.

# Rede principal

# Construção da Rede
Devido ao numero alto de pedidos no dataset, rede principal vai utilizar pedidos com duas ou mais categorias, o que reduz os para 0.7% as categorias que serão trabalhadas. O processo de contrução da rede foi feito mudando de bipartida para uma rede de co-ocorrência usando a matriz de incidência (BᵀB). Forma utilizadas duas ponderações nas arestas, uma usando o indice de Jaccard e outra usando a contagem. A visualização e as comunidades usam o backbone G_bb, que mantém as arestas com Jaccard acima da mediana.

O grafo principal não usa arestas direcionadas pois, arestas direcionadas seriam apropiadas para indicar pedidos em uma categoria, mas devido ao numero elevado de pedidos no dataset (112650) é mais apropiado utilizar os pedidos como peso das arestas construindo uma rede de co-ocorrencia.

# Rede Complementar

## O que ficou de fora
- Pedidos com apenas uma categoria ou sem categoria
- Pedidos cancelados ou indisponíveis

## Conteúdo do curso utilizado
| conceito | semana | onde aparece | para que serviu |
| tipos de grafo, atributos de nó e aresta | 6 | Olist_analise.ipynb, cél. 12 e 14 | modelar a rede como grafo não dirigido ponderado por Jaccard e Contagem |
| densidade e esparsidade | 2 | Olist_analise.ipynb, cél. 18 e 22 | medir o quão ligada é a rede |
| grau e distribuição de grau (histograma e CCDF) | 2 | Olist_analise.ipynb, cél. 18 e 20 | forma geral da rede e presença de hubs |
| matriz de adjacência (simetria) | 2 | Olist_analise.ipynb, cél. 18 e 22 | representação da rede e confirmação de que é não dirigida |
| componentes (WCC = SCC, pois é não dirigida) | 4 | Olist_analise.ipynb, cél. 24 | verificar fragmentação |
| clustering local e global (transitividade) | 4 | Olist_analise.ipynb, cél. 24 e 35 | coesão local |
| caminho médio, diâmetro, eficiência | 4 | Olist_analise.ipynb, cél. 24 | proximidade global |
| centro e periferia (excentricidade, raio) | 3 e 4 | Olist_analise.ipynb, cél. 31 | posição geométrica das categorias |
| closeness | 4 | Olist_analise.ipynb, cél. 34 | quem está mais próximo de todos |
| betweenness (sem peso e ponderada por Jaccard e contagem) | 4 | Olist_analise.ipynb, cél. 26, 27 e 33 | categorias-ponte |
| eigenvector | 4 | Olist_analise.ipynb, cél. 26 e 34 | importância por vizinhos |
| hubs e pontes (corte no percentil 90) | 4 | Olist_analise.ipynb, cél. 33 | separar hubs de pontes |
| visualização com nxviz (circos, arc) | 4 | Olist_analise.ipynb, cél. 49 e 50 | enxergar comunidades, hubs e pontes |
| k-core, core number, onion layers | 5 | Olist_analise.ipynb, cél. 37 | núcleo da rede |
| exportação para o Gephi e modularidade (usada para agrupar categorias) | 5 | Olist_analise.ipynb, cél. 46 e figuras/ | grupos de consumo e figura final |
| rede bipartida e projeção | 6 | Olist_analise.ipynb, cél. 12 e 14 | transformar pedidos × categorias em rede de categorias |
| matriz de incidência (BᵀB) | 6 | Olist_analise.ipynb, cél. 14 | calcular a projeção de forma eficiente |
| co-ocorrência | 6 | Olist_analise.ipynb, cél. 14 | definir arestas e peso por contagem |
| índice de Jaccard | 6 | Olist_analise.ipynb, cél. 14 | reduzir o viés de popularidade nos pesos |
| artefato de clique | 6 | Olist_analise.ipynb, cél. 40 | checar se pedidos grandes inflam a rede |
| esqueleto (backbone) por limiar de peso | 6 | Olist_analise.ipynb, cél. 43 | filtrar arestas fracas para visualizar e achar comunidades |

## Como executar
1-Baixar o dataset:
https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce
2-Criar a pasta /data e colocar todas as planilhas .csv do dataset na pasta
3-Clicar em "Executar tudo"

## Resultados


## Limitações e próximos passos

## Referências
https://github.com/ivanovitchm/datastructure/tree/main
The Atlas for the Aspiring Network Scientist: https://share.google/2B4XBybzJrTtSmwzs
