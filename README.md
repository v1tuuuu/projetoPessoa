# ProjetoPessoa - API de Gestão de Clientes e Pessoas

Uma Web API RESTful moderna, robusta e modular desenvolvida em **.NET 8** e **C#**, utilizando arquitetura em camadas, **Entity Framework Core**, **SQLite**, validações automáticas com DataAnnotations, paginação e tratamento global de exceções.

---

## 🛠️ Tecnologias Utilizadas

- **.NET 8 (LTS)**
- **C# 12**
- **ASP.NET Core Web API** (Controllers)
- **Entity Framework Core 8** (Code-First com SQLite)
- **Swagger / OpenAPI** (com documentação XML interativa)
- **Injeção de Dependência nativa**
- **Tratamento Global de Erros** (Middleware personalizado)

---

## 📂 Arquitetura da Solução

O projeto segue a separação de responsabilidades em camadas:

```
ProjetoPessoa/
├── Controllers/              # Controladores REST com anotações e roteamento
│   └── PessoasController.cs
├── Services/                 # Regras de negócio, normalizações e validações
│   ├── IPessoaService.cs
│   └── PessoaService.cs
├── DTOs/                     # Objetos de transferência de dados e paginação
│   ├── CriarPessoaDto.cs
│   ├── AtualizarPessoaDto.cs
│   ├── AlterarStatusDto.cs
│   ├── PessoaResponseDto.cs
│   └── PaginacaoResposta.cs
├── Models/                   # Entidades do domínio
│   └── Pessoa.cs
├── Data/                     # Contexto EF Core e mapeamento de banco
│   └── AppDbContext.cs
├── Exceptions/               # Exceções de domínio personalizadas
│   ├── NotFoundException.cs
│   └── ConflictException.cs
├── Middlewares/              # Tratamento centralizado de erros
│   └── ErrorHandlingMiddleware.cs
├── Properties/
│   └── launchSettings.json
├── appsettings.json          # Configuração de conexão com o banco SQLite
├── Program.cs                # Inicialização, injeção de dependências e pipeline
├── ProjetoPessoa.csproj       # Configurações do projeto e pacotes NuGet
└── ProjetoPessoa.http        # Coleção de requisições HTTP para testes rápidos
```

---

## 🚀 Como Executar

### 1. Clonar ou navegar até a pasta
```bash
cd /home/vitor/ProjetoPessoa
```

### 2. Restaurar e Compilar
```bash
dotnet build
```

### 3. Iniciar a API
```bash
dotnet run
```

Ao iniciar, a API:
- Cria automaticamente o arquivo de banco de dados `pessoas.db` via SQLite se ele não existir.
- Aplica as tabelas e índices únicos (`Email`, `CPF`).
- Insere registros iniciais de semente (seed data) para você poder testar imediatamente.
- Disponibiliza o Swagger na raiz: [http://localhost:5073](http://localhost:5073) (ou na porta configurada).

---

## 📡 Endpoints da API

| Método | Rota | Descrição | Status de Sucesso |
|---|---|---|---|
| `GET` | `/api/pessoas` | Listagem paginada (suporta `busca`, `ativo`, `pagina`, `tamanhoPagina`) | `200 OK` |
| `GET` | `/api/pessoas/{id}` | Busca pessoa por ID | `200 OK` / `404 Not Found` |
| `POST` | `/api/pessoas` | Cadastro de nova pessoa | `201 Created` / `400 Bad Request` / `409 Conflict` |
| `PUT` | `/api/pessoas/{id}` | Atualização completa de dados | `200 OK` / `404 Not Found` / `409 Conflict` |
| `PATCH` | `/api/pessoas/{id}/status` | Ativação/desativação de status | `204 No Content` / `404 Not Found` |
| `DELETE` | `/api/pessoas/{id}` | Remoção de cadastro | `204 No Content` / `404 Not Found` |

---

## 🧪 Testando com o arquivo HTTP

Se você usa VS Code, JetBrains Rider ou Cursor com a extensão *REST Client*, você pode abrir e disparar as requisições prontas diretamente do arquivo [`ProjetoPessoa.http`](./ProjetoPessoa.http).

Exemplo de requisição de criação:
```http
POST http://localhost:5073/api/pessoas
Content-Type: application/json

{
  "nome": "Ana Carolina Oliveira",
  "email": "ana.oliveira@empresa.com",
  "cpf": "345.678.901-22",
  "telefone": "(11) 97123-4567",
  "dataNascimento": "1994-03-25T00:00:00Z"
}
```
