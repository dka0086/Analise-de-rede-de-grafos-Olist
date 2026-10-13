# Analise de rede de grafos Olist

## Integrantes:
Getúlio Faria Segundo Petrus César · Nascimento Melo 

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
| betweenness | 4 | analise.ipynb, cél. 12 | trechos críticos |
## Como executar
## Resultados
## Limitações e próximos passos
## Referências
