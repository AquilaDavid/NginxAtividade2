# Apache Bench e execução de um teste de carga no Home de uma  React Js.

# Requisitos:

  1. [Ter concluido os passos de Configuração do Nginx ](https://github.com/AquilaDavid/NginxAtividade2/blob/main/redirecionamento_react.md)
  2. Ter WSL instalado
  3. Ter 8 RAM
  4. Ter o NGINX instalado
  5. Ter o CURL instalado
  6. Ter o DOCKER instalado

## 1° passo:

User esse comando para instalar o Apache Bench: `apt update && apt install -y apache2-utils`

## 2° passo:

Verifique se o Nginx está funcionando corretamente. Para isso, você pode testar com:

`curl http://localhost`

Se retornar o HTML da aplicação, o servidor está ativo e pronto para testes.

3° passo:

Execute o teste de carga com o seguinte comando:

`ab -n 100000 -c 100 http://localhost/`

### O que esta acontencendo?

No primeiro teste, ao utilizar:

`ab -n 100000 -c 100 http://localhost:8080/`

Esse comando usa o Apache Bench (ab) para fazer um teste de carga no servidor.

### Explicação dos parametros.

- `-n 100000` → total de **100.000 requisições**
- `-c 100` → **100 requisições simultâneas** (100 usuários ao mesmo tempo)
- `http://localhost/` → servidor sendo testado (seu próprio PC)

Em resumo:

> Simula 100 usuários acessando ao mesmo tempo até completar 100 mil acessos, para medir o desempenho do servidor.

## Problema que eu enfrentei ao fazer os testes

Foi gerado o erro **Connection refused**, pois a porta utilizada não existia dentro do container. A porta **8080** é exposta apenas no host (Windows), devido ao mapeamento de portas do Docker.

![6](/6.png)

## Resolução

O problema foi corrigido ao utilizar o comando:

`ab -n 100000 -c 100 http://localhost/`

Nesse caso, o teste é realizado diretamente na porta **80**, onde o Nginx está rodando dentro do container. Assim, as requisições conseguem ser processadas corretamente.

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
