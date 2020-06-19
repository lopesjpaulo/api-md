FORMAT: A1

# Ecommerce API

API de integração da loja virtual do cliente Armas e Bagagens e o sistema de controle
de dados e estoque da Pegassus.

### Paginação 

    base_url/api/orders?page=1

# Group Categories

Recursos relativos a categorias de Produtos

## Category [/categories]

### Criar uma nova categoria [POST]

Cria uma nova categoria

+ Request
    + Attributes
        + name (string, required)
        + description (string)
        + status: 1 (number)

+ Response 200

## Category [/categories/{id}]

### Editar uma categoria [PUT]

Editar uma categoria

+ Response 201

# Group Stock

Recursos relativos a estoque de produtos

## Stock [/stock/{id}]

### Atualizar o estoque [PUT]

Atualizar o estoque de um produto ou o estoque de uma opção do produto. Se o produto não tiver nenhuma propriedade
(cor, tamanho, capacidade...) deverá ser enviado somente o atributo **quantity**. Caso deseje alterar a quantidade
de um atributo, o **productAttributeId** deverá ser enviado. Uma lista de todos os atributos relativos a um produto
podem ser obtidas através de um endpoint de produtos.

+ Request
    + Attributes
        + quantity (number)
        + productAttributeId
        + productAttributeQuantity
    
+ Response 201
    
# Group Options

Recursos relativos a opções dos produtos, ou também chamado de atributos.

## Options [/options]

### Criar uma nova opção(atributo) [POST]

Criar uma nova opção(atributo) do produto.

+ Request (application/json)
    + Attributes
        + name: Capacidade (string, required)
        + values (array[value])
    
+ Exemplo

        {
            "name":"Capacity",
            "values":[
                {"value":"10L"},
                {"value":"20L"},
                {"value":"30L"},
                {"value":"40L"}
            ]
        }

+ Response 200

## Options [/options/{id}]

### Atualizar uma opção(atributo) [PUT]

Nesse recurso só é permitido alterar o nome da opção(atributo). Caso deseje alterar dados dos valores das opções,
outro endpoint deve ser usado.

+ Request
    + Attributes
        + name (string, required)
        
+ Response 201

# Group Option-values

Recursos relativos aos valores das opções(atributos)

## Valores das opções [/option-values]

### Criar um novo valor de opção(atributo) [POST]

+ Request
    + Attributes
        + option_id (number, required)
        + values (array[{name])

+ Exemplo

        {
            "option_id":8,
            "values":[
               {
                  "name":"Vermelho"
               },
               {
                  "name":"Azul"
               }
            ]
        }

+ Response 200

## Valores de opções [/option-values/{id}]

### Atualizar o nome de um valor de opção [PUT]

Alterar o valor de vermelho para verde, por exemplo, no endpoint anterior.

+ Request
    + Attributes
        + value (string, required)    
        
+ Response 201

# Group Products

Recursos relativos a inserção, edição e recuperação de produtos.

## Produtos [/products]

### Inserir um novo produto [POST]

+ Request
    + Attributes
        + sku (string, required)
        + name (string, required, unique)
        + quantity (number, required)
        + price (decimal, required)
        + cover (file, required, image:png,jpeg,jpg)
        
+ Response 200

## Produtos [/products/{id}]

### Atualizar um produto [PUT]

+ Response 201

## Produtos [/products/{id}/product-attributes]

### Recuperar os atributos de um produto [GET]

+ Response 200
    + Attributes
        + id (number) - id da opção
        + quantity (number) - quantidade da opção
        + price (decimal) - preço da opção
        + default (number)
        + attributeValue (array)
    + Body
            
            {
                "id": 3,
                "quantity": 10,
                "price": "8.00",
                "default": 1,
                "attributeValue": [
                    [
                        {
                            "attributeName": "Size"
                        },
                        {
                            "attributeValue": "small"
                        }
                    ]
                ]
            }

# Group Orders 

Recursos relativos aos pedidos

## Pedidos [/orders]

### Obter todos os pedidos [GET]

+ Response 200

### Obter todos os pedidos pelo status [GET]

    base_url/api/orders?status=pending
    
## Pedidos [/orders/{id}]

### Obter os dados de um pedido [GET]

+ Response 200

### Atualizar um pedido [PUT]

+ Request
    + Attributes
        + customer_id (number)
        + address_id (number)
        + order_status_id (number)
            + 1 - pago
            + 2 - pendente
            + 3 - erro
            + 4 - em entrega
            + 5 - pedido feito
        + payment (string)
        + discounts (decimal)
        + total_products (decimal)
        + total_shipping (decimal)
        + tax (decimal)
        + total (decimal)
        + total_paid (decimal)
        + tracking_number (string)
 
 + Response 201
    
 