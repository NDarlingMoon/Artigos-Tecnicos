_Atenção: Não confundir o **Modelo de Tobit** com a série de anime/mangá **[Chobits](https://pt.wikipedia.org/wiki/Chobits)**_.
# Modelo Tobit
O **Modelo de Tobit**, também chamado de **Regressão de Tobit** é um modelo estatístico utilizado quando a variável dependente é **censurada**. 
<p align="center"> <img src="../../../Images/CensoredDolphin.png"> <br> <em>Sim. O G0lf!%$# também foi c#n$%#@%$.</em> </p>Na prática, o que significa "ter uma variável censurada"? Significa que ela pode não representar o que realmente representa, que ela pode estar oculta e não demonstrando seu verdadeiro valor real.
Em outros termos... Imagine uma tabacaria que vende maconha (Em Las Vegas, pois lá é legalizado). Todo mês o dono anota quantas gramas de erva cada cliente comprou para uma promoção de descontos cumulativos. Em determinado mês, ele anotou os seguintes dados:
- Winter: Comprou 20g de maconha  
- Flipper: Comprou 5g de maconha  
- Hope: Comprou 0g de maconha  
- Ecco: Comprou 0g de maconha  

Há um enorme porém nessa brincadeira... Hope é uma usuária de cannabis recreativa, mas não comprou esse mês pois estava sem dinheiro. Já Ecco ativamente evita fumar cannabis, preferindo tabacos tradicionais. Enquanto o **0** de Hope representa que ela não comprou, mas pode comprar, o **0** de Ecco representa que ele não comprou e jamais vai comprar. Logo, dizemos que os dados observados de Ecco e Hope estavam censurados — os dados coletados deles são iguais, porém, são diferentes em essência.

Se formos representar isso numa escala, poderíamos dizer que Hope comprou 0g e Ecco comprou -500g. Não há como comprar maconha negativa, -500g serve apenas como representação intuitiva, mas o Modelo de Tobit utiliza essa escala (Representada por "$y^*$") para dizer que Ecco não comprou e está ABSURDAMENTE longe de comprar, enquanto Hope não comprou, mas ainda pode comprar. Na prática, os dados que estaremos observando (Representados por "$y$") vão de 0 a infinito, mas o Modelo de Tobit nos apresenta ao $y^*$, que vai de infinito negativo a infinito positivo, para separar as "diferentes interpretações de um 0".  
O modelo Tobit não trabalha diretamente com $y$, mas sim com $y^*$, assumindo que a censura ocorre de forma determinística em um ponto conhecido.

Inclusive, o Modelo de Tobit também pode trabalhar com censuras à direita. No exemplo dado acima, a censura ocorria à esquerda, no 0. Mas, pensando em outro exemplo, vamos ver abaixo...

Atualmente no Brasil (jan/2026 D.C.), uma pessoa pode andar com até 40g de maconha e ser considerada usuária. Acima deste valor, o indivíduo é considerado traficante e pode ser preso. Suponha que o IBGE está fazendo uma pesquisa para entender o consumo mensal dos usuários de maconha. Os dados coletados foram:
- Jeremias: 700kg
- João: 10g
- Maria Lúcia: 0g
- Pablo: 300kg

Como a pesquisa foi desenhada para observar apenas usuários, podemos desconsiderar os dados acima de 40g e considerar todos esses como "40g" que foram censurados e aplicar o Modelo de Tobit para lidar com eles. E mais... Como Maria Lúcia comprou 0g, não sabemos se ela de fato não comprou por não consumir ou por alguma outra razão, então o Modelo de Tobit pode ser aplicado tanto no 0g quanto no 40g, se tornando "intervalar".

### Nota Importantíssima
O Modelo de Tobit não distingue as razões pelas quais um valor foi censurado. Ele assume que toda censura decorre do mesmo processo. A distinção entre “não comprou porque não pôde” e “não comprou porque não quis”, ou "comprou acima de 40g porque é traficante" e "comprou acima de 40g porque consome muito" é feita na interpretação dos dados pela pessoa que irá analisá-los, não pelo modelo. Se quisermos definir explicitamente a diferença entre cada tipo de dado que foi censurado, podemos usar o **[Modelo de Seleção de Heckman](Heckman.md)**

Mas então, vamos entrar um pouco na matemática das coisas?
# Formulação

A fórmula básica do Modelo de Tobit é:

$$
y^* = X\beta + \varepsilon, \quad \varepsilon \sim \mathcal{N}(0, \sigma^2)
$$
onde:
- $y^*$ é a variável latente
- $X$ é o vetor de variáveis explicativas
- $\beta$ é o vetor de coeficientes
- $\varepsilon$ é o erro normalmente distribuído

Vamos ignorar a segunda parte da fórmula por enquanto e focar em $y^* = X\beta + \varepsilon, \quad \varepsilon$.

### Variável Latente


A variável observada $y$ é dada por:

$$
y =
\begin{cases}
y^*, & \text{se } y^* > c \\
c, & \text{se } y^* \leq c
\end{cases}
$$