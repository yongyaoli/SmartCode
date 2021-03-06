<p align="center">
  <img width="100"src="./doc/Logo.png"/>
</p>

# SmartCode([Chinese Document](./README.md))

> SmartCode = IDataSource -> IBuildTask -> IOutput => Build Everything

## Introduction

![SmartCode](./doc/SmartCode-EN.png)

## SmartCode.Db (Code generator)

### Demo

![SmartCode](./doc/SmartCode-Db-1.gif)

### Getting Started

1. Install from .NET Core Global Tool  

  ``` shell
  dotnet tool install --global SmartCode.CLI
  ```

2. edit build configuration file (default: SmartCode.yml)
3. the command line executes the SmartCode command.
    - SmartCode
    - wait for prompt to enter the configuration file path (optional: SmartCode.yml file in the default program root directory)
    - carriage return execution command
4. wait for the end of the task execution.
5. View output directory results
6. run the API project and debug with Swagger.

### Building configuration files

``` yml
Module: SmartSql.Starter
Author: Ahoo Wang
DataSource:
  Name: Db
  Paramters:
    DbName: SmartSqlDB
    DbProvider: SqlServer
    ConnectionString: Data Source=.;Initial Catalog=SmartSqlDB;Integrated Security=True
Language: CSharp
TemplateEngine: Razor 
Output: 
  Type: File
  Path: 'E://SmartSql-Starter'

# 构建任务
Build:
  ClearDir:
    Type: Clear
    Paramters:
      Dirs: '.'

  Scaffolding:
    Type: MultiTemplate
    Output: 
      Path: '.'
    Paramters:
      Templates: [{Key: 'Sln.cshtml',Output: {Name: '{{Project.Module}}',Extension: '.sln'}},
        {Key: "Proj-Entity.cshtml",Output: {Path: '{{Project.Module}}.Entity',Name: '{{Project.Module}}.Entity',Extension: '.csproj'}},
        {Key: "Proj-Repository.cshtml",Output: {Path: '{{Project.Module}}.Repository',Name: '{{Project.Module}}.Repository',Extension: '.csproj'}},
        {Key: "Proj-Service.cshtml",Output: {Path: '{{Project.Module}}.Service',Name: '{{Project.Module}}.Service',Extension: '.csproj'}},
        {Key: "Proj-API.cshtml",Output: {Path: '{{Project.Module}}.API',Name: '{{Project.Module}}.API',Extension: '.csproj'}},
        {Key: "API/LaunchSettings.cshtml",Output: {Path: '{{Project.Module}}.API/Properties',Name: 'launchSettings',Extension: '.json'}},
        {Key: "API/AppSettings.cshtml",Output: {Path: '{{Project.Module}}.API',Name: 'appsettings',Extension: '.json'}},
        {Key: "API/AppSettings-Development.cshtml",Output: {Path: '{{Project.Module}}.API',Name: 'appsettings.Development',Extension: '.json'}},
        {Key: "API/Program.cshtml",Output: {Path: '{{Project.Module}}.API',Name: 'Program',Extension: '.cs'}},
        {Key: "API/Startup.cshtml",Output: {Path: '{{Project.Module}}.API',Name: 'Startup',Extension: '.cs'}},
        {Key: "API/APIException.cshtml",Output: {Path: '{{Project.Module}}.API/Exceptions',Name: 'APIException',Extension: '.cs'}},
        {Key: "API/GlobalExceptionFilter.cshtml",Output: {Path: '{{Project.Module}}.API/Filters',Name: 'GlobalExceptionFilter',Extension: '.cs'}},
        {Key: "API/GlobalValidateModelFilter.cshtml",Output: {Path: '{{Project.Module}}.API/Filters',Name: 'GlobalValidateModelFilter',Extension: '.cs'}},
        {Key: "API/QueryRequest.cshtml",Output: {Path: '{{Project.Module}}.API/Messages',Name: 'QueryRequest',Extension: '.cs'}},
        {Key: "API/QueryByPageRequest.cshtml",Output: {Path: '{{Project.Module}}.API/Messages',Name: 'QueryByPageRequest',Extension: '.cs'}},
        {Key: "API/ResponseMessage.cshtml",Output: {Path: '{{Project.Module}}.API/Messages',Name: 'ResponseMessage',Extension: '.cs'}},
        {Key: "API/QueryResponse.cshtml",Output: {Path: '{{Project.Module}}.API/Messages',Name: 'QueryResponse',Extension: '.cs'}},
        {Key: "API/QueryByPageResponse.cshtml",Output: {Path: '{{Project.Module}}.API/Messages',Name: 'QueryByPageResponse',Extension: '.cs'}},
        {Key: "API/ResponseMessage.cshtml",Output: {Path: '{{Project.Module}}.API/Messages',Name: 'ResponseMessage',Extension: '.cs'}},
        {Key: "SqlMapConfig.cshtml",Output: {Path: '{{Project.Module}}.API',Name: 'SmartSqlMapConfig',Extension: '.xml'}}
        ]

  Entity:
    Type: Table
    Module: Entity
    Template: Entity.cshtml
    Output: 
      Path: '{{Project.Module}}.{{Build.Module}}'
      Name: '{{Items.CurrentTable.ConvertedName}}'
      Extension: '.cs'
    NamingConverter:
      Table:
        Tokenizer: 
          Type: Default
          Paramters: 
            IgnorePrefix: 'T_'
            Delimiter: '_'
        Converter:
          Type: Pascal
          Paramters: { }
      View:
        Tokenizer:
          Type: Default
          Paramters: 
            IgnorePrefix: 'V_'
            Delimiter: '_'
        Converter:
          Type: Pascal
      Column:
        Tokenizer:
          Type: Default
          Paramters: 
            Delimiter: '_'
        Converter:
          Type: Pascal

  Repository:
    Type: Table
    Module: Repository
    Template: Repository.cshtml
    IgnoreNoPKTable: true
    IgnoreView: true
    Output: 
      Path: '{{Project.Module}}.{{Build.Module}}'
      Name: 'I{{Items.CurrentTable.ConvertedName}}Repository'
      Extension: .cs
    NamingConverter:
      Table:
        Tokenizer:
          Type: Default
          Paramters: 
            IgnorePrefix: 'T_'
            Delimiter: '_'
        Converter:
          Type: Pascal

  Service:
    Type: Table
    Module: Service
    Template: Service.cshtml
    IgnoreNoPKTable: true
    IgnoreView: true
    Output: 
      Path: '{{Project.Module}}.{{Build.Module}}'
      Name: '{{Items.CurrentTable.ConvertedName}}Service'
      Extension: .cs
    NamingConverter:
      Table:
        Tokenizer:
          Type: Default
          Paramters: 
            IgnorePrefix: 'T_'
            Delimiter: '_'
        Converter:
          Type: Pascal

  APIController:
    Type: Table
    Module: API
    Template: API/APIController.cshtml
    IgnoreNoPKTable: true
    IgnoreView: true
    Output: 
      Path: '{{Project.Module}}.{{Build.Module}}/Controllers'
      Name: '{{Items.CurrentTable.ConvertedName}}Controller'
      Extension: .cs
    NamingConverter:
      Table:
        Tokenizer:
          Type: Default
          Paramters: 
            IgnorePrefix: 'T_'
            Delimiter: '_'
        Converter:
          Type: Pascal

  SqlMap:
    Type: Table
    Template: SqlMap.cshtml
    Output: 
      Path: '{{Project.Module}}.API/Maps'
      Name: '{{Items.CurrentTable.ConvertedName}}'
      Extension: .xml
    IgnoreNoPKTable: true
    IgnoreView: true
    NamingConverter:
      Table:
        Tokenizer:
          Type: Default
          Paramters: 
            IgnorePrefix: 'T_'
            Delimiter: '_'
        Converter:
          Type: Pascal
      View:
        Tokenizer:
          Type: Default
          Paramters: 
            IgnorePrefix: 'V_'
            Delimiter: '_'
        Converter:
          Type: Pascal
      Column:
        Tokenizer:
          Type: Default
          Paramters: 
            IgnorePrefix: 'T_'
            Delimiter: '_'
        Converter:
          Type: Pascal
```
### 构建文件参数概览

