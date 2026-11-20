# Alura ByteBank

Aplicação de gerenciamento bancário desenvolvida em C# e ASP.NET Core MVC. O projeto simula operações administrativas de um banco, com cadastro e manutenção de agências, clientes e contas correntes, além de autenticação de usuários.

O repositório também contém uma aplicação de console e projetos de testes automatizados.

## Funcionalidades

- Cadastro, consulta, edição e exclusão de agências.
- Cadastro, consulta, edição e exclusão de clientes.
- Cadastro, consulta, edição e exclusão de contas correntes.
- Cadastro e gerenciamento de usuários da aplicação.
- Tela de login com geração e utilização de token JWT.
- Armazenamento do token e de informações da sessão.
- Validação de dados de domínio, incluindo validação de CPF.
- Persistência de dados com Entity Framework Core e MySQL.
- Aplicação de console para operações básicas com agências e contas correntes.

## Tecnologias utilizadas

- **C#**
- **.NET 5**
- **ASP.NET Core MVC**
- **Entity Framework Core 5**
- **MySQL**
- **Pomelo.EntityFrameworkCore.MySql**
- **AutoMapper**
- **JWT (JSON Web Token)**
- **Razor Views**
- **xUnit**
- **Moq**
- **Selenium WebDriver**

## Arquitetura e estrutura da solução

A solução utiliza uma organização em camadas, separando regras de negócio, aplicação, persistência e apresentação.

```text
Alura.ByteBank.WebApp-master/
├── Alura.ByteBank.sln
├── Alura.ByteBank.Domain/               # Entidades, regras e contratos de domínio
├── Alura.ByteBank.Aplicacao/            # Serviços de aplicação e DTOs
├── Alura.ByteBank.Dados/                # Contexto, repositórios e migrations
├── Alura.ByteBank.Apresentacao/         # Aplicação de console e comandos
├── Alura.ByteBank.WebApp/               # Aplicação web ASP.NET Core MVC
│   ├── Controllers/
│   ├── Views/
│   ├── Util/
│   ├── ViewModel/
│   ├── wwwroot/
│   ├── Program.cs
│   └── Startup.cs
├── Alura.ByteBank.Dominio.Testes/       # Testes das regras de domínio
├── Alura.ByteBank.Infraestrutura.Testes/ # Testes de persistência e repositórios
├── Alura.ByteBank.WebApp.Testes/        # Testes de interface e fluxo web
└── Alura.ByteBank/                      # Projeto de console adicional
```

### Responsabilidade dos projetos

| Projeto | Responsabilidade |
|---|---|
| `Alura.ByteBank.Domain` | Entidades e regras centrais do domínio, como `Agencia`, `Cliente`, `ContaCorrente` e `UsuarioApp`. |
| `Alura.ByteBank.Aplicacao` | Serviços de aplicação, interfaces e objetos de transferência de dados (DTOs). |
| `Alura.ByteBank.Dados` | Contexto do Entity Framework Core, repositórios e migrações do banco. |
| `Alura.ByteBank.Apresentacao` | Interface de console para operações com agências e contas correntes. |
| `Alura.ByteBank.WebApp` | Aplicação web MVC, controllers, views Razor, sessão e autenticação. |
| `Alura.ByteBank.Dominio.Testes` | Testes automatizados das regras de domínio. |
| `Alura.ByteBank.Infraestrutura.Testes` | Testes de repositórios e infraestrutura de dados. |
| `Alura.ByteBank.WebApp.Testes` | Testes de navegação e autenticação da aplicação web com Selenium. |

## Pré-requisitos

Para executar a solução, você precisará de:

- Visual Studio 2019 ou superior, ou Visual Studio Code.
- SDK do **.NET 5** compatível com a solução.
- Servidor **MySQL** em execução.
- Google Chrome e ChromeDriver compatível, caso queira executar os testes de interface.

> **Atenção:** o .NET 5 está fora do período de suporte oficial. Para uso em produção, planeje a atualização para uma versão LTS atualmente suportada do .NET e revise a compatibilidade dos pacotes.

## Como executar

### 1. Clone o repositório

```bash
git clone https://github.com/SEU-USUARIO/SEU-REPOSITORIO.git
cd SEU-REPOSITORIO
```

