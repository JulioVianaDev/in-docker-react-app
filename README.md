# Este projeto tem o intuito de "descomplicar" o Docker

### Então por isto o projeto App.js está como um projeto React recém criado

<img src="./images-readme/react-dockerizado.png">

## Dockerfile

### Primeiro vamos entender o Dockerfile, a tarefa dele é criar uma imagem do nosso projeto, mas como funciona uma imagem? Ela precisa de algumas coisas para funcionar, a primeira delas é qual "linguagem"(pode ser um banco de dados ou SO porém lembrando, estamos descomplicando) ele usará no nosso caso colocamos:

-```FROM node```

### Então isto significa que queremos ter uma imagem(aplicação) que tem como uso dentro dela o nodejs ferramenta responsável por compilar o React

### Agora vamos criar uma área de trabalho para podermos trabalhar mais pra frente, ou seja, toda a nossa imagem agora se chama /app

-```WORKDIR /app```


### Como estamos cansados de saber toda ferramenta React precisa do node_modules com as libs instaladas(isso inclue axios,bootstrap, materialUI,entre outras libs que vc pode querer por),mas como vamos instalar isso na nossa imagem? copiando os dois package.json que estão no nosso projeto aonde fica as versões das libs, porém aonde vamos jogar os packages? Na nossa imagem ou seja o ponto final no fim da linha "." TRAAADUZINDO estamos copiando os packages e colando na nossa imagem

-```COPY package*.json .```

### Agora precisamos instalar as libs na imagem né, então utilizamos o comando RUN para rodar o npm install dentro da imagem

-```RUN npm install```

### Terminado de instalar, não está faltando algo? A verdade a nossa aplicação, então como fazemos para copiar o nosso projeto? copiamos tudo que está no nosso diretório atual(projeto) com um ponto e movemos para a localização da nossa Dockerfile(imagem) com outro ponto

-```COPY . . ```

### Agora que o projeto já está clonado e instalado na imagem para onde ele deve ir? vamos colocar ele na porta 3000 do NOSSO container:

-```EXPOSE 3000```

### E para finalizar vamos iniciar o projeto

-```CMD [ "npm", "start" ]```

### Com isto finalizamos a nossa Imagem com react e apenas ela está funcionando, porém tem um problema, imagens são apenas para leitura, ou seja, sempre que alterarmos o código ela precisa ser refeita, e isso é muito chato, imagina pra cada linha de código ter que fazer de novo a imagem, para resolver este problema vamos usar o docker-compose uma ferramenta para orquestrar e manipular imagens e containers(o container é como se fosse um computadorzinho só rodando a sua imagem) pode checar eles funcionando com o comando ```docker ps``` para ver os containers ativos e ```docker ps -a``` para todos os containers criado

<img src="./images-readme/Dockerfile-o-que-faz.png">

## Docker-compose

### Docker-compose é uma ferramenta pra poder manipular containers e ligar eles, mas também serve pra fazer a imagem "recarregar" sempre que for alterada para isto vamos as explicações das linhas do docker-compose.yaml


### Versão do docker-compose que estou usando

-```version: '3.9'```

### Serviços(containers rodando o nosso projeto) que estão rodando, preste muuuita atenção nos TAB(espaços do lado) das linhas a identação importa muito

-```services:```

### O nome do serviço que eu desejo criar é "frontend" genérico? Talvez, ia ser mais legal se eu chamasse de tomate? Com certeza mas a idéia é simplificar, 

-```frontend:```

### O build se refere aonde está o nosso Dockerfile eu coloquei . porque ele se encontra no mesmo diretório da pasta

-```build: .```

### Agora as portas, lembram que lá no Dockerfile nós colocamos o container para jogar para a porta 3000? então o que vamos fazer agora é pegar a porta 3000 do container e jogar a nossa localhost na porta 3000

-```ports:
      - "3000:3000"```


### Agora vamos ao pulo do gato, para poder rodar o nosso projeto SEMPRE que editarmos o código ele recompilar precisamos criar uma coisa chamada volume, tudo dentro do volume é "mudável" então vamos imaginar que ele consiga saber quando vc editou teu código, mas como ele faz isto?Ele utiliza do seguinte pressuposto pra montar: nome_do_volume/Caminho_do_container, Lembram que nomeamos no Dockerfile o WORKDIR /app então agora vamos usar ele como caminho do volume para que tudo que estiver no nosso projeto seja automaticamente enviado a imagem

-```volumes:
      - .:/app```

### Estas duas opções são para manter o container aberto a comunicação do container fazendo com que seja mais fácil de interagir

-```stdin_open: true
    tty: true```

# Como rodar o projeto:

## Precisa ter instalado o docker e o docker-compose no seu computador(docker-compose já vem com o docker):

- <a href="https://docs.docker.com/engine/install/" __blank> Docker </a>

## E para rodar este template e iniciar o seu docker-compose basta usar:

```docker-compose up```