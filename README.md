# Classe User

Esta classe representa a estrutura de um usuário e será utilizada para mapear os dados retornados do banco de dados MySQL e também os dados que serão serializados para o cache Redis.
Propriedades:

Id: Identificador único do usuário.

Name: Nome do usuário.

Email: E-mail do usuário.

UltimoAcesso: Data do último acesso do usuário.

Essa classe será utilizada tanto para mapear dados retornados do banco de dados como para armazenar os dados no cache Redis.


# Classe de Serviço RedisService

A conexão com o Redis deve ser centralizada em uma classe de serviço para gerenciar as conexões e facilitar a reutilização da conexão.
Lazy<ConnectionMultiplexer>: A classe ConnectionMultiplexer é pesada, portanto, usamos o padrão Lazy Initialization para garantir que a conexão só seja criada quando necessário.

Método GetDatabase(): Retorna a instância do banco de dados Redis.

Método Dispose(): Para fechar a conexão com o Redis, se necessário.


# Serviço MySqlService

O serviço de conexão com o banco de dados MySQL abstrai a lógica de interação com o banco e facilita testes e manutenção.

GetUsersAsync(): Este método consulta os usuários no banco de dados MySQL e retorna uma lista de usuários.


# HomeController
## Lógica de Cache e Fallback

A seguir, a implementação do controller que gerencia a lógica de cache (com Redis) e fallback (com MySQL) quando o cache não está disponível.
Verificação do Cache: A primeira etapa é tentar recuperar os dados do Redis. Se os dados estiverem no cache, ele os retorna diretamente.

Fallback para o MySQL: Caso os dados não estejam no cache, ele consulta o banco de dados MySQL usando o serviço MySqlService. Em seguida, ele serializa os dados em formato JSON e os armazena no Redis.
Cache com Expiração: O Redis é configurado para armazenar os dados por 15 minutos. Após esse período, os dados serão recarregados do banco de dados MySQL.


# Conclusão
Ao usar Redis como cache, evitamos chamadas repetidas ao banco de dados, melhorando a performance da aplicação. Além disso, o uso de boas práticas de programação garante que a aplicação seja mais robusta e fácil de manter no futuro.
