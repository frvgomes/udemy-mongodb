[MongoDB: The Developer Data Platform](http://www.mongodb.com)

> Existem duas certificações do mongo.
> 
> 
> C100DEV - Developments
> 
> C100DBA - Database Adminstrators
> 

# Comandos Básicos
    
- Iniciar Serviço do mongo
    
    ```jsx
    sudo systemctl start mongod
    ```
    
- Verificar o status do mongo
    
    ```jsx
    sudo systemctl status mongod
    ```
    
- Acessar um banco de dados
  
  Observação: Caso o banco não exista ele vai criar o banco, porém é necessário incluir dados para ele persistir.
    
    ```bash
    	use novo-banco
    ```
    
- Criar uma collection
    
    Para criar uma nova collection.
    
    ```bash
    db.createCollection("clientes")
    ```
    
    Para criar uma nova collection inserindo dados.
    
    ```bash
    db.posts.insert({nome:"José", postagem:"Bons Produtos", data:"31-06-209"})
    ```
    
    Para criar vários documentos em uma collection
    
    ```bash
    db.posts.insert([
    	{nome:"André", postagem:"Produtos Caros", data:"12-01-2019", idade:25},
    	{nome:"Ricardo", postagem:"Produtos caros", data:"14-07-2019", idade:12}
    ])
    ```
    
- Variações do insert
    
    insertOne
    
    insetMany
    
    Ordered é um parâmetro que basicmente define se  no momento de uma inclusão de vários registros e houver um erro (ex:chave duplicada) o mongo vai encerrar o processo e não inserir os demais dergistros ou deve inserir os registros possíveis.
    
    ordered:true é padrão e nao precisa ser informado. Desta forma interrompe a operação caso haja um erro.
    
    ordered:false vai seguir o processamento.
    
    ```jsx
    db.testInsertMany.insertMany([{_id:2, a:"123"},{_id:7, a:"456"},{_id:5, a:"789"}], {ordered:false})
    ```
    
# Find
    
- find - Recupera todos os registros se vazio ou pode-se passar parâmetro de pesquisa.
    
- findOne - Recupera um único registro
    ```bash
    db.posts.find({nome:"José"})
    db.posts.find({nome:"José", postage:"Bons Produtos"}) #and
    db.posts.findOne()
    db.posts.find().pretty()
    ```
    
# Filtros
    
| Função | Finalidade           | 
|--------| -------------------  |
| $eq    | igual                |
| $gt    | maior que            |
| $gte   | maior ou igual que   |
| $lt    | menor que            |
| $lte   | menor ou igual que   |
| $ne    | deferente de         |
| $in    | contém               |
| $nin   | não contém           |
| $or    | ou                   |

- _Exemplos_

    ```
    db.posts.find({postagem:"Produtos caros", idade: {$lte:12}})
        
    db.posts.find({$or:[{nome:"José"}, {nome:"Antonio"}]})
        
    db.posts.find({postagem: {$in: ["Produtos Caros","Loja Suja"]}})
    ```
    
- _Filtro pelo objectId_
    
    ```
    {_id: ObjectId('58f8085dc1840e050034d98f')}
    ```
    
# Projeção
    
- Estrutura de busca de dados onde é possível definir quais campos não devem ser retornados.
    
    ```bash
    db.posts.find({nome:"José"},{_id:0, nome:0})
    ```
    
# Limit
    
- Equivalente ao comando top do sql em um banco de dados relacional.
    
    ```bash
    db.posts.find().limit(2)
    ```
    
- Ordenação
    
    Equivalente ao order by do sql em um banco de dados relacional.
    
    1 :  crescente
    
    -1 : decrescente
    
    ```bash
    db.post.find().sort({nome:1})
    ```
    
# Update
    
- O comando update atualiza um documento se ele existir, se não existir ele cria um novo.
    
    ```bash
    db.posts.update({nome:"André"}, {$set:{idade:29}})
    ```
    
# UpdateOne
    
- O updateOne possui a seguinte estrutura:
  db.collection.updateOne(filter, update, options)
    
  ```jsx
    db.collection.updateOne(
       <filter>,
       <update>,
       {
         upsert: <boolean>,
         writeConcern: <document>,
         collation: <document>,
         arrayFilters: [ <filterdocument1>, ... ],
         hint:  <document|string>        // Available starting in MongoDB 4.2.1
       }
    )
  ```
    
# ReplaceOne
    
- Subistitui o documento inteiro.
    
    ```jsx
    db.testInsertMany.replaceOne({_id: 8}, {x: 200})
    
    meudb> db.testInsertMany.find()
    [
      { _id: 1, a: '123' },
      { _id: 2, a: '456' },
      { _id: 3, a: '789' },
      { _id: 7, a: '456' },
      { _id: 5, a: '789' },
      { _id: 4, b: '123' },
      { _id: 6, b: '123' },
      **{ _id: 8, x: 200 }**
    ]
    ```
    
# Delete
    
- deleteOne() : Excluir um único documento mesmo que o critério retorne vários.
    
- deleteMany() : Excluir todos os documentos conforme critério.
    
- remove() : Excluir todos os documentos da coleção.
    
  ```bash
    db.posts.deleteOne({nome:"André"})
  ```
    
- Excluir Collection
    
  Excluir a coleção e todos os seus documentos
    
  ```bash
    db.posts.drop()
  ```
  