Conheço um homem que certa vez disse:
>Eu não cometo erros. Eu não sou como o todos vocês. E-eu sou mais forte. Mais esperto e... eu sou melhor! **Eu sou melhor!** Vocês aí deveriam agradecer a Cristo por eu ser quem eu sou, porque precisam de mim. Precisam que eu salve vocês. Vocês sabem. Que eu sou o único que pode salvar. Vocês não são os verdadeiros heróis. **Eu sou o verdadeiro herói.**

Talvez ele tivesse razão, talvez não, mas eu sempre evitei dar chance ao azar. Por isso, sempre escrevo meus códigos para levantar Erros, trabalhar com Exceções e dar Avisos.
<p align="center"> <img src="../../../Images/HomelanderRaises.png" width="100%"> <br> <em>Capitão Pátria não seria um bom programador pois ele se recusaria a tratar exceções em seus códigos e quando ocorresse algum bug, culparia o usuário.</em> </p>

Em Python, nem todo problema deve ser tratado da mesma forma. Às vezes a melhor opção é abortar o código, em outras, podemos tentar tratar, em outras situações, apenas avisar que algo não saiu como planejado. Como fazemos isso? Com Raises, Exceptions e Warnings! Cada um desses 3 comunica um tipo diferente de falha, e escolher um deles errado pode ser pior do que não tratar nada.

Como escolher cada um? Vejamos:
- Imagine um casamento infeliz, onde você leva um chifre... Raise é o divórcio imediato após descobrir a traição. Não há negociação;
- Imagine um casamento infeliz, onde a pessoa nunca te escuta... Exceptions são a terapia de casal, para tentar resolver a situação e melhorar a convivência;
- Imagine um casamento onde a pessoa deixa a toalha molhada na cama... Warnings são o aviso que se dá para a pessoa não fazer mais isso, e tudo continua bem logo em seguida.

Vamos aprofundar melhor...
# Raises
Se tem uma boa frase para definir o que é um Raise, seria essa:
> Pare! Até quando você quer mandar e mudar minha vida?

Todo problema deve ser tratado de forma diferente, e o Raise é utilizado justamente nos casos onde não é possível fazer nada. Só podemos... Parar. Pense que Raises não são erros inesperados, mas uma decisão consciente de parar a execução pois continuar corromperia o estado do programa. Não há defeito algum nisso.

Aqui temos um exemplo de Raise bem simples e funcional...
```python
def divide(a, b):
	if b == 0:
		raise ZeroDivisionError("É impossível dividir por 0.")

	return a / b
```
Note que o Raise utilizado foi "ZeroDivisionError", mas poderia ser outro tipo de erro. Veja o exemplo:
```python
def divide(a, b):
	if b == 0:
		raise ZeroDivisionError("É impossível dividir por 0.")
	elif b < 0:
		raise ValueError("Este programa não divide por números negativos.")

	return a / b
```
Existem diversos tipos de Raises existentes, cada um para um tipo de erro específico que pode ocorrer. Levantar exceções específicas é essencial para que quem execute seu código possa tratá-las corretamente.

E o melhor, você também pode criar suas próprias Raises!!!
```python
class DivideByFourError(ValueError):
    """Erro levantado quando o divisor é igual a 4."""
    pass
    
def divide(a, b):
	if b == 0:
		raise ZeroDivisionError("É impossível dividir por 0.")
	elif b < 0:
		raise ValueError("Este programa não divide por números negativos.")
	elif b == 4:
		raise DivideByFourError("Este programa não divide por 4.")
	
	return a / b

```
Raises personalizadas são uma boa forma de garantir proteção contra erros específicos. Basta criar uma classe que recebe alguma Raise nativa do python nos parâmetros (Com isso, ela herda todas as propriedades da Raise que foi passada, poupando-te tempo de ter que definir tudo).

### Dica de Platina
Por padrão, Raises sempre terminarão com "Error" no final de seus nomes, como em ValueError, ZeroDivisionError, IndexError, KeyError, etc. Ao criar suas Raises personalizadas, mantenha o padrão ao colocar "Error" no final do nome.

### Dica de Diamante
Ao criar Raises personalizadas, faça com que ela herde a Raise nativa mais "semanticamente próxima". Em nosso exemplo, o erro ocorre ao dividir por 4, ou seja, receber certo valor. Então herdar de "ValueError" é o mais correto. Caso a exceção personalizada tivesse a ver com um Loop, talvez "IndexError" faça mais sentido para ser herdada.

### Dica de Ouro
Você pode conferir todas as Raises existentes nesse link: https://docs.python.org/pt-br/3/library/exceptions.html  
Note que todas as Raises também podem ser utilizadas como exceptions.

# Exceptions
