Por que utilizar Docstrings? Daqui alguns meses, quando precisar revisitar a função que você escreveu, descobrirá a resposta.
Uma função escrita hoje em dia parecerá óbvio para você, que saberá de cabo a rabo o que ela retorna, como é utilizada e onde. Existe, porém, uma entidade maligna conhecida como "**Fada do Esquecimento**". 
Essa fada surge enquanto dormimos e utiliza uma técnica proibida conhecida como "Mugic" para limpar nosso cérebro, em específico a região do Hipocampo, apagando todas as nossas memórias. Então, somos visitados pela fada e, no dia seguinte, quando vamos mexer no código... Esquecemos o que ele faz.
Infelizmente, não há um jeito de destruir a fada, ela sempre conseguirá deturpar nossas memórias ao longo do tempo. O melhor jeito de lidar com isso é justamente utilizando Docstrings, para documentar no próprio código tudo que nossas funções e métodos recebem, retornam, os erros possíveis e dar exemplos de uso, para que outras pessoas saibam como utilizá-los no código, e no futuro você mesmo possa usar, caso se esqueça.

# O que são Docstrings?
Docstrings são, literalmente, Strings. Strings que você utiliza logo após a definição da função ou método para documentá-la. Veja o exemplo abaixo:
```python
def magic_shield(current_mana: int) -> str:
    """
    Um escudo de proteção contra a Fada do Esquecimento.
    Consome 60 de mana e garante que suas memórias serão preservadas por mais um dia.

    Args:
        current_mana (int): Recebe a mana atual. Só é possível utilizar o escudo 
                            de proteção caso a mana seja maior ou igual a 60.

    Raises:
        ValueError: Caso a mana seja inferior a 60.

    Returns:
        str: Retorna o feitiço de invocação do escudo.
    
    Examples:
        print(magic_shield(80))  # Retorna "Utamo Vita!!!"
        print(magic_shield(40))  # Levanta ValueError

    """
    if current_mana >= 60:
        return "Utamo Vita!!!"
    
    raise ValueError(f"Mana atual é de {current_mana}. Impossível usar a magia.")
```
Tão simples que chega a ser ridículo. O tipo de coisa que você escreve em dois minutos e pode poupar horas de análise no futuro. Caso no futuro você esqueça a implementação da função, pode utilizar o comando `help()` para verificar a Docstring. Veja abaixo:
```python
help(magic_shield)
```
E a saída será:
```text
Help on function magic_shield in module __main__:

magic_shield(current_mana: int) -> str
    Um escudo de proteção contra a Fada do Esquecimento.
    Consome 60 de mana e garante que suas memórias serão preservadas por mais um dia.

    Args:
        current_mana (int): Recebe a mana atual. Só é possível utilizar o escudo
                            de proteção caso a mana seja maior ou igual a 60.

    Raises:
        ValueError: Caso a mana seja inferior a 60.

    Returns:
        str: Retorna o feitiço de invocação do escudo.

    Examples:
        print(magic_shield(80))  # Retorna "Utamo Vita!!!"
        print(magic_shield(40))  # Levanta ValueError
```
E caso esteja usando PyCharm, Visual Studio Code ou alguma outra IDE poderosa, Docstrings se tornam **AINDA MAIS** poderosas, já que você pode repousar o mouse sobre a função e será exibida uma janela Pop-Up com sua Docstring.
<p align="center"> <img src="../../Images/Docstring.png"> <br> <em>É útil, mas lembre-se que lendas da programação não usam o mouse.</em> </p>

## Outros Tópicos dentro de Docstrings
A esmagadora maioria das Docstrings contará com Args, Raises, Examples e Returns. Apesar disso, existem outros tópicos que podem ser encaixados nela. Eles são:
- **Attributes**: Para classes
- **Yields**: Para funções geradoras
- **Warns**: Para avisos
- **Notes**: Para notas importantes
- **See Also**: Para adicionar referências cruzadas
- **References**: Para citações acadêmicas
- **Deprecated**: Para funções obsoletas
- **Todo**: Para indicar funcionalidades futuras
- **Version**: Para controle de versão

# Sphinx e Modelos de Docstrings