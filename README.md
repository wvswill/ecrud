## ECRUD (Easy-CRUD)

ORM de fácil utilização e simples configuração.

### Configuração (.config)

Para configurar o ECRUD, é necessário que sejam colocadas algumas chaves (key) no seu arquivo .config (Web / App). 

Para o log são necessárias duas chaves (key): 
* PathLog - Especifica a pasta onde será gerado o log. Ex.: C:\ECrudLog
* PathFile - Especifica o arquivo onde será gerado o log. Ex.: C:\ECrudLog\Log.txt

As conexões de banco de dados, são colocadas de acordo com a necessidade, o name das conexões serão utilizados pelo ECRUD, através do Enum que deve ser configurado com os names das conexões.

```c#
<configuration> 
  <appSettings> 
   <add key="PathLog" value="C:\ECrudLog" /> 
   <add key="PathFile" value="C:\ECrudLog\Log.txt" />
   <add key="SmtpClient" /> 
  </appSettings> 
  <connectionStrings> 
   <add name="BancoProd" connectionString="DATA SOURCE=BDPROD;USER ID=USER;PASSWORD=PASS" /> 
   <add name="BancoDev" connectionString="DATA SOURCE=BDDEV;USER ID=USER;PASSWORD=PASS" /> 
  </connectionStrings> 
</configuration>
```

### Configuração (Enumerador)

Para que o ECRUD consiga se comunicar com seu .config (Web / App), é necessário que seja criada uma classe. 
Nesta classe será criado um Enum (Enumerador), que irá conter as chaves (keys) do seu .config (Web / App)

```c#
public class Enums 
{ 
  public enum Bancos 
  { 
     BancoProd, 
     BancoDev 
  } 
} 
```

### Utilização (Oracle Basic)

* GetQuantity - Retorna a quantidade de registros encontrados baseado na instrução SQL

```c#
public long Teste() 
{ 
  using (var Oracle = new ECRUD.Oracle()) 
  { 
    string strQuery = "SELECT COUNT(ID) FROM PRODUCT"; 
    return Oracle.GetQuantity(strQuery, Enums.Bancos.BancoProd); 
  } 
}
```

* GetDataTable - Retorna uma tabela de dados (DataTable) com base na instrução SQL

```c#
public long Teste() 
{ 
  using (var Oracle = new ECRUD.Oracle()) 
  { 
    string strQuery = "SELECT * FROM PRODUCT"; 
    return Oracle.GetQuantity(strQuery, Enums.Bancos.BancoProd); 
  } 
}
```

* GetDataTableWithException - Retorna uma tabela de dados (DataTable), e erros de execução com base na instrução SQL

```c#
public long Teste() 
{ 
  using (var Oracle = new ECRUD.Oracle()) 
  { 
    string strQuery = "SELECT * FROM PRODUCT"; 
    return Oracle.GetQuantity(strQuery, Enums.Bancos.BancoProd); 
  } 
}
```

* GetDataTableBind - Retorna uma tabela de dados (DataTable) com base na instrução SQL utilizando Bind Variables

```c#
public long Teste() 
{ 
  using (var Oracle = new ECRUD.Oracle()) 
  { 
    string strQuery = "SELECT * FROM PRODUCT"; 
    return Oracle.GetQuantity(strQuery, Enums.Bancos.BancoProd); 
  } 
}
```

* GetDataTableBind - Retorna uma tabela de dados (DataTable) com base na instrução SQL utilizando Bind Variables

```c#
public long Teste() 
{ 
  using (var Oracle = new ECRUD.Oracle()) 
  { 
    string strQuery = "SELECT * FROM PRODUCT"; 
    return Oracle.GetQuantity(strQuery, Enums.Bancos.BancoProd); 
  } 
}
```