| Parameter Name | Description |
| :--------- | --------:|
| Module | Root Module Name |
| Author | Author |
| DataSource | Data Source |
| Language | Language: CSharp/Java/.... |
| TemplateEngine | Template Engine: Currently Built: Razor/Handlebars |
| Output | Output |
| Build | Task Build |

#### DataSource Data Source, Name: Db

> Property Name: Db, using the DbSource plugin as a data source

DbSource.Paramters accepts the following three parameters:

| Parameter Name | Description |
| :--------- | --------:|
| DbName | Database Name |
| DbProvider | Data Drivers: MySql, MariaDB, PostgreSql, SqlServer, Oracle, SQLite |
| ConnectionString | Connection String |

#### Build Task Build

| Parameter Name | Description |
| :--------- | --------:|
| Type | Build type, Clear: used to clean up the directory s / file s, Project: used to build a single file, such as: solution file / project file, Table: used to build a data table-based file, such as: Entity , Repository file|
| Module | Building Module Name |
| TemplateEngine | Template Engine, optional, default to root module engine |
| Template | Template File |
| Output | Output |
| IgnoreNoPKTable | Ignore no PK Table |
| IgnoreView | Ignore view |
| IncludeTables | Include table name s |
| IgnoreTables | Ignore table name s |
| NamingConverter | Named Converter |
| Paramters | Custom Build Parameters |

#### NamingConverter Name Conversion

| Attribute | Description |
| :--------- | --------:|
| Type | Table/View/Column |
| Tokenizer | Word Segmenter |
| Converter | Converter: Camel/Pascal/None |

##### NamingConverter.Tokenizer Word Segmenter

| Attribute | Description |
| :--------- | --------:|
| Type | Default |
| Paramters.IgnorePrefix | Ignore prefix characters |
| Paramters.Delimiter | Separator |
| Paramters.UppercaseSplit | Using uppercase separation, default: true |