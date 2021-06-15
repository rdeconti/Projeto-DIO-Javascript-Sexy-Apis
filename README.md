:spiral_calendar: Atualizado em 15 de Junho de 2021 :heart:

<img align="right" alt="GIF" height="160px" src="https://github.com/rdeconti/rdeconti-resources/blob/main/Digital%20Innovation%20One%20-%20Logotipo.png" />

# Projeto Digital Innovation One Javascript
# Projeto Web: APIs para Gestão de Produtos utilizando Node.js
Este projeto foi proposto pela Digital Innovation One 
- Link do código original: https://github.com/IgorHalfeld/digital-innovation-one-demo
- Professor: Igor Halfeld
- Aula: https://web.digitalinnovation.one/lab/sexy-apis-usando-arquitetura-serverless/learning/52cf3e06-958c-479b-bae8-c5dcbebdbbcc

# Descrição
Nesse desafio você deve desenvolver e entregar um projeto de “APIs para Gestão de Produtos utilizando Node.js” ao qual você praticará e aplicará os conceitos de desenvolvimento de APIs e Arquitetura Serverless com Node.js. Demonstre toda sua capacidade criativa para transformar a base do projeto apresentada nesta sessão em um desenvolvimento inovador.

# Construindo sexy APIs usando arquitetura serverless

#### Serverless?

- Vou rodar uma função
- Vou pagar o quanto eu uso ou não
- Vou aumentar o número de fn com um click
- Vou ter facilidade no bootstraping
- Vou ter mais facilidade de por outros triggers

**Azure Functions**

AWS Lambda, Google Cloud Functions, Open Whisk

**Por que usar?**

Criar APIs

- listar
- criar
- update
- delete

Precisa de uma conta de Microsoft Azure;

https://azure.microsoft.com/

Extensão do VSCode, Azure Functions

Azure Functions Azure Tools

#### Pré-requisitos

Para executar os comandos você vai precisar do CLI do Azure instalado localmente.

https://docs.microsoft.com/pt-br/azure/azure-functions/functions-run-local?tabs=linux%2Ccsharp%2Cbash

**Iniciando o Projeto**

`func init`

-- node

-- javascript

**Criar uma função**

`func new`

8 Http trigger

Function Name: GetProducts

**No VSCode**

dar sim na notificação

No index.js

*Parte 3*

Pelo terminal

`func host start`



**Teste pelo Postman**

Listagem de produtos

localhost:7071/api/products

```js
const createMongoClient = require('../shared/mongoClient');

module.exports = async context => {
  const { client: MongoClient, closeConnectionFn } = await createMongoClient();
  const Products = MongoClient.collection('products');
  const res = await Products.find({});
  const body = await res.toArray();
  
  closeConnectionFn();
  context.res = { status: 200, body };
};
```

**Instalação da dependencia**

`npm i mongodb`

`func host start`

**Para criar produtos**

`func new`

8

CreateProduct

o mesmo para UpdateProducts

```js
const createMongoClient = require('../shared/mongoClient');

module.exports = async function (context, req) {
  const product = req.body || {};

  if (product) {
    context.res = {
      status: 400,
      body: 'Product is required',
    };
  }

  const { client: MongoClient, closeConnectionFn } = await createMongoClient();
  const Products = MongoClient.collection('products');

  try {
    const products = await Products.insert(product);
    closeConnectionFn();
    context.res = { status: 201, body: products.ops[0] };
  } catch (error) {
    context.res = {
      status: 500,
      body: 'Error on insert product',
    }; 
  }
};
```

**Para deletar**

localhost:7071/api/products

**Deploy na Azure**

https://azure.microsoft.com/

**Estrutura**

```
├── CreateProduct
│   ├── function.json
│   └── index.js
├── DeleteProduct
│   ├── function.json
│   └── index.js
├── GetProductById
│   ├── function.json
│   └── index.js
└── GetProduct
    ├── function.json
    └── index.js
```

Arquivo mongoClient.js

```js
const { MongoClient } = require('mongodb');
const config = {
  url: 'mongodb+srv://god:dog@cluster0-dfsvs.mongodb.net/dgo?retryWrites=true&w=majority',
};

module.exports = () => new Promise((resolve, reject) => {
  MongoClient
    .connect(config.url, { useNewUrlParser: true }, (err, mongoConnection) =>
      err
      ? reject(err)
      : resolve({
          client: mongoConnection.db(config.dbName),
          closeConnectionFn: () => setTimeout(() => {
            mongoConnection.close();
          }, 1000),
          mongoConnection,
        })
    );
});
