# Média, Moda e Mediana
Existem 3 **Medidas de Tendência Central**, como são chamadas, em estatística: Média, Moda e Mediana.  
A Média Brasileira está entre 13,1cm e 15,7cm e você sabe do que estou falando...  
Mas o que isso significa na prática?

## Moda
Moda é o conceito mais simples de se entender. Quando falamos "Moda" na estatística, queremos dizer sobre o valor que mais aparece em determinada pesquisa. Veja a tabela abaixo, com dados _Categóricos Nominais_:

| Entrevistado   | Banda Favorita |
| -------------- | -------------- |
| Eddie          | Iron Maiden    |
| Modified Bear  | Radiohead      |
| Vic Rattlehead | Megadeth       |
| Murralsee      | DIO            |
| Snaggletooth   | Motörhead      |
| Papa Emeritus  | Ghost          |

Qual a Moda dessa tabela?
**NENHUMA!!!**  
Este exemplo não tem Moda pois não há um valor recorrente. Todos são diferentes entre si. É um caso "Amodal".

| Nome do Entrevistado | Banda Favorita |
| -------------------- | -------------- |
| Eddie                | Iron Maiden    |
| Modified Bear        | Radiohead      |
| Vic Rattlehead       | Megadeth       |
| Murralsee            | DIO            |
| Snaggletooth         | Motörhead      |
| Papa Emeritus        | Ghost          |
| Cardinal Copia       | Ghost          |
| Frater Imperator     | Ghost          |

Já no exemplo acima, note que a banda "Ghost" aparece 3 vezes, sendo o dado mais recorrente encontrado. Neste caso, Ghost é a Moda.
Sem segredo.
Vamos ver outro exemplo abaixo, mas com dados _Descritivos Discretos_:

| Nome do Entrevistado | Idade em que Perdeu a Virgindade |
| -------------------- | -------------------------------- |
| Entrevistado 1       | 22 anos                          |
| Entrevistado 2       | 19 anos                          |
| Entrevistado 3       | 26 anos                          |
| Entrevistado 4       | 21 anos                          |
| Entrevistado 5       | 22 anos                          |
| Entrevistado 6       | 18 anos                          |
| Entrevistado 7       | 26 anos                          |
| Entrevistado 8       | 20 anos                          |
| Entrevistado 9       | 22 anos                          |
| Entrevistado 10      | 21 anos                          |
Dentre os entrevistados, 
- **18 anos** aparece **1 vez** na pesquisa;
- **19 anos** aparece **1 vez** na pesquisa;
- **20 anos** aparece **1 vez** na pesquisa;
- **21 anos** aparece **2 vezes** na pesquisa;
- **22 anos** aparece **3 vezes** na pesquisa;
- **26 anos** aparece **1 vez** na pesquisa.

Qual o valor mais recorrente nos dados? 22 anos, então dizemos que 22 anos é a Moda, sendo 21 anos o segundo valor mais frequente (Mas não é Moda. Moda é somente o primeiro valor mais frequente).  
Utilizamos a Moda para indicar qual o dado mais popular, a maior tendência dentro de determinado grupo pesquisado. Extremamente útil quando a Média não pode ser calculada.  
Em resumo, Moda responde "O que é mais comum?"

Dito isso, não se sinta pressionado para perder a virgindade. **NÃO CEDA A PRESSÕES SOCIAIS!!!**

E vamos ver um último exemplo...

| Nome do Entrevistado | Dívida com o Agiota |
| -------------------- | ------------------- |
| Seu Madruga          | R$ 500              |
| Peter Parker         | R$ 500              |
| Pekola               | R$ 2 000            |
| Kaiji                | R$ 5 000            |
| Mona                 | R$ 5 000            |

Notou como R\$500 e R\$5 000 se repetem duas vezes cada? Isso é um caso "Multimodal", onde mais de um valor é considerado como Moda.

### Quando utilizar a Moda é mais útil que a Média?
- Como visto no primeiro exemplo, os dados são de tipo _Categórico Nominal_. Não são números. Como eu poderia calcular a Média disso?  
- A Moda também é extremamente útil (Juntamente à Mediana) para verificar se a "Média está mentindo".  
- Caso o objetivo da pesquisa seja encontrar o que é mais popular, então a Moda será o objeto central do estudo.  
- Em Pesquisas Qualitativas, a Moda geralmente é a única Medida de Tendência Central possível.

## Média
A fórmula da Média é essa:
$$\bar{x} = \frac{\sum_{i=1}^{n} x_i}{n}$$
Boo!  
Assustou? Vamos ignorar a fórmula por agora e buscar entender o conceito.

Imagine que eu, você e mais amigos vamos num bar... Nossa conta fechou assim:

| Pessoa          | Shots de Whisky Pedidos |
| --------------- | ----------------------- |
| Eu              | 15 Shots                |
| Você            | 0 Shots                 |
| Amigo 1         | 2 Shots                 |
| Amigo 2         | 1 Shot                  |
| Amigo 3         | 2 Shots                 |
| **Soma Total:** | 20 Shots                |
Se você não sabe o que é um "Shot", saiba que "Shot" é um pequeno copo (Geralmente 50ml) de bebida alcoólica.  
No exemplo acima, a Moda é 2, mas ela pouco importa para nós agora. O total de shots consumidos nesse dia foi de 20, o que dá, em Média, 4 shots para cada. Pode parecer que consumimos de forma controlada, mas a verdade é que eu provavelmente acabarei em coma alcoólico.

A Média nada mais é do que o valor total (No exemplo, 20) dividida pela quantidade de elementos (No exemplo, 5 pessoas).  
Enquanto a Moda nos diria que pedir 2 shots foi o mais comum para nosso grupo, a Média nos mostra que, se dividíssemos o total igualmente, cada um teria consumido 4 shots.
- Total de shots = 20
- Número de pessoas = 5
- Média = 20 ÷ 5 = 4 shots