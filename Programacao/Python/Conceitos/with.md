# With e Rollbacks
<p align="center"> <img src="../../../Images/Duwang.png" width="100%"> <br> <em>Pessoas que usam <b>with</b> em seus códigos tendem a ter uma vida mais tranquila.</em> </p>
Já pensou se você fosse capaz de voltar no tempo para corrigir seus erros?  

Imagine-se indo para o trabalho, mas, no meio do caminho, você é espancado violentamente por adolescentes e ninguém presta socorro imediato. Quando a ambulância finalmente chega para te socorrer, você já estava morto.  
Agora, imagine que, ao ser espancado violentamente por adolescentes, chamam uma ambulância para você imediatamente, ligam para a polícia e fazem os primeiros socorros. E melhor ainda... O "Mago do Tempo" te faz voltar ao passado e você acorda na sua cama, como se tudo até aquele momento tivesse sido um sonho. Você voltou no tempo para impedir uma tragédia.
<p align="center"> <img src="../../../Images/TimeWizard.png" width="30%"> <br> <em>Em outras palavras, o "with" nem sempre te salva da morte, mas sempre garante que seu corpo seja enterrado direito. </em> </p>
O comando "With" do Python funciona mais ou menos dessa forma. Ao invocá-lo, você segue seu dia normalmente, mas caso aconteça alguma tragédia (Ser explodido, assassinado, sequestrado, desfigurado, entre outras possibilidades), ele automaticamente chamará a ambulância, a polícia, os bombeiros e quem mais for necessário. E com sorte, você ainda consegue voltar no tempo para tentar de novo.  

"With" não impede que erros ocorram em seu código, ele apenas garante que, caso ocorra um erro, um plano de emergência será executado.

## Como o "with" funciona internamente?

Veja esse código criado para fins didáticos:
```python
with IrAoTrabalho() as sair:
	print("4. Grande dia!")
```

`IrAoTrabalho()` implementa dois métodos internos, `__enter__` e `__exit__`.  
Você **SEMPRE** usará `with` com objetos que respeitem esse protocolo, seja por terem `__enter__` e `__exit__` ou por estar implementando o módulo `contextlib`.  
Veja o código de `IrAoTrabalho()` para entender melhor:
```python
class IrAoTrabalho:
    def __enter__(self):
        print("1. Troco de roupa...")
        print("2. Abro a porta...")
        print("3. Vou andando até o trabalho...")
        return self
    
    def __exit__(self, exc_type, exc_value, exc_tb):
        if exc_type is None:
            print("5. Cheguei ao trabalho! Grande Dia!")
            print("6. Terminei meu expediente. Hora de ir embora!")
            print("7. Me despeço de todos...")
            
        else:
            print(f"5. Oh, não!!! Sofri um {exc_type}!")
            print("6. A ambulância foi chamada imediatamente.")
            print("7. Preciso voltar no tempo para impedir o acidente...")
        
        print("8. Volto para casa, em segurança.")
        return True
```

Por trás do uso do `with`, há toda uma implementação no objeto `IrAoTrabalho`.

**`def __enter__(self)`** é o "Setup". Ele define como as coisas serão e inicia o que for preciso.
Após a execução de `__enter__()`, que é automática e oculta com o `with`, o código explicitamente definido no `with` será executado.
```python
print("Grande dia!")
```
Quando todo o código dentro do `with` for executado, então o "CleanUp", **`def __exit__(self, exc_type, exc_value, exc_tb)`**, é automaticamente executado, também de forma oculta.  
`__exit__()` pode receber 4 parâmetros:
- `self` - O próprio objeto;
- `exc_type` - O tipo de erro;
- `exc_value` - A mensagem de erro;
- `exc_tb` - O local onde ocorreu o erro (Traceback).  
Caso tenha ocorrido algum erro, então o erro estará registrado em `exc_type`. Caso tudo esteja certo, `exc_type` será `None`. Dois caminhos seguirão, um caso não haja erros e outro caso ocorra tudo errado.  
Por fim, `__exit__()` poderá executará algo (Independentemente se der erro ou não) e retornará `True` caso o erro tenha sido suprimido, e `False` caso o código vá seguir com o erro.

Pois... Vamos ver como um código real seria.

## With e Pandas

Veja a classe abaixo, criada para tratar de arquivos Excel no Pandas. Analise bem o código, principalmente `__enter__()` e `__exit__()`.

