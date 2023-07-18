# Bootcamp-banco-Pan-DynamoDB
Tive Várias dificuldades em utilizar os comandos no AWS cli, muito provavelmente por utilizar SO Windows.
para reverter os problemas que tive fiz da seguinte maneira:<br> 
1-criar um usuário, para isso basta pesquisar por IAM no AWS, localizar usuários nas opções e prosegguir com a criação<br> 
2-vou deixar abaixo os comandos funcionais para Windows<br> 
2.5-não consegui utilizar os esquemas de chave diretamente no terminal então optei por utilizar um arquivo JSON local que possua os esquemas<br> 
3- sempre que ver<br> 
```
file://
```
altere para o seu diretório que contenha o arquivo com o esquema que deseja utilizar

-criar tabela
```
aws dynamodb create-table --cli-input-json file://c:/Users/eriqu/Desktop/bootcamp-cli.json --region sa-east-1
```

-inserir item
```
aws dynamodb put-item --table-name Music --item file://c:/Users/eriqu/Desktop/itemMusic-cli.json
```

-inserior vários itens
```
aws dynamodb batch-write-item --request-items file://c:/Users/eriqu/Desktop/variosItens-cli.json
```

-ver itens
```
aws dynamodb scan --table-name Music
```

-buscar por artista
```
aws dynamodb query --table-name Music --key-condition-expression "Artist = :artist" --expression-attribute-values  file://c:/Users/eriqu/Desktop/testedebusca.json
```

-buscar pela musica - precisa de uma partition key(criar index)
```
aws dynamodb query --table-name Music --key-condition-expression "SongTitle = :title" --expression-attribute-values file://c:/Users/eriqu/Desktop/testedebuscatitle.json
```

-atualizar a tabela para utilizar os outros atributos com index secundários e poder serem utilizados para busca


-Criar um index global secundário baseado no título do álbum
```
aws dynamodb update-table --table-name Music --attribute-definitions AttributeName=AlbumTitle,AttributeType=S --global-secondary-index-updates  "[{\"Create\":{\"IndexName\": \"AlbumTitle-index\",\"KeySchema\":[{\"AttributeName\":\"AlbumTitle\",\"KeyType\":\"HASH\"}],\"ProvisionedThroughput\": {\"ReadCapacityUnits\": 10, \"WriteCapacityUnits\": 5      },\"Projection\":{\"ProjectionType\":\"ALL\"}}}]"
```

-Pesquisa pelo index secundário baseado no título do álbum
```
aws dynamodb query --table-name Music --index-name AlbumTitle-index --key-condition-expression "AlbumTitle = :name" --expression-attribute-values  file://c:/Users/eriqu/Desktop/pesquisarAlbum.json
```
