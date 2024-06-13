# Limpeza

## Retirando coluna de termo de consentimento
Utilizei um ``` for ``` e um ```if``` para retirar a coluna dos termo de consentimento que era muito grande para escrever manualmente.

Segue trecho de código que utlizei para aplicar a remoção:

```
colunas_para_remover = [coluna for coluna in discentes_2s_2023_ipiranga_filtrado.columns if coluna.startswith('Termo de consentimento')]
discentes_2s_2023_ipiranga_filtrado.drop(columns=colunas_para_remover, inplace=True)
```
## Retirando demais colunas que não serão utilizadas
Para retirar as demais colunas que não seriam utilizadas criei uma lista e utilizei o método ```drop()``` do pandas.

Segue trecho de código que utlizei para aplicar a remoção:

```
colunas_para_remover = ['Hora de início', 'Hora de conclusão', 'O seu projeto de PCI ocorreu em qual Fatec e com qual instituição estrangeira?', 'Em qual período você estuda?', 'Você participou de um PCI em qual idioma?', 'Fala', 'Escrita', 'Leitura', 'Compreensão Oral', 'Gramática', 'As instruções sobre os procedimentos estavam claras.', 'As etapas de organização do projeto estavam adequadas.', 'Os objetivos do projeto estavam claros.', 'O tempo para a realização das tarefas foi suficiente.', 'O grupo estrangeiro tinha as mesmas informações que o grupo brasileiro.', 'O(a) professor(a) dedicou tempo de aula para a resolução de dúvidas.', 'O(a) professor(a) ofereceu ajuda para resolver questões relativas aos PCIs.', 'Quais ferramentas você utilizou para a realização do PCI? (Marque todas as que você utilizou para se comunicar com as equipes e realizar as tarefas propostas)']
discentes_2s_2023_ipiranga_filtrado.drop(columns=colunas_para_remover, inplace=True)
```

# Filtros

## Filtrando alunos apenas da Fatec Ipiranga e Fatec Aveiro
Para o desenvolvimento do projeto precisava apenas dos alunos dessas duas Fatecs.

Segue trecho do código que utilizei para realizar a filtragem

Primeiro identifiquei as colunas presentes no dataset:

```
discentes_2s_2023_ipiranga.columns
```

Depois contabilizei a coluna "O seu projeto de PCI ocorreu em qual Fatec e com qual instituição estrangeira?":

```
discentes_2s_2023_ipiranga['O seu projeto de PCI ocorreu em qual Fatec e com qual instituição estrangeira?'].value_counts()
```

E por fim, criei o filtro com as Fatecs desejadas:

```
discentes_2s_2023_ipiranga['O seu projeto de PCI ocorreu em qual Fatec e com qual instituição estrangeira?'] = discentes_2s_2023_ipiranga['O seu projeto de PCI ocorreu em qual Fatec e com qual instituição estrangeira?'].str.strip()

filtro_resultado = discentes_2s_2023_ipiranga['O seu projeto de PCI ocorreu em qual Fatec e com qual instituição estrangeira?'] == 'Fatec Ipiranga com Profª. Patricia Patrício e Universidade de Aveiro'

discentes_2s_2023_ipiranga_filtrado = discentes_2s_2023_ipiranga[filtro_resultado]
```

Para visualizar o dataset filtrado:

```
discentes_2s_2023_ipiranga_filtrado
```

## Filtrando Portugueses
Como as duas Fatecs estavam reunidas na mesma resposta, precisei achar uma forma de diferenciar entre alunos da Fatec Ipiranga e da Fatec Aveiro. Foi possível identificar na coluna "Aponte os pontos positivos deste PCI.", algumas palavras escritas em português de Portugal.
Dessa forma, possibilitando a diferenciação entre os dois grupos.

Segue trecho de código em que realizei a filtragem dos portugueses no dataset

Primeiro visualizei todas as respostas do grupo filtrado com apenas estudantes da Fatec Ipiranga e Aveiro:

```
discentes_2s_2023_ipiranga_filtrado['Aponte os pontos positivos deste PCI.'].value_counts()
```

Depois, com as palavras que identifiquei como sendo de linguagem dos portugueses, consegui criar um filtro para separa-los:

```
filtro = discentes_2s_2023_ipiranga_filtrado['Aponte os pontos positivos deste PCI.'].str.contains('Ficar a saber|equipa|Partilhas')
```

Por fim, criei um novo dataframe com o filtro aplicado:

```
discentes_2s_2023_aveiro = discentes_2s_2023_ipiranga_filtrado[filtro]
```
Para visualizar o dataset

```
discentes_2s_2023_aveiro
```

## Filtrando Fatec Ipiranga
Após identificação dos alunos que eram portugueses, criei um terceiro dataframe para manter apenas os alunos brasileiros.

Segue trecho de código em que realizei a filtragem dos brasileiros no dataset

Filtro para pegar todos os aluno que deram respostas que não contenham as palavras em português de Portugal:

```
filtro_exclusao = ~discentes_2s_2023_ipiranga_filtrado['Aponte os pontos positivos deste PCI.'].str.contains('Ficar a saber|equipa|Partilhas')
```

Criei o dataset com o novo filtro:

```
discentes_2s_2023_ipiranga = discentes_2s_2023_ipiranga_filtrado[filtro_exclusao]
```

Para visualizar o dataset filtrado:

```
discentes_2s_2023_ipiranga
```

# Análise

Após ter os dois grupos separados, realizei as análises entre eles.

![Gráfico remomendaria participar de PCI](https://github.com/Joabe18/Sistema-de-Monitoramento-de-Creche/assets/87384920/999b1f50-b0cb-47b7-b7c3-1aa6af05ba9c)