```python
from pathlib import Path
import pandas as pd
import tempfile
import shutil
import os

class SafeExcelContext:
    def __init__(self, path, **kwargs):
        self.path = Path(path)
        self.kwargs = kwargs
        self.df = None
        self.backup_path = None
    
    def __enter__(self):
        self.validate_file()
        self.create_backup()

        ext = self.path.suffix.lower()

        engines = {
            '.xlsx': 'openpyxl',
            '.xlsm': 'openpyxl',
            '.xls':  'xlrd',
            '.xlsb': 'pyxlsb'
        }

        try:
            if ext in engines and 'engine' not in self.kwargs:
                self.kwargs['engine'] = engines[ext]
            
            self.df = pd.read_excel(self.path, **self.kwargs)
            
        except ImportError as e:
            raise ImportError(f"Pacote necessário não instalado: {e}")
            
        except Exception as e:
            raise IOError(f"Erro na leitura do Excel: {e}")
        
        return self.df
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        if exc_type is None:
            try:
                self.save_changes()
            except Exception as e:
                self.rollback()
        else:
            self.rollback()
        
        self.cleanup()
        return False

    def save_changes(self):
        if self.df is not None:
            engine = self.kwargs.get('engine') 
            self.df.to_excel(self.path, index=False, engine=engine)
    
    def create_backup(self):
        if self.path.exists():
            temp_dir = tempfile.gettempdir()
            backup_name = f"{self.path.stem}_backup_{os.getpid()}{self.path.suffix}"
            self.backup_path = Path(temp_dir) / backup_name
            shutil.copy2(self.path, self.backup_path)
    
    def rollback(self):
        if self.backup_path and self.backup_path.exists():
            shutil.copy2(self.backup_path, self.path)
    
    def cleanup(self):
        if self.backup_path and self.backup_path.exists():
            try:
                os.unlink(self.backup_path)
            except OSError:
                pass
    
    def validate_file(self):
        if not self.path.exists():
            raise FileNotFoundError(f"Arquivo não encontrado: {self.path.absolute()}")
        if not self.path.is_file():
            raise ValueError(f"Caminho não é um arquivo: {self.path}")
        if not os.access(self.path, os.R_OK):
            raise PermissionError(f"Sem permissão de leitura no arquivo: {self.path}")
        if not os.access(self.path, os.W_OK):
            raise PermissionError(f"Sem permissão de escrita no arquivo: {self.path}")
            
        return True
```

O bloco `with` lerá o arquivo Excel passado e salvará na variável `df`:
```python
with SafeExcelContext("pokemons.xlsx") as df:
    df.rename(columns={"Pokemon": "Pokémon"}, inplace=True)
```

O que `__enter__()` fez? Ele inicializa a Engine para leitura do arquivo Excel e o lê, levantando possíveis erros que possam ocorrer na leitura.
O que `__exit__()` faz? Aqui a coisa fica legal... Qualquer alteração que tenha sido feita dentro do bloco, no caso, renomear a coluna "Pokemon" para "Pokémon", adicionando um acento, é salva, sobrescrevendo o arquivo. Caso dê algum erro? Desfaz qualquer alteração que tenha sido feita.  
Independentemente se deu erro ou não, `__exit__()` faz uma limpeza geral para não deixar rastros pesando.

> Sempre precisarei criar uma classe quando quiser usar `with`?

Não é necessário, afinal, muitas classes já vem com implementações nativas disso, como por exemplo bibliotecas de leitura de banco de dados.

```python
import sqlite3

with sqlite3.connect("pokemons.db") as connection:
    print("Temos que pegar!!")
```

Não é necessário criar uma classe com `__exit__()` e `__enter__()` pois `sqlite3` já possui uma implementação nativa.  
Em outros casos, não é preciso criar classe quando se trata de uma implementação simples. Existe o Decorator `@contextmanager`. Basta criar um método simples e adicioná-lo que o Decorator criará `__exit__()` e `__enter__()` automaticamente. Pessoalmente, prefiro criar classes completas para ter melhor controle sobre, mas `@contextmanager` é uma opção.

```python
from contextlib import contextmanager

@contextmanager
def safe_excel(path):
    df = pd.read_excel(path)
    
    try:
        yield df
        
    finally:
        df.to_excel(path)

```

`@contextmanager` não nos dá muito poder de controle, então, o ideal é usá-lo em casos bem simples.

### Dica de Platina
O `with` é perfeito para ser utilizado em conexões a banco de dados ou leitura de arquivos, principalmente para não se preocupar em fechar as conexões, que são fechadas automaticamente.  
Apesar disso, versões antigas do `sqlite3` ou outros bancos de dados podem não fechar as conexões automaticamente. Esteja sempre utilizando versões estáveis recentes, e caso precise utilizar uma versão antiga, lembre-se de fechar a conexão.