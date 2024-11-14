
# OperacoesBD de Leticya

A biblioteca `operacoesbd` oferece um conjunto de funções para facilitar operações de manipulação de dados em um banco de dados MySQL usando Python e `mysql.connector`. Essa biblioteca inclui funcionalidades para conexão, inserção, listagem, atualização e exclusão de dados, com tratamento de exceções para assegurar o controle transacional em caso de erros.

## Instalação

Para utilizar a biblioteca, é necessário instalar o conector MySQL para Python. Caso ainda não tenha instalado, você pode usar o seguinte comando:

```bash
pip install mysql-connector-python
```

## Funcionalidades

### 1. `criarConexao(endereco, usuario, senha, bancodedados)`

Inicializa a conexão com o banco de dados.

- **Parâmetros**:
  - `endereco` (str): Endereço do servidor MySQL.
  - `usuario` (str): Nome de usuário do banco de dados.
  - `senha` (str): Senha do usuário.
  - `bancodedados` (str): Nome do banco de dados a ser utilizado.
- **Retorno**: Um objeto de conexão (`connection`) se bem-sucedido; `None` caso ocorra um erro.

### 2. `encerrarConexao(connection)`

Encerra a conexão com o banco de dados.

- **Parâmetros**:
  - `connection` (obj): Objeto de conexão ativo com o banco de dados.
- **Retorno**: Nenhum.

### 3. `insertNoBancoDados(connection, sql, dados)`

Insere um registro no banco de dados com suporte a prepared statements e rollback em caso de falha.

- **Parâmetros**:
  - `connection` (obj): Objeto de conexão.
  - `sql` (str): Comando SQL de inserção.
  - `dados` (tuple): Dados a serem inseridos.
- **Retorno**: ID do registro inserido ou `None` em caso de erro.

### 4. `listarBancoDados(connection, sql, params=None)`

Lista registros do banco de dados com suporte a prepared statements.

- **Parâmetros**:
  - `connection` (obj): Objeto de conexão.
  - `sql` (str): Comando SQL de consulta.
  - `params` (tuple, opcional): Parâmetros para a consulta.
- **Retorno**: Lista de registros encontrados ou lista vazia em caso de erro.

### 5. `atualizarBancoDados(connection, sql, dados)`

Atualiza registros no banco de dados com rollback em caso de erro.

- **Parâmetros**:
  - `connection` (obj): Objeto de conexão.
  - `sql` (str): Comando SQL de atualização.
  - `dados` (tuple): Dados para atualização.
- **Retorno**: Número de linhas afetadas ou `0` em caso de erro.

### 6. `excluirBancoDados(connection, sql, dados)`

Exclui registros do banco de dados com rollback em caso de erro.

- **Parâmetros**:
  - `connection` (obj): Objeto de conexão.
  - `sql` (str): Comando SQL de exclusão.
  - `dados` (tuple): Dados para exclusão.
- **Retorno**: Número de linhas afetadas ou `0` em caso de erro.

## Exemplo de Uso

```python
from operacoesbd import criarConexao, encerrarConexao, insertNoBancoDados, listarBancoDados, atualizarBancoDados, excluirBancoDados

# Conectar ao banco de dados
conexao = criarConexao("localhost", "usuario", "senha", "meubanco")

# Inserir dados
sql_insert = "INSERT INTO tabela (coluna1, coluna2) VALUES (%s, %s)"
dados_insert = ("valor1", "valor2")
id_inserido = insertNoBancoDados(conexao, sql_insert, dados_insert)

# Listar dados
sql_select = "SELECT * FROM tabela WHERE coluna1 = %s"
resultados = listarBancoDados(conexao, sql_select, ("valor1",))

# Atualizar dados
sql_update = "UPDATE tabela SET coluna2 = %s WHERE coluna1 = %s"
linhas_afetadas = atualizarBancoDados(conexao, sql_update, ("novo_valor", "valor1"))

# Excluir dados
sql_delete = "DELETE FROM tabela WHERE coluna1 = %s"
linhas_afetadas_exclusao = excluirBancoDados(conexao, sql_delete, ("valor1",))

# Fechar a conexão
encerrarConexao(conexao)
```

## Considerações

A biblioteca `operacoesbd` inclui tratamento de exceções em cada operação, garantindo que a transação seja revertida em caso de erros. Este controle ajuda a manter a consistência dos dados no banco de dados.

