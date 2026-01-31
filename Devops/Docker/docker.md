# Docker
**Aviso:** *Para maior acessibilidade, este guia foi feito baseando-se no Fedora Linux*.

> Por que só Fedora? E o Windows? A maioria das pessoas usa Windows!

A maioria das pessoas usa Windows e a maioria das pessoas é medíocre. Você quer ou não quer ser uma lenda? Com um nome capaz de rivalizar com grandes Imperadores e divindades do Panteão do DevOps? Caso sim, instale Linux utilizando uma Máquina Virtual ou Dual Boot. As razões para preferir Linux ao trabalhar com Docker são inúmeras:
- Linux vem com Docker nativo. Windows precisa fazer gambiarra;
- Mais fácil de configurar e aprender Docker no Linux;
- Maior performance;
- Uso de memória RAM reduzido;
- Menor consumo de CPU;
- Maior facilidade para lidar com redes e servidores;
- Gerenciamento de arquivos é instantâneo e superior;
- Acesso direto ao hardware (Útil se o projeto envolve GPUs, portas serial, USB, etc);
- Estabilidade;
- Servidores gigantes como AWS, Google Cloud, Azure (Sim, até a Microsoft prefere Linux), entre outros rodam em Linux.

Eu mesma uso Windows para jogar joguinho, mas para trabalho e projetos sérios, fico longe do "*Janelas*". Se ainda não se sente convencido em trocar seu ambiente de produção para Linux, fique tranquilo que depois faremos um "Guia de Docker para usuários pesados de Windows". Mas vai demorar...  
Se você quer somente usar Docker, o Windows dá um jeito. Se você quer ENTENDER e DOMINAR Docker, Linux é inevitável.
<p align="center"> <img src="../../Images/TuxAsThanos.png" width="100%"> <br> <em>"Eu sou Inevitável!"</em> </p>

## O que é "Containerização"?
Imagine que você vai viajar para a Groenlândia — um país bem frio.  
O que você precisa levar para sua viagem? Vejamos...

- Casacos;
- Calças;
- Meias;
- Documentos;
- Camisas;
- Sapatos.

E está bom por hora. Você cria um Container (Sua mala) e coloca tudo que precisa para a viagem. Aí você vai para a Groenlândia e tem uma excelente estadia!  
Isso se difere de "Virtualização" pois virtualização seria levar sua casa toda (Incluindo encanamento, rede elétrica e estruturas) para a viagem, sendo que não vai usar a maioria das coisas. Containerização se baseia em levar apenas o que você precisa.

No contexto da informática, vamos supor que eu escreva um algoritmo em Python que utilize versões específicas de bibliotecas. Se eu enviar o código para alguém, pode ser que a pessoa não consiga rodar por possuir versões diferentes instaladas em seu computador.  Criando Containers esse problema não existe, pois terei colocado todas as bibliotecas e dependências necessárias dentro do Container, assim a pessoa rodará todo o projeto dentro do próprio Container.  
Tecnicamente falando, Containers levam dependências dentro de si, mas compartilham o Kernel com o seu sistema operacional para funcionarem. Assim, é como se você tivesse um """"mini sistema operacional paralelo"""" criado especificamente para determinada tarefa, todo configurado e otimizado para certa função, mas sem carregar todo o peso de um sistema operacional inteiro.

## O que são Imagens?
Se "Container" é a mala que você leve para a Groenlândia, pense que a "Imagem" é a lista de coisas que você vai levar para saber o que botar na mala.  
"Imagem" é como a planta de uma construção, que dita como o projeto vai ser construído. A Imagem contém todas as instruções para criação do Container. Você pode criar centenas de Containers a partir de uma única imagem, como numa produção em escala. Também pode deletar diversos Containers e a imagem estará intocável, servindo apenas como molde para criação dos Containers.  
Por isso imagens são imutáveis. Você não "usa" uma imagem, você cria Containers a partir dela.  
Portanto, caso faça certa alteração em um Container, seus dados serão perdidos eventualmente pois não ficam salvos na imagem. Precisará de um "Volume" caso queira salvar modificações em Containers.

## O que são Volumes?
Ao voltar de sua viagem à Groenlândia, você precisa desfazer a mala (Derrubar um Container). Nisso, você terá que arrumar a mala toda de novo quando for viajar novamente.  
Mas, você teve uma ideia... Como vai viajar para a Noruega (Outro lugar frio) em 2 semanas, irá guardar os objetos da mala em um lugar separado para facilitar a arrumação da mala posteriormente.  
Existe a possibilidade de você não desfazer a mala? Sim, e seria o equivalente a deixar o Container ativo, consumindo recursos sem estar usando ele.  
Portanto, Volumes são basicamente isso. Uma unidade de memória separada onde, ao criar um novo Container, ele irá recuperar os dados salvos no Volume e criar um novo Container que recupera o estado dos dados deixados no Volume.  
É como usar um Memory Card de PS1 ou PS2 para salvar um jogo e poder jogar de novo no mesmo ponto onde parou.

