# database-query
Desafio: Realizará consultas no banco de dados com o TypeORM de três maneiras.

Abrir projeto com VSCode Online:

https://github1s.com/MyIgnite/database-query

Clone o projeto, navegue até a raiz do projeto e execute:</br>

`yarn` </br>

Execute o comando para testar a aplicação </br>
`yarn test` </br> </br>

- [x] should be able to find user with games list by user's ID
- [x] should be able to list users ordered by first nam
- [x] should be able to find user by full nam
- [x] should be able find a game by entire or partial given titl
- [x] should be able to get the total count of game
- [x] should be able to list users who have given game i

## UsersRepository.ts

O método busca um usuário com seus jogos relacionados a partir de um ID de usuário `user id`. A buscar é realizada usando o ORM que retorna os usuários com os jogos relacionados. 

```js
async findUserWithGamesById({
    user_id,
  }: IFindUserWithGamesDTO): Promise<User> {
    // Complete usando ORM
    const userWithGames = await this.repository.findOneOrFail({
      relations: ["games"],
      where: {
        id: user_id
      }
    });

    return userWithGames;
  }
```

O método busca todos os usuários com ordem crecente.
A consulta é do tipo puro, ou seja, através de uma string sem utilizar métodos ou sintaxes espefíficas de um ORM ou biblioteca de bando de dados.

```js
async findAllUsersOrderedByFirstName(): Promise<User[]> {
  // Complete usando raw query
  const query = "SELECT * FROM users ORDER BY first_name ASC";
  const users = await this.repository.query(query);
  return users;
  }
```

O método retorna todos os usuários, independente da capitalização de sues nomes.
Por exemplo, nomes como "JoSé", "Maria", "JoÃo" serão incluídos nos resultados.

```js
async findUserByFullName({
  first_name,
  last_name,
}: IFindUserByFullNameDTO): Promise<User[] | undefined> {
  // Complete usando raw query
  // Retornar usuário que possua first_name e last_name
  // Ignorar caixa alta

  const query = "SELECT * FROM users WHERE LOWER(first_name) = LOWER($1) AND LOWER(last_name) = LOWER($2)";
  const users = await this.repository.query(query, [first_name, last_name])
  return users; 
}
```