| Overloads                                                                               | GetQuantity        | GetDataTable       | GetDataTableWithException | GetDataTableBind   |
| :-------------------------------------------------------------------------------------- | :----------------: | :----------------: | :-----------------------: | :----------------: |
| String strQuery, Enum DataBase                                                          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:        | :heavy_check_mark: | 
| String strQuery, Enum DataBase, String[,] Params                                        | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:        | :heavy_check_mark: | 
| String strQuery, Enum DataBase, Object[,] Params                                        | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:        | :heavy_check_mark: | 
| String strQuery, String strDataBase, String strSchema, String strPass                   | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:        | :heavy_check_mark: | 
| String strQuery, String strDataBase, String strSchema, String strPass, String[,] Params | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:        | :heavy_check_mark: | 
| String strQuery, String strDataBase, String strSchema, String strPass, Object[,] Params | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:        | :heavy_check_mark: | 
| String strQuery, OracleConnection strConnection                                         | :x:                | :x:                | :heavy_check_mark:        | :heavy_check_mark: | 

### Utilização (Oracle Mapping)

Classes de exemplo para utilização do Mapping.

```c#
[Serializable]
[ECRUD.NoteSchema("SCHEMA")]
[ECRUD.NoteTable("FUNCIONARIO")]
[ECRUD.PrimaryKey("ID", SequenceType = ECRUD.Enums.SequenceType.Max)]
public class Funcionario
{
    [ECRUD.Coluna("ID")]
    [ECRUD.NoteDataType(ECRUD.Enums.DataTypes.NUMBER)]
    public long? Id { get; set; }

    [ECRUD.Coluna("REGISTRO")]
    [ECRUD.NoteDataType(ECRUD.Enums.DataTypes.NUMBER)]
    public long? Registro { get; set; }

    [ECRUD.Coluna("NOME")]
    [ECRUD.NoteDataType(ECRUD.Enums.DataTypes.VARCHAR2)]
    public string Nome { get; set; }        

    [ECRUD.Coluna("ATIVO")]
    [ECRUD.NoteDataType(ECRUD.Enums.DataTypes.NUMBER)]
    public long? Ativo { get; set; }        

    [ECRUD.OneToOne("USUARIO", "ID", "ID_FUNCIONARIO")]
    public Usuario Usuario { get; set; }
}

[Serializable]
[ECRUD.NoteSchema("SCHEMA")]
[ECRUD.NoteTable("USUARIO")]
[ECRUD.PrimaryKey("ID", SequenceType = ECRUD.Enums.SequenceType.Max)]
public class Usuario
{
    [ECRUD.Coluna("ID")]
    [ECRUD.NoteDataType(ECRUD.Enums.DataTypes.NUMBER)]
    public long Id { get; set; }

    [ECRUD.Coluna("ID_FUNCIONARIO")]
    [ECRUD.NoteDataType(ECRUD.Enums.DataTypes.NUMBER)]
    public long IdFuncionario { get; set; }

    [ECRUD.Coluna("LOGIN")]
    [ECRUD.NoteDataType(ECRUD.Enums.DataTypes.VARCHAR)]
    public string Login { get; set; }

    [ECRUD.Coluna("SENHA")]
    [ECRUD.NoteDataType(ECRUD.Enums.DataTypes.VARCHAR)]
    public string Senha { get; set; }

    [ECRUD.OneToMany("ACESSO", "ID", "ID_USUARIO")]
    public List<Acesso> Acessos { get; set; }
}

[Serializable]
[ECRUD.NoteSchema("SCHEMA")]
[ECRUD.NoteTable("ACESSO")]
[ECRUD.PrimaryKey("ID", SequenceType = ECRUD.Enums.SequenceType.None)]
public class Acesso
{
    [ECRUD.Coluna("ID")]
    [ECRUD.NoteDataType(ECRUD.Enums.DataTypes.NUMBER)]
    public long Id { get; set; }

    [ECRUD.Coluna("ID_USUARIO")]
    [ECRUD.NoteDataType(ECRUD.Enums.DataTypes.VARCHAR)]
    public string IdUsuario { get; set; }

    [ECRUD.Coluna("ATIVO")]
    [ECRUD.NoteDataType(ECRUD.Enums.DataTypes.VARCHAR)]
    public long? Ativo { get; set; }
}
```


* Find - Retorna um modelo com todos seus relacionamentos com base no Id informado.

```c#
public Model.Funcionario GetFuncionario(long id)
{
  using (var Ecrud = new ECRUD.OracleMapping())
  {
    Funcionario _funcionario = new Funcionario();

    _funcionario = Ecrud.Find<Funcionario>(id, Enums.Bancos.BancoProd);

    return _funcionario;
  }
}
```
