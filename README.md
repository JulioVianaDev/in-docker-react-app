# Este projeto tem o intuito de "descomplicar" o Docker

### Então por isto o projeto App.js está como um projeto React recém criado

## Dockerfile

### Primeiro vamos entender o Dockerfile, a tarefa dele é criar uma imagem do nosso projeto, mas como funciona uma imagem? Ela precisa de algumas coisas para funcionar, a primeira delas é qual "linguagem"(pode ser um banco de dados ou SO porém lembrando, estamos descomplicando) ele usará no nosso caso colocamos:

-```FROM node```

### Então isto significa que queremos ter uma imagem(aplicação) que tem como uso dentro dela o nodejs ferramenta responsável por compilar o React

### Agora vamos criar um projeto 

-```FROM node```
WORKDIR /app

COPY package*.json .

RUN npm install

COPY . . 

EXPOSE 3000

CMD [ "npm", "start" ]