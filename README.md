# 🤖 Rose AI Chatbot Backend

A sophisticated, production-ready chatbot backend built with FastAPI, featuring advanced user management, persistent chat history, and seamless OpenAI integration. This system provides a complete foundation for building intelligent conversational applications with robust authentication and administrative controls.

## ✨ Key Features

### 🔐 **Advanced Authentication & Authorization**

- JWT-based authentication with secure token management
- Role-based access control (Admin/Student user types)
- Automatic default admin account creation
- Password hashing with bcrypt for security

### 💬 **Intelligent Chat Management**

- Persistent chat sessions with SQLite database
- Message history tracking and retrieval
- Support for multiple concurrent conversations per user
- Optimized message ordering and pagination

### 🧠 **AI Integration**

- Seamless OpenAI API integration using LangChain
- Configurable AI models (GPT-3.5-turbo, GPT-4, etc.)
- Dynamic system prompt management (admin-only)
- Context-aware conversations with message history

### 📚 **Document Processing & Indexing**

- LlamaIndex integration for document understanding
- PDF processing capabilities
- Vector storage for efficient document retrieval
- FAQ index system for quick responses

### 🛡️ **Production-Ready Features**

- Comprehensive error handling and logging
- CORS middleware for cross-origin requests
- Database schema validation and auto-migration
- UTF-8 encoding support for multilingual content
- RESTful API design with OpenAPI documentation

## 🏗️ Architecture Overview

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend UI   │────│   FastAPI API   │────│   OpenAI API    │
│   (Dash/React)  │    │    Backend      │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                │
                                │
                       ┌─────────────────┐
                       │   SQLite DB     │
                       │ (Users, Chats,  │
                       │   Messages)     │
                       └─────────────────┘
```

## 🚀 Quick Start

### Prerequisites

- Python 3.11+
- OpenAI API key
- Git

### Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/miracle5284/dash-fastapi-chatbot-llamaindex.git
   cd dash-fastapi-chatbot-llamaindex/backend
   ```

2. **Create virtual environment**

   ```bash
   python -m venv venv
   # Windows
   .\venv\Scripts\activate
   # Linux/Mac
   source venv/bin/activate
   ```

3. **Install dependencies**

   ```bash
   pip install -r requirements.txt
   ```

4. **Environment Configuration**
   Create a `.env` file in the root directory:

   ```env
   OPENAI_API_KEY=your_openai_api_key_here
   OPENAI_MODEL_NAME=gpt-3.5-turbo
   SECRET_KEY=your_secret_key_for_jwt
   DEFAULT_ADMIN_USERNAME=admin
   DEFAULT_ADMIN_PASSWORD=your_admin_password
   ```

5. **Initialize the database**

   ```bash
   python -m chatbot.runserver
   ```

6. **Start the server**

   ```bash
   # Using uvicorn directly
   uvicorn chatbot.server:app --reload --host 0.0.0.0 --port 8000

   # Or using the PowerShell script
   .\run_chatbot.ps1
   ```

The API will be available at `http://localhost:8000` with interactive documentation at `http://localhost:8000/docs`.

## 📖 API Documentation

### Authentication Endpoints

- `POST /register` - Register new user (student role)
- `POST /login` - User authentication
- `GET /me` - Get current user information

### Chat Management

- `GET /chats` - List all user chats
- `GET /chats/{chat_id}` - Get specific chat with messages
- `POST /get_ai_response` - Send message and get AI response

### Admin Features

- `GET /system-prompt` - Get current system prompt (admin only)
- `PUT /system-prompt` - Update system prompt (admin only)

### Legacy Support

- `POST /generate-response/` - Direct AI response generation

For detailed API documentation with request/response schemas, visit `/docs` after starting the server.

## 🗂️ Project Structure

```
backend/
├── chatbot/                    # Main application package
│   ├── __init__.py
│   ├── server.py              # FastAPI application and routes
│   ├── models.py              # SQLAlchemy database models
│   ├── schemas.py             # Pydantic request/response models
│   ├── database.py            # Database configuration and connection
│   ├── auth.py                # Authentication and authorization
│   ├── chatbot.py             # OpenAI integration logic
│   ├── prompt_manager.py      # System prompt management
│   ├── admin_setup.py         # Default admin creation
│   ├── config.py              # Application configuration
│   ├── utils.py               # Utility functions
│   ├── indexing.py            # Document indexing with LlamaIndex
│   └── documents/             # Document storage
│       └── PDFs/              # PDF files for processing
├── chatbot-ui/                # Frontend UI components
├── requirements.txt           # Python dependencies
├── run_chatbot.ps1           # Windows startup script
├── system_prompt.txt         # Default AI system prompt
├── test_api.py              # API testing script
└── README.md                # This file
```

## 🔧 Configuration

### Environment Variables

| Variable                 | Description                     | Default         |
| ------------------------ | ------------------------------- | --------------- |
| `OPENAI_API_KEY`         | OpenAI API key for AI responses | Required        |
| `OPENAI_MODEL_NAME`      | OpenAI model to use             | `gpt-3.5-turbo` |
| `SECRET_KEY`             | JWT secret key                  | Required        |
| `DEFAULT_ADMIN_USERNAME` | Default admin username          | `admin`         |
| `DEFAULT_ADMIN_PASSWORD` | Default admin password          | Required        |

### Database Configuration

The application uses SQLite by default with the following features:

- Automatic table creation on startup
- Schema validation and migration
- Connection pooling for performance
- Foreign key constraints for data integrity

## 🧪 Testing

Run the included test script to verify API functionality:

```bash
python test_api.py
```

This will test:

- User registration and authentication
- Chat creation and message handling
- AI response generation
- Error handling scenarios

## 🐳 Docker Deployment

Create a `Dockerfile`:

```dockerfile
FROM python:3.11-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .
EXPOSE 8000

CMD ["uvicorn", "chatbot.server:app", "--host", "0.0.0.0", "--port", "8000"]
```

Build and run:

```bash
docker build -t rose-chatbot .
docker run -p 8000:8000 --env-file .env rose-chatbot
```

## 🔒 Security Features

- **Password Security**: Bcrypt hashing with salt
- **JWT Tokens**: Secure authentication with expiration
- **Input Validation**: Pydantic models for request validation
- **SQL Injection Protection**: SQLAlchemy ORM with parameterized queries
- **CORS Configuration**: Controlled cross-origin access
- **Role-Based Access**: Admin/Student permission system

## 📈 Performance Optimizations

- **Database Indexing**: Optimized queries with proper indexes
- **Connection Pooling**: Efficient database connection management
- **Async Operations**: FastAPI async support for better concurrency
- **Message Pagination**: Efficient chat history retrieval
- **Caching**: LlamaIndex vector storage for fast document retrieval

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support & Troubleshooting

### Common Issues

**Unicode Decode Error**: Ensure all text files use UTF-8 encoding

```bash
# Check file encoding
file -bi system_prompt.txt
```

**Database Connection Issues**: Verify SQLite permissions and file access

**OpenAI API Errors**: Check API key validity and rate limits

**Authentication Failures**: Verify JWT secret key configuration

### Getting Help

- Check the [API Documentation](API_DOCUMENTATION.md) for detailed endpoint information
- Review the `/docs` endpoint for interactive API testing
- Open an issue on GitHub for bug reports and feature requests

---

**Built with ❤️ using FastAPI, LangChain, and LlamaIndex**
