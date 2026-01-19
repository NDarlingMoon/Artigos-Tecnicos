Quais nomes na história não precisam ser nomeados para serem lembrados? 

Pense nos indivíduos **MAIS FAMOSOS E INFLUENTES NA HISTÓRIA DA CIVILIZAÇÃO HUMANA**. Talvez você recorde de nomes como Jesus Cristo de Nazaré, Caio Júlio César, Michael Jackson, Albert Einstein, Sir Isaac Newton, Wolfgang Amadeus Mozart, Eu (Em algum momento no futuro)... pois bem, arquétipos que, para o bem ou para o mal, marcaram seu nome na história. Suas lendas estão marcadas no consciente coletivo da humanidade, tornando-se símbolos que não precisam ser nomeados.

Em Python, temos algo parecido: Closures. Funções que se recordam de variáveis externas, mesmo que essas variáveis não sejam passadas como parâmetros nas funções quando chamadas.
> — De onde vem esse valor?
> — Você sabe.
> — Sei? — pergunta a Função, confusa.
> — Sabe...

Mas, então, como seria a construção de uma Closure no código?
Algo parecido com isso:
```python
def salutation(praise: str):
    def genius(name: str):
        return f"{praise} {name}!"
    return genius

praiser = salutation("Olá")

print(praiser("Mozart"))
print(praiser("Newton"))
```

## Explicação do Código
Caso tenha copiado o código e executado, notará que a saída foi
```text
Olá Mozart!
Olá Newton!
```
Mas o único parâmetro passado dentro do `print()` foram as strings "Mozart" e "Newton". A função se recordou do valor definido para `salutation()` definido anteriormente, mesmo que o escopo já tenha finalizado sua execução.

E adentrando as bases da Closure... Temos aqui uma função dentro de uma função, um conceito conhecido como "Função Aninhada".
```python
def salutation(praise: str):
    def genius(name: str):
        return f"{praise} {name}!"
    return genius
```
 Note que a função `salutation()`, que está no lado externo, retorna a própria função `genius()`, no lado interno. Ela também recebe um parâmetro `praise: str`. Uma string que, para nosso exemplo, carregará uma saudação como "Olá".
 
 A função `genius()`, por sua vez, executa a lógica e retorna uma string formatada com `praise: str`, que é passada como parâmetro na função externa, e `name: str`, que é passada como parâmetro na função `genius()`.
 
 E agora... magia!
 ```python
praiser = salutation("Olá")
 ```
 Invocamos uma variável que será usada de forma intermediária. Ela carregará consigo o retorno da função `salutation()`, `genius()`, mas definimos aqui o parâmetro que será recebido por `praise: str`.

E façamos um teste aqui... Copie as linhas de código abaixo e cole em algum lugar para executar:
```python
print(praiser.__closure__)
print(praiser.__closure__[0].cell_contents)
```
Mesmo após termos terminado de executar a função `salutation()`, o parâmetro que passamos continua lá! Ele foi para um lugar mágico, de sonhos e esperanças — algo próximo de Valhalla —, onde os parâmetros permanecem lembrados mesmo após o fim da execução da função.
Por isso, quando executamos as linhas abaixo
```python
print(praiser("Mozart"))
print(praiser("Newton"))
```
Invocamos as funções com a variável intermediária `praiser()`, que está armazenando a função `genius()` — já que `genius()` é o retorno de `salutation()`, e invocamos ela ao definirmos `praiser()` — e definimos o parâmetro como o nome de quem será saudado.

O parâmetro de `salutation()` foi lembrado!

E o melhor de tudo, você pode criar variações da mesma função! Confira no código abaixo:
```python
def salutation(praise: str):
    def genius(name: str):
        return f"{praise} {name}!"
    return genius

praiser = salutation("Olá")
imperial_praiser = salutation("Hail")
roman_praiser = salutation("Ave")

print(praiser("Mozart"))
print(praiser("Newton"))
print(imperial_praiser("Sithis"))
print(imperial_praiser("the Empire"))
print(roman_praiser("César"))
```
A saída será:
```text
Olá Mozart!
Olá Newton!
Hail Sithis!
Hail the Empire!
Ave César!
```
E caso não queira criar uma nova variável para tal, é possível usar na mesma linha:
```python
print(salutation("Saudações")("Terráqueos"))
```
E a saída ficará:
```text
Saudações Terráqueos!
```
Nosso exemplo é apenas uma saudação besta, para fins didáticos, então você ainda não esteja vendo uma utilidade prática para Closures. Vamos ver agora um exemplo realmente bom, que ilustre a utilidade titânica dessa funcionalidade do Python.