Substitua a URL pelo endereço do repositório.

### 2. Configure o MySQL

O contexto `ByteBankContexto`, localizado em `Alura.ByteBank.Dados/Contexto/ByteBankContexto.cs`, configura atualmente a conexão diretamente no método `OnConfiguring`, utilizando:

- Servidor: `localhost`
- Banco de dados: `bytebankBD_webapp`
- Usuário: `root`

Ajuste a conexão para o ambiente local e garanta que o banco e o usuário existam.

**Importante:** não versione senhas ou credenciais reais. O ideal é mover a connection string para configuração externa (por exemplo, variáveis de ambiente ou User Secrets) e lê-la por injeção de dependência, em vez de mantê-la no código-fonte.

### 3. Restaure os pacotes

Na pasta da solução:

```bash
dotnet restore Alura.ByteBank.sln
```

### 4. Aplique as migrações

O projeto contém migrações do Entity Framework Core em `Alura.ByteBank.Dados/Migrations`.

A partir da raiz da solução, com a ferramenta `dotnet-ef` compatível instalada e a conexão configurada:

```bash
dotnet tool install --global dotnet-ef --version 5.0.12
dotnet ef database update --project Alura.ByteBank.Dados --startup-project Alura.ByteBank.WebApp
```

Caso a ferramenta já esteja instalada, utilize `dotnet tool update` para ajustar a versão.

### 5. Execute a aplicação web

```bash
dotnet run --project Alura.ByteBank.WebApp
```

O terminal informará os endereços locais para acessar a aplicação. Abra o endereço HTTPS ou HTTP indicado pelo ASP.NET Core.

### 6. Execute a aplicação de console

A solução também possui uma interface de console no projeto `Alura.ByteBank.Apresentacao`:

```bash
dotnet run --project Alura.ByteBank.Apresentacao
```

O menu permite cadastrar agências, cadastrar contas correntes e listar contas correntes.

## Testes automatizados

Os projetos de testes utilizam xUnit. Para executar os testes da solução:

```bash
dotnet test Alura.ByteBank.sln
```

Os testes estão organizados em três projetos:

- `Alura.ByteBank.Dominio.Testes`: validações e comportamentos das entidades de domínio.
- `Alura.ByteBank.Infraestrutura.Testes`: testes relacionados aos repositórios e ao contexto de dados.
- `Alura.ByteBank.WebApp.Testes`: testes de navegação e autenticação utilizando Selenium WebDriver.

Os testes de infraestrutura e de interface podem depender de serviços externos/configurações locais, como banco de dados e navegador.

## Modelo de domínio

O domínio possui as seguintes entidades centrais:

- **Agência:** representa uma agência bancária, com número, nome, endereço e coleção de contas.
- **Cliente:** representa o titular de contas, com nome, CPF, profissão e coleção de contas.
- **Conta corrente:** representa uma conta vinculada a um cliente e a uma agência, com número, saldo e identificador Pix.
- **Usuário da aplicação:** armazena os dados de acesso utilizados no fluxo de autenticação.

As entidades e seus relacionamentos são mapeados pelo `ByteBankContexto`, que utiliza Entity Framework Core.

## Segurança e observações

- O projeto implementa autenticação baseada em JWT e mantém o token na sessão web.
- O segredo utilizado para assinar tokens deve ser mantido fora do código-fonte e substituído por um valor forte e exclusivo em cada ambiente.
- A configuração atual do contexto de dados contém parâmetros de conexão diretamente no código. Revise essa configuração antes de publicar ou implantar a aplicação.
- A implementação de autenticação e armazenamento de senhas deve ser revisada antes de qualquer uso real. Prefira mecanismos consolidados de gerenciamento de identidade e hash de senha com salt e fator de trabalho apropriado.
- O projeto foi desenvolvido com .NET 5 e versões antigas de dependências; avalie atualizações e possíveis mudanças de API antes de migrar.

## Licença

Não foi identificada uma licença nos arquivos fornecidos. Antes de permitir o uso, modificação ou redistribuição por terceiros, escolha uma licença e inclua o respectivo arquivo `LICENSE` no repositório.
