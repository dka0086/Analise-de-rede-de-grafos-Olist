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
Devido ao numero alto de pedidos no dataset, rede principal vai utilizar pedidos com duas ou mais categorias, o que reduz os para 0.7% as categorias que serão trabalhadas. O processo de contrução da rede foi feito mudando de bipartida para uma rede de co-ocorrência usando a matriz de incidência (BᵀB). Forma utilizadas duas ponderações nas arestas, uma usando o indice de Jaccard e outra usando a contagem. A visualização e as comunidades usam o backbone G_bb, que mantém as arestas com Jaccard acima da mediana.

O grafo principal não usa arestas direcionadas pois, arestas direcionadas seriam apropiadas para indicar pedidos em uma categoria, mas devido ao numero elevado de pedidos no dataset (112650) é mais apropiado utilizar os pedidos como peso das arestas construindo uma rede de co-ocorrencia.

## O que ficou de fora
- Pedidos com apenas uma categoria ou sem categoria
- Pedidos cancelados ou indisponíveis

## Conteúdo do curso utilizado
| Conceito | Semana | Onde aparece | Para que serviu |
| :--- | :---: | :--- | :--- |
| modelagem de rede | 2 | Olist_analise.ipynb, cél. 12 | criação da rede bipartida definindo nós e conexões |
| ponderação de arestas | 6 | Olist_analise.ipynb, cél. 14 | ponderando arestas por contagem e jaccard |
| grau e densidade | 2 | Olist_analise.ipynb, cél. 19 a 24 | medir grau e densidade e visualização usando distribuições |
| componentes, clustering e caminhos | 4 | Olist_analise.ipynb, cél. 26 | medir clustering medio e camninho médio |
| hubs, pontes e centralidades | 4 | Olist_analise.ipynb, cél. 28 e 29 | calculando betweenness e grau, e listando hubs e pontes em uma tabela |
| k-core, core number, onion layers | 5 | Olist_analise.ipynb, cél. 36 | núcleo da rede |
| artefato de clique | 6 | Olist_analise.ipynb, cél. 39 | encontando cliques presentes na rede e listando os nós que os compôe |
| backbone por limiar de peso | 6 | Olist_analise.ipynb, cél. 41 | backbone utilizando uma ponderação de corte |
| visaulização arc plot e circus plot | 4 | Olist_analise.ipynb, cél. 44 e 45 | visualização usando circus plot e arc plot |


## Como executar
1. Baixar o dataset:https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce.
2. Criar a pasta /data e colocar todas as planilhas .csv do dataset na pasta.
3. Clicar em "Executar tudo" e esperar baixar as dependências.

## Resultados
**A rede responde a pergunta?** A análise das métricas mostrou que diferentes categorias respondem a partes diferentes da pergunta. Os hubs, categorias com mais ligações, são móveis (`furniture_decor`), com grau 30, e artigos têxteis e de decoração para o lar (`bed_bath_table`), com grau 27, e são os que mais aparecem juntos com outras categorias. Mas a categoria que mais fica n caminho de outras é a `baby` com betweenness ponderado por Jaccard de 0,262, sendo o 7º maior grau. Ou seja, quem tem mais ligações não necessariamente conecta mais partes diferentes da rede.

**O que a modelagem mudou.** O uso de pesos nas arestas permitiu um estudo mais elaborado da rede possibilitando um ranqueamento mais sofisticado dos nós, a ponderação mais utilizada para o calculo das métrics foi por Jaccard, que faz ligações de afinidade forte encontrarem caminhos curtos. Por isso a betweenness ponderada aponta pontes (`baby`, `cool_stuff`) que a betweenness sem peso não destaca. 

**Conclusão** A rede apontam quais categorias costumam ser compradas juntas e quais ligam grupos diferentes, o que ajuda a criar uma forma eficiente de cruzar as recomendações. 

## Limitações e próximos passos
A rede usada tinha 0,7% dos pedidos (pedidos com mais de uma categoria).

## Referências
1. https://github.com/ivanovitchm/datastructure/tree/main.
2. The Atlas for the Aspiring Network Scientist:https://share.google/2B4XBybzJrTtSmwzs.