Falando em outros termos...
- **Imagem:** CD de um jogo de vídeogame;
- **Container**: Jogo rodando;
- **Volume:** Memory Card.

Agora que sabemos os conceitos, como podemos criar Containers?

# 神光あれと言たまひければ光ありき (Deus falou "Haja Luz", e então houve Luz)
Infelizmente, **ainda** não somos poderosos o bastante para criar Containers como um Demiurgo ordenando a criação, então, necessitamos criar "Dockerfiles". Ele se parece com isso:
```dockerfile
FROM fedora:latest
RUN dnf install -y python3
COPY . /app
CMD ["python3", "/app/genesis.py"]
```
Isso **NÃO** é um script, isso está mais próximo do "DNA" de um Container. Cada linha é uma **Camada** que define o estado final do Container, alterar uma linha redefine tudo que vem depois. Isto é, se eu quiser fazer alguma alteração, o que foi feito antes ficará salvo no cache e o Docker só irá executar a partir da linha que alterei, poupando tempo e processamento.  
Cada linha é uma Camada que vai sendo empilhada sobre a anterior. Camadas anteriores não são modificadas, por isso a ordem de construção do Dockerfile importa. Coloque o que menos terá alterações em cima.  
Lembre-se: Isso aqui é Docker, então... **Abandone a mentalidade Procedural!!!**

Vamos analisar essa Dockerfile simples...
```dockerfile
FROM fedora:latest
```
Essa primeira linha é a Gênese de nosso Container. Ela diz: 

>"O senhor programador modelou o Container com Fedora. Soprou-lhe acesso ao Kernel e deu-lhe isolamento e recursos. E o Container tornou-se um processo."

Basicamente, estamos dizendo que o Container será feito com base na última (Latest) atualização do Fedora. Poderia, por exemplo, ser:

```dockerfile
FROM ubuntu:latest
```

Ou:

```dockerfile
FROM archlinux:latest
```

Ou:

```dockerfile
FROM nginx:latest
```

Ou:

```dockerfile
FROM continuumio/anaconda3:latest
```

Ou:

```dockerfile
FROM openjdk:latest
```

Como **ainda** não somos deuses, não podemos criar uma imagem completamente do zero, então o `FROM` indica justamente que estamos pegando uma imagem já existente para ser a base da imagem que estamos criando para o projeto.  
Aliás, note que todos os exemplos usaram `latest`. Isso não é uma boa prática pois queremos estabilidade nos projetos com Docker. Prefira sempre definir versões específicas, como no exemplo abaixo:

```dockerfile
FROM mysql:8.0
```

Isso evita que algo pare de rodar mediante atualização, já que o `latest` sempre pegará a versão mais recente da imagem utilizada como base.

E a segunda linha de nosso Dockerfile?

```dockerfile
RUN dnf install -y python3
```

Aqui estamos executando comandos para criação do Container. `dnf` é o instalador de pacotes do Fedora Linux, logo, supondo que a imagem carregada tenha sido ```FROM archlinux:latest```, você utilizaria:

```dockerfile
RUN pacman -Sy --noconfirm python
```

Pois pacman é o instalador de pacotes do Arch Linux. E supondo que sua primeira linha tenha sido `FROM continuumio/anaconda3:latest`, então você usaria:

```dockerfile
RUN conda install -y python3
```

Mas o Anaconda já vem com o Python instalado, então não seria necessário executar isso. O importante aqui é entender que o `RUN` irá executar comandos para instalação de dependências e configuração de tudo que irá dentro do Container.  
Aliás, o `-y` é obrigatório. Se você estivesse utilizando esse comando dentro do Terminal, uma mensagem apareceria perguntando "Deseja instalar? Digite 'y' para Sim e 'n' para não". Mas o Docker não terá um Console para que você possa digitar essas informações, então precisará definir tudo já no comando.

E a terceira linha? O que ela faz?

```dockerfile
COPY . /app
```

Basicamente, está dizendo "Copie TUDO dentro da pasta atual e jogue na pasta '/app'". Esse tudo realmente significa **TUDO**, então evite guardar pornô ou outros arquivos paralelos na mesma pasta de seu projeto para não ficar pesado e não comprometer com informações sensíveis. Caso seja inevitável, crie um arquivo ".dockerignore" e configure-o para ignorar arquivos que você não quer que sejam lidos na inicialização do Container. Veremos mais sobre ele adiante.  
Aliás, a pasta '/app' não precisa existir. Caso não exista, o Docker criará automaticamente.