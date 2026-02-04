# Testes e Mocks
Quando menor, já cheguei a me imaginar casando com pessoas do mesmo gênero... mas nunca realmente considerei a ideia. Até meados de meus 21 anos...  
Fui a um bar com minha "amiga" e, num momento em que o álcool tomava conta de nossas mentes, ela começou a flertar comigo. Fiquei me fazendo de idiota, querendo prestar atenção na TV (Que, aliás, estava passando "Konosuba"), mas, acabei cedendo e ela me meteu um beijão.  
Naquela noite, quase nos expulsaram com medo que fôssemos transformar o sofá do bar em motel. Passado esse dia, namoramos por quase 1 ano e isso consolidou minha fama de *Sáfica Cafajeste*.  
O que essa história nos ensina? Que precisamos testar as coisas! A imaginação é um bom campo experimental, mas ela não supre a realidade. Ao escrever um código, ele funciona? Você pode dizer que sim, mas, e os possíveis erros que vão vir? Você os testou ou você apenas **acha** que vai funcionar? Testes Unitários existem em todas as linguagens orientadas a objetos e possuem justamente a função de simular diferentes cenários possíveis em seu código, para garantir que tudo esteja saindo como planejado.
<p align="center"> <img src="../../../Images/MagiRevoKiss.png" width="100%"> <br> <em>Se está em dúvida sobre sua Orientação Sexual, meu maior conselho é "Caia de boca".</em> </p>

## Pytest
O Python vem com uma biblioteca para testes padrão, o `unittest`. Finja que ele não existe.

> Mas eu quero usar ele!

Não quer. Cale-se!  
`unittest` é antigo, engessado, verboso e tudo de mais podre que existe nesse mundo. Se quiser, use `unittest` para testar sua sanidade, mas aqui, utilizaremos `pytest`, que é extremamente mais simples, legível e poderoso.  
Comece instalando ele:

```bash
pip install pytest
```

E para rodar seus testes, basta digitar no terminal:

```bash
pytest
```

Não irá aparecer nada, afinal, nenhum teste foi criado ainda, oras. Mas veja como `pytest` é simples!

Pois bem, para criar testes, precisamos de métodos para serem testados. Vamos utilizar o método criado para fins didáticos a seguir como exemplo e criar testes em cima dele:

```python
import cmath
import math
```

> Números Complexos?  Não é um exemplo complexo demais?

Muitos guias e tutoriais por aí utilizam exemplos bestas de soma e fazem testes do tipo "2+2 é igual a 4?". Considero exemplos extremamente genéricos pois não demonstram o verdadeiro poder dos testes.  
Aqui, a escolha de um exemplo com cálculos de engenharia foi dada justamente por serem propensos a erros de arredondamentos e sinal, mostrando resultados inesperados que realmente podem ocorrer (E não testes idiotas do tipo "2+2 = 5?" para apitar que um teste falhou).  
Testes existem para capturar erros plausíveis. Exemplos simplistas não geram erros plausíveis.  
Se não se recorda ou nunca aprendeu números complexos, essa é uma oportunidade inefável para entendê-los. Liberte o Gênio que há em você!

Pois bem...  
Comece criando uma pasta chamada `tests` e um arquivo chamado `test_ac.py`. É **IMPRESCINDÍVEL** que o nome do arquivo comece com `test_`. Caso o nome seja outro, `pytest` irá ignorar completamente o arquivo na hora de testar os métodos.