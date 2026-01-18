Quando se é Noob em R, costumamos importar bibliotecas usando o clássico `install.packages("tidyverse")` seguido de `library(tidyverse)` para importá-la no código.
Evoluindo um pouco, descobre-se que validar se a instalação já foi feita é uma boa prática, então passamos a utilizar `if(!require(tidyverse)) install.packages("tidyverse")`. Eu estava justamente nesse nível... **ATÉ HOJE!!!**
<center><img src="Images/PacmanPng.png" width=60% /><br />

Sim, Pacman é o Herói que elevará o nível de nosso R!</center>
# Pacman
Você pode (E DEVE!) conferir a documentação do Pacman para R clicando [aqui](https://www.rdocumentation.org/packages/pacman).
Pacman, abreviação de Package Manager, é uma poderosa biblioteca para gerenciar seus pacotes.

Utilizá-lo é muito simples e basta que você o instale corretamente em seu computador através do
```r
if(!require(pacman)) install.packages("pacman")
```
E eu prometo, será a última vez que utilizará esse mesmo comando! A partir de hoje poderá instalar e importar todos os seus pacotes utilizando
```r
pacman::p_load()
```
O Pacman possui diversos outros comando úteis, dependendo da sua necessidade, mas dificilmente algum fará tanta diferença em sua vida quanto o `p_load()`. O `p_load` automaticamente verifica se as bibliotecas estão instaladas e as instala caso não estejam. Por fim, também importa elas no seu código, eliminando qualquer necessidade de verificação, instalação e importação extra. Vejamos o exemplo abaixo:
```r
pacman::p_load(tidyverse, janitor, fs, vroom)
```
Principalmente caso seu código exija a importação de muitas bibliotecas, o Pacman será de grande ajuda!

## Dica de Platina
Existe um arquivo chamado **.Rprofile** em algum lugar no seu computador. Ele gerencia as configurações iniciais do R. Uma vez o Pacman instalado, adicione o comando abaixo e garanta que o Pacman sempre será inicializado com o R, acabando com a necessidade de executar a linha abaixo toda vez.
```r
if(interactive()) require(pacman)
```
Caso não encontre o arquivo **.Rprofile**, utilize o comando abaixo:
```r
if(!require(usethis)) install.packages("usethis")
usethis::edit_r_profile()
```
Você abrirá o arquivo do **.Rprofile** no próprio RStudio. Apenas adicione o comando que carrega o Pacman e salve.

Inclusive... Caso esteja trabalhando num projeto compartilhado com alguém, evite instalações automáticas das bibliotecas com o Pacman para garantir que você e seu camarada estejam sempre utilizando as mesmas versões da biblioteca, garantindo que o código sempre funcionará sem erros. Para projetos compartilhados, considere usar o [Renv](https://www.rdocumentation.org/packages/renv).
# Outros comandos úteis do Pacman
Além de caçar fantasmas e instalar e importar bibliotecas automaticamente, Pacman também nos ajuda em outras coisas úteis. Caso não coloque nomes de bibliotecas dentro dos parâmetros, então os comandos afetarão TODAS as bibliotecas do seu ambiente de desenvolvimento.

### Atualizar Bibliotecas
Através do comando abaixo, o Pacman atualiza automaticamente todas as bibliotecas instaladas.
```r
p_update()
```
## Descarregar Bibliotecas
O comando abaixo nos permite descarregar as bibliotecas de nosso código, liberando memória ou eliminando bugs por bibliotecas defeituosas ou em conflito.
```r
p_unload()
```
### Instalar Bibliotecas Temporariamente
Só precisa usar determinada biblioteca por certo tempo e não quer instalar ela em todo o seu ambiente? Este comando permite que a biblioteca seja instalada e automaticamente apagada quando a sessão do R foi encerrada.
```r
p_temp()
```
### Conferir os autores de uma biblioteca
Caso queira saber quem foram os criadores de determinada biblioteca, use este código para prestar suas continências.
```r
p_author()
```

## Dica de Diamante
Utilize os comandos do Pacman utilizando `pacman::` antes do método específico. "**::**" é um operador chamado "Operador de Namespace". Ele nos permite utilizar atributos e métodos de alguma biblioteca sem precisarmos importar ela com `library()`. Ao utilizar `pacman::p_update()`, por exemplo, estamos dizendo ao R para executar o comando `p_update()` da biblioteca pacman, mas sem importá-la toda no código.
Além do mais, o operador de namespace é especialmente útil quando existem duas bibliotecas que utilizam funções diferentes com o mesmo nome.