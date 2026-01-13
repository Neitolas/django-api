# Django REST API - User Management

Uma API RESTful desenvolvida com Django e Django REST Framework para gerenciamento de usuários.

## Sobre o Projeto

Este projeto implementa uma API REST completa para gerenciamento de usuários, permitindo operações CRUD (Create, Read, Update, Delete) através de endpoints HTTP. A aplicação utiliza Django REST Framework para serialização de dados e fornece uma interface robusta para manipulação de informações de usuários.

## Funcionalidades

- **Listar todos os usuários** - GET endpoint para recuperar lista completa de usuários
- **Buscar usuário por nickname** - GET endpoint com parâmetro de rota
- **Criar novo usuário** - POST endpoint para cadastro
- **Atualizar usuário** - PUT endpoint para modificação de dados
- **Deletar usuário** - DELETE endpoint para remoção
- **Filtragem por query parameters** - Busca usuários através de parâmetros de consulta

## Modelo de Dados

### User
- `user_nickname` (CharField, Primary Key) - Nome de usuário único
- `user_name` (CharField) - Nome completo
- `user_email` (EmailField) - E-mail do usuário
- `user_age` (IntegerField) - Idade do usuário

### UserTasks (modelo adicional)
- `user_nickname` (CharField) - Referência ao usuário
- `user_task` (CharField) - Descrição da tarefa

## Tecnologias Utilizadas

- **Django 6.0** - Framework web principal
- **Django REST Framework 3.16.1** - Toolkit para criação de APIs REST
- **Django CORS Headers 4.9.0** - Middleware para configuração de CORS
- **SQLite** - Banco de dados (padrão do Django para desenvolvimento)

## Endpoints da API

### Base URL: `http://localhost:8000/api/`

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | `/api/` | Lista todos os usuários |
| GET | `/api/user/<nickname>` | Busca usuário por nickname |
| GET | `/api/data/?user=<nickname>` | Busca usuário por query parameter |
| POST | `/api/data/` | Cria novo usuário |
| PUT | `/api/user/<nickname>` | Atualiza usuário específico |
| PUT | `/api/data/` | Atualiza usuário (via body) |
| DELETE | `/api/data/` | Deleta usuário |

### Exemplos de Uso

#### Criar Usuário (POST)
```bash
curl -X POST http://localhost:8000/api/data/ \
  -H "Content-Type: application/json" \
  -d '{
    "user_nickname": "john_doe",
    "user_name": "John Doe",
    "user_email": "john@example.com",
    "user_age": 25
  }'
```

#### Listar Todos os Usuários (GET)
```bash
curl http://localhost:8000/api/
```

#### Buscar Usuário por Nickname (GET)
```bash
curl http://localhost:8000/api/user/john_doe
```

#### Atualizar Usuário (PUT)
```bash
curl -X PUT http://localhost:8000/api/user/john_doe \
  -H "Content-Type: application/json" \
  -d '{
    "user_nickname": "john_doe",
    "user_name": "John Doe Updated",
    "user_email": "john.new@example.com",
    "user_age": 26
  }'
```

#### Deletar Usuário (DELETE)
```bash
curl -X DELETE http://localhost:8000/api/data/ \
  -H "Content-Type: application/json" \
  -d '{
    "user_nickname": "john_doe"
  }'
```

## Instalação e Configuração Local

### Pré-requisitos

- Python 3.8 ou superior
- pip (gerenciador de pacotes Python)
- Git

### Passo a Passo

1. **Clone o repositório**
   ```bash
   git clone <url-do-repositorio>
   cd "django api"
   ```

2. **Crie um ambiente virtual** (recomendado)
   ```bash
   # Linux/Mac
   python3 -m venv venv
   source venv/bin/activate

   # Windows
   python -m venv venv
   venv\Scripts\activate
   ```

3. **Instale as dependências**
   ```bash
   pip install -r requirements.txt
   ```

4. **Execute as migrações do banco de dados**
   ```bash
   python manage.py migrate
   ```

5. **Crie um superusuário** (opcional, para acessar o admin)
   ```bash
   python manage.py createsuperuser
   ```

6. **Inicie o servidor de desenvolvimento**
   ```bash
   python manage.py runserver
   ```

7. **Acesse a aplicação**
   - API: `http://localhost:8000/api/`
   - Admin: `http://localhost:8000/admin/` (se criou superusuário)

## Estrutura do Projeto

```
django api/
│
├── api_rest/                 # Aplicação principal da API
│   ├── migrations/          # Migrações do banco de dados
│   ├── __init__.py
│   ├── admin.py            # Configuração do Django Admin
│   ├── apps.py             # Configuração da aplicação
│   ├── models.py           # Modelos de dados (User, UserTasks)
│   ├── serializers.py      # Serializers do DRF
│   ├── tests.py            # Testes unitários
│   ├── urls.py             # Rotas da API
│   └── views.py            # Views/Controllers
│
├── api_root/                # Configurações do projeto Django
│   ├── __init__.py
│   ├── asgi.py            # Configuração ASGI
│   ├── settings.py        # Configurações principais
│   ├── urls.py            # URLs raiz do projeto
│   └── wsgi.py            # Configuração WSGI
│
├── db.sqlite3             # Banco de dados SQLite
├── manage.py              # CLI do Django
├── requirements.txt       # Dependências do projeto
└── README.md             # Este arquivo
```

## Testando a API

Você pode testar a API usando ferramentas como:

- **cURL** (linha de comando)
- **Postman** (interface gráfica)
- **Insomnia** (interface gráfica)
- **HTTPie** (linha de comando)
- **ThunderClient** (extensão VS Code)

### Exemplo com Python Requests

```python
import requests

# Criar usuário
response = requests.post('http://localhost:8000/api/data/', json={
    'user_nickname': 'test_user',
    'user_name': 'Test User',
    'user_email': 'test@example.com',
    'user_age': 30
})
print(response.json())

# Listar usuários
response = requests.get('http://localhost:8000/api/')
print(response.json())
```

## Configurações de Segurança

**IMPORTANTE**: Este projeto está configurado para desenvolvimento local. Antes de fazer deploy em produção:

1. Altere o `SECRET_KEY` em `settings.py`
2. Configure `DEBUG = False`
3. Defina `ALLOWED_HOSTS` apropriadamente
4. Configure um banco de dados robusto (PostgreSQL, MySQL)
5. Utilize variáveis de ambiente para informações sensíveis
6. Configure HTTPS
7. Revise as configurações de CORS

## Contribuindo

Contribuições são bem-vindas! Sinta-se à vontade para:

1. Fazer fork do projeto
2. Criar uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abrir um Pull Request

---
