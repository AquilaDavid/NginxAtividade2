# Apache Bench e execução de um teste de carga no Home de uma  React Js.

## 1° passo:

User esse comando para instalar o Apache Bench: `apt update && apt install -y apache2-utils`

## 2° passo:

Verifique se o Nginx está funcionando corretamente. Para isso, você pode testar com:

`curl http://localhost`

Se retornar o HTML da aplicação, значит o servidor está ativo e pronto para testes.

3° passo:

Execute o teste de carga com o seguinte comando:

`ab -n 100000 -c 100 http://localhost/`

### O que esta acontencendo?

No primeiro teste, ao utilizar:

`ab -n 100000 -c 100 http://localhost:8080/`

foi gerado o erro Connection refused, pois essa porta não existe dentro do container. A porta 8080 é utilizada apenas no host (Windows), devido ao mapeamento de portas do Docker.

![6](/6.png)

Já no comando corrigido:

`ab -n 100000 -c 100 http://localhost/`

o teste é realizado diretamente na porta 80, onde o Nginx está rodando dentro do container, permitindo que as requisições sejam processadas corretamente.

Resultado do teste:

O teste foi executado com sucesso, realizando 100.000 requisições com 100 usuários simultâneos, sem falhas.

Os principais resultados foram:

Aproximadamente 4649 requisições por segundo
Tempo médio de resposta de 21 ms
Nenhuma requisição falhou

![7](/7.png)
![8](/8.png)

## Minhas Observações do teste

A aplicação React rodando no Nginx conseguiu responder bem mesmo com muitas requisições ao mesmo tempo. Também foi possível perceber que usar a URL correta faz toda a diferença, principalmente quando se está trabalhando dentro de um container, já que nem todas as portas funcionam da mesma forma dentro e fora dele.