## Exemplo TITÂNICO
Conhece a biblioteca **Pandas**? Caso não, tenho um artigo sobre ele que você pode acessar clicando [Pandas](../Bibliotecas/Pandas.md).

Mas, apenas introduzindo, **Pandas** é uma biblioteca utilizada para manipular Datasets, ler conjuntos de dados, organizar planilhas, etc. Seu nome é derivado de "Panel Data", que é um termo amplamente utilizado em estatística, econometria e áreas correlatas.
<p align="center"> <img src="../../../Images/PoEating.png" width="100%"> <br> <em>A biblioteca Pandas não tem nada a ver com Kung-Fu... Mas seria legal se tivesse!</em> </p>

Para nosso exemplo a seguir, irei assumir que sabe minimamente utilizar o Pandas. Caso não saiba, lembre-se de conferir o [outro artigo](../Bibliotecas/Pandas.md) antes.

Veja o código abaixo:
```python
import pandas as pd

def remove_lines(*args, mode='individual'):
    def transform_df(df):
        if mode == 'interval' and len(args) == 1:
            return df.drop(list(range(0, args[0])))
        elif mode == 'interval' and len(args) == 2:
            return df.drop(list(range(args[0],args[1])))
        else:
            return df.drop(list(args))
    return transform_df
    
def remove_columns(*args, mode='individual'):
    def transform_df(df):
        if mode == 'interval' and len(args) == 1:
            return df.drop(df.columns[0:args[0]], axis=1)
        elif mode == 'interval' and len(args) == 2:
            return df.drop(df.columns[args[0]:args[1]], axis=1)
        else:
            return df.drop(df.columns[list(args)], axis=1)
    return transform_df

def rename_columns(*args):
    def transform_df(df):
        return df.set_axis(list(args), axis=1)
    return transform_df
```

Nesse código estamos definindo 3 funções diferentes — 3 closures — para manipular estruturas em Pandas. A primeira função remove linhas, a segunda remove colunas e a terceira renomeia as colunas. Vamos analisar cada função individualmente, isso também será um belo salto para entendermos melhor o Pandas.
```python
def rename_columns(*args):
    def transform_df(df):
        return df.set_axis(list(args), axis=1)
    return transform_df
```
Analisando o código acima, podemos ver que a função externa recebe `*args`. Ele é como um "parâmetro coringa". Ao inserir `*args` onde se inserem os parâmetros na hora de criar uma função, você está dizendo que a função pode receber  uma quantidade indeterminada de parâmetros — podendo ser 1 parâmetro, 2 parâmetros, 10 parâmetros ou +8000 parâmetros.
Já a função interna recebe um dataframe do Pandas. A lógica da função interna consiste em renomear as colunas do dataframe com os nomes que forem passados como parâmetro na função externa.

Já a função para remover linhas...
```python
def remove_lines(*args, mode='individual'):
    def transform_df(df):
        if mode == 'interval' and len(args) == 1:
            return df.drop(list(range(0, args[0])))
        elif mode == 'interval' and len(args) == 2:
            return df.drop(list(range(args[0],args[1])))
        else:
            return df.drop(list(args))
    return transform_df
```
Ela também recebe `*args`, com o adendo de receber `mode`. `mode` já vem com um valor padrão, `individual`, isso indica que, caso nenhum valor seja passado, o valor será `individual`.
Logo, seguimos para a função interna que também recebe um dataframe e prossegue para a lógica de remoção de linhas.

Já a última função, para remover colunas, é praticamente idêntica à de remover linhas, com o único adendo de indicar que se trata de colunas, não de linhas.
```python
def remove_columns(*args, mode='individual'):
    def transform_df(df):
        if mode == 'interval' and len(args) == 1:
            return df.drop(df.columns[0:args[0]], axis=1)
        elif mode == 'interval' and len(args) == 2:
            return df.drop(df.columns[args[0]:args[1]], axis=1)
        else:
            return df.drop(df.columns[list(args)], axis=1)
    return transform_df
```

### E por que essas 3 closures são úteis?
Se fossem apenas funções, o trabalho para implementá-las aumentaria bastante. Como são closures, podemos facilmente utilizá-las através do `pipe`.
```python
transformed_df = (df.pipe(remove_lines(0, 2, mode='interval'))
				.pipe(remove_columns(2))
				.pipe(rename_columns('AA', 'SQ')) )
```
Não há adjetivo melhor para definir isso do que **ELEGANTE**!

A utilização de closures nos permitiu utilizar funções em cadeia com `pipe`, funcionando exatamente como uma linha de montagem, onde cada resultado de função é passado como parâmetro para a próxima função, em sequência. Além de nos poupar diversas linhas de código, permite dinamismo, melhor legibilidade e manutenção de código.