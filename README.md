# 🥾 NZWalks API

<div align="center">

![.NET Version](https://img.shields.io/badge/.NET-8.0-512BD4?style=for-the-badge&logo=dotnet)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)
![API](https://img.shields.io/badge/API-REST-orange?style=for-the-badge)
![Auth](https://img.shields.io/badge/Auth-JWT-red?style=for-the-badge)

**A comprehensive .NET Core Web API for managing walking trails and regions across New Zealand**

[Features](#-features) • [Quick Start](#-quick-start) • [API Documentation](#-api-endpoints) • [Contributing](#-contributing)

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Technical Stack](#-technical-stack)
- [Prerequisites](#-prerequisites)
- [Quick Start](#-quick-start)
- [Data Models](#-data-models)
- [API Endpoints](#-api-endpoints)
- [Authentication](#-authentication)
- [Advanced Querying](#-advanced-querying)
- [Project Structure](#-project-structure)
- [Configuration](#-configuration)
- [Testing](#-testing)
- [Deployment](#-deployment)
- [Best Practices](#-best-practices)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🌏 Overview

The **NZWalks API** is a production-ready RESTful service designed to manage walking trails and regions across New Zealand. Built with modern .NET Core principles, it implements clean architecture, repository patterns, and comprehensive authentication mechanisms.

### Why NZWalks API?

- 🎯 **Production-Ready**: Built with enterprise-grade patterns and practices
- 🔒 **Secure**: JWT authentication with role-based access control
- 📊 **Scalable**: Efficient pagination, filtering, and sorting capabilities
- 📚 **Well-Documented**: Comprehensive Swagger/OpenAPI documentation
- 🧪 **Testable**: Clean architecture with dependency injection

---

## ✨ Features

### 🥾 Walk Management
- ✅ Full CRUD operations (Create, Read, Update, Delete)
- ✅ Advanced filtering by multiple fields
- ✅ Multi-column sorting (ascending/descending)
- ✅ Pagination for large datasets
- ✅ Image URL support for walks
- ✅ Difficulty level categorization
- ✅ Regional association and navigation

### 🌄 Region Management
- ✅ Complete CRUD operations
- ✅ Region code and naming system
- ✅ Image URL support
- ✅ Related walks navigation
- ✅ Unique constraint validation

### 🔐 Authentication & Authorization
- ✅ ASP.NET Core Identity integration
- ✅ JWT Bearer token authentication
- ✅ Role-based access control (Reader/Writer)
- ✅ Secure password hashing
- ✅ Token expiration and refresh
- ✅ User registration and login endpoints

### ⚙️ Advanced Querying
- ✅ Dynamic filtering via query parameters
- ✅ Multi-field sorting
- ✅ Cursor-based pagination
- ✅ Optimized database queries with EF Core
- ✅ Query result caching

---

## 🧱 Technical Stack

| Layer | Technology | Purpose |
|-------|-------------|---------|
| **Framework** | .NET 8 (ASP.NET Core Web API) | Core application framework |
| **ORM** | Entity Framework Core 8.0 | Database abstraction and migrations |
| **Database** | SQL Server | Primary data storage |
| **Authentication** | ASP.NET Core Identity + JWT | User management and auth |
| **Documentation** | Swagger / OpenAPI 3.0 | Interactive API documentation |
| **Architecture** | Repository Pattern | Data access abstraction |
| **DI Container** | Built-in .NET DI | Dependency injection |
| **Mapping** | AutoMapper | DTO to Entity mapping |
| **Validation** | FluentValidation | Input validation |
| **Logging** | Serilog | Structured logging |

---

## 📦 Prerequisites

Before you begin, ensure you have the following installed:

- ✅ [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0) or higher
- ✅ [SQL Server 2019+](https://www.microsoft.com/sql-server/sql-server-downloads) or SQL Server Express
- ✅ [Visual Studio 2022](https://visualstudio.microsoft.com/) / [VS Code](https://code.visualstudio.com/) / [Rider](https://www.jetbrains.com/rider/)
- ✅ [Postman](https://www.postman.com/) (optional, for API testing)
- ✅ [Git](https://git-scm.com/)

### Verify Installation

```bash
dotnet --version  # Should show 8.0.x or higher
```

---

## 🚀 Quick Start

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/yourusername/NZWalksAPI.git
cd NZWalksAPI
```

### 2️⃣ Configure Database Connection

Edit `appsettings.json` and update the connection string:

```json
{
  "ConnectionStrings": {
    "NZWalksConnectionString": "Server=localhost;Database=NZWalksDb;Trusted_Connection=True;TrustServerCertificate=True;"
  },
  "Jwt": {
    "Key": "YourSuperSecretKeyHere_MinimumLength32Characters!",
    "Issuer": "https://localhost:7001",
    "Audience": "https://localhost:7001"
  }
}
```

> **⚠️ Security Note**: Never commit sensitive keys to source control. Use User Secrets or Azure Key Vault in production.

### 3️⃣ Install Dependencies

```bash
dotnet restore
```

### 4️⃣ Apply Database Migrations

```bash
# Create initial migration (if not exists)
dotnet ef migrations add InitialCreate

# Update database
dotnet ef database update
```

### 5️⃣ Seed Initial Data (Optional)

Run the application once to seed default roles and sample data:

```bash
dotnet run --seed
```

### 6️⃣ Run the Application

```bash
dotnet run
```

or with hot reload:

```bash
dotnet watch run
```

### 7️⃣ Access the API

- **API Base URL**: `https://localhost:7001`
- **Swagger UI**: `https://localhost:7001/swagger`
- **Health Check**: `https://localhost:7001/health`

---

## 🗂️ Data Models

### **Walk Entity**

Represents a walking trail with detailed information.

| Field | Type | Constraints | Description |
|--------|------|-------------|-------------|
| `Id` | `Guid` | Primary Key | Unique identifier |
| `Name` | `string` | Required, MaxLength(100) | Walk name |
| `Description` | `string` | Required, MaxLength(1000) | Detailed description |
| `LengthInKm` | `double` | Required, Range(0.1, 1000) | Length in kilometers |
| `WalkImageUrl` | `string?` | Optional, URL format | Image URL |
| `DifficultyId` | `Guid` | Foreign Key | Difficulty level reference |
| `RegionId` | `Guid` | Foreign Key | Associated region |
| `CreatedAt` | `DateTime` | Auto-generated | Creation timestamp |
| `UpdatedAt` | `DateTime?` | Auto-updated | Last update timestamp |

### **Region Entity**

Represents a geographical region in New Zealand.

| Field | Type | Constraints | Description |
|--------|------|-------------|-------------|
| `Id` | `Guid` | Primary Key | Unique identifier |
| `Code` | `string` | Required, Unique, MaxLength(10) | Region code (e.g., "AKL") |
| `Name` | `string` | Required, MaxLength(100) | Region name |
| `RegionImageUrl` | `string?` | Optional, URL format | Image URL |
| `Walks` | `ICollection<Walk>` | Navigation | Related walks |

### **Difficulty Entity**

Categorizes walk difficulty levels.

| Field | Type | Constraints | Description |
|--------|------|-------------|-------------|
| `Id` | `Guid` | Primary Key | Unique identifier |
| `Name` | `string` | Required, Unique | Difficulty level (Easy/Medium/Hard) |

### **Entity Relationship Diagram**

```
┌─────────────┐          ┌──────────────┐          ┌─────────────┐
│   Region    │          │     Walk     │          │ Difficulty  │
├─────────────┤          ├──────────────┤          ├─────────────┤
│ Id (PK)     │◄────────┤│ Id (PK)      │          │ Id (PK)     │
│ Code        │ 1      * │ Name         │ *      1 │ Name        │
│ Name        │          │ Description  │├─────────┤│             │
│ ImageUrl    │          │ LengthInKm   │          │             │
│             │          │ ImageUrl     │          │             │
└─────────────┘          │ RegionId(FK) │          └─────────────┘
                         │ DifficultyId │
                         │ CreatedAt    │
                         │ UpdatedAt    │
                         └──────────────┘
```

---

## 🔌 API Endpoints

### **Walks API**

#### Get All Walks
```http
GET /api/walks
```

**Query Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `filterOn` | string | - | Field to filter (Name, Difficulty, LengthInKm) |
| `filterQuery` | string | - | Filter value |
| `sortBy` | string | Name | Sort field |
| `isAscending` | boolean | true | Sort direction |
| `pageNumber` | int | 1 | Page number (min: 1) |
| `pageSize` | int | 10 | Results per page (max: 100) |

**Example Request:**
```bash
curl -X GET "https://localhost:7001/api/walks?filterOn=Difficulty&filterQuery=Easy&sortBy=LengthInKm&isAscending=true&pageNumber=1&pageSize=10" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

**Response (200 OK):**
```json
{
  "data": [
    {
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "Hooker Valley Track",
      "description": "Easy walk to Hooker Lake with stunning mountain views",
      "lengthInKm": 10.0,
      "walkImageUrl": "https://example.com/hooker-valley.jpg",
      "difficulty": {
        "id": "difficulty-guid",
        "name": "Easy"
      },
      "region": {
        "id": "region-guid",
        "code": "CAN",
        "name": "Canterbury",
        "regionImageUrl": "https://example.com/canterbury.jpg"
      }
    }
  ],
  "pageNumber": 1,
  "pageSize": 10,
  "totalRecords": 45,
  "totalPages": 5
}
```

#### Get Walk by ID
```http
GET /api/walks/{id}
```

**Example:**
```bash
curl -X GET "https://localhost:7001/api/walks/3fa85f64-5717-4562-b3fc-2c963f66afa6" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

#### Create Walk
```http
POST /api/walks
```

**Authorization Required:** Writer role

**Request Body:**
```json
{
  "name": "Milford Track",
  "description": "One of New Zealand's Great Walks through Fiordland National Park",
  "lengthInKm": 53.5,
  "walkImageUrl": "https://example.com/milford-track.jpg",
  "difficultyId": "difficulty-guid",
  "regionId": "region-guid"
}
```

**Example:**
```bash
curl -X POST "https://localhost:7001/api/walks" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Milford Track",
    "description": "Scenic walk through Fiordland",
    "lengthInKm": 53.5,
    "walkImageUrl": "https://example.com/milford.jpg",
    "difficultyId": "guid-here",
    "regionId": "guid-here"
  }'
```

#### Update Walk
```http
PUT /api/walks/{id}
```

**Authorization Required:** Writer role

#### Delete Walk
```http
DELETE /api/walks/{id}
```

**Authorization Required:** Writer role

---

### **Regions API**

#### Get All Regions
```http
GET /api/regions
```

**Example:**
```bash
curl -X GET "https://localhost:7001/api/regions" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

**Response:**
```json
[
  {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "code": "AKL",
    "name": "Auckland",
    "regionImageUrl": "https://example.com/auckland.jpg"
  },
  {
    "id": "4fb96f75-6828-5673-c4gd-3d074g77bgb7",
    "code": "WLG",
    "name": "Wellington",
    "regionImageUrl": "https://example.com/wellington.jpg"
  }
]
```

#### Get Region by ID
```http
GET /api/regions/{id}
```

#### Create Region
```http
POST /api/regions
```

**Authorization Required:** Writer role

**Request Body:**
```json
{
  "code": "OTA",
  "name": "Otago",
  "regionImageUrl": "https://example.com/otago.jpg"
}
```

#### Update Region
```http
PUT /api/regions/{id}
```

**Authorization Required:** Writer role

#### Delete Region
```http
DELETE /api/regions/{id}
```

**Authorization Required:** Writer role

---

## 🔐 Authentication

### Register New User

```http
POST /api/auth/register
```

**Request Body:**
```json
{
  "username": "john.doe",
  "email": "john.doe@example.com",
  "password": "SecurePass123!",
  "roles": ["Reader"]
}
```

**Response (200 OK):**
```json
{
  "message": "User registered successfully"
}
```

### Login

```http
POST /api/auth/login
```

**Request Body:**
```json
{
  "username": "john.doe",
  "password": "SecurePass123!"
}
```

**Response (200 OK):**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expiration": "2025-11-04T10:30:00Z",
  "username": "john.doe",
  "roles": ["Reader"]
}
```

### Using JWT Token

Include the token in the Authorization header for all protected endpoints:

```bash
curl -X GET "https://localhost:7001/api/walks" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN_HERE"
```

### Available Roles

| Role | Permissions |
|------|-------------|
| **Reader** | GET operations on all endpoints |
| **Writer** | Full CRUD operations (includes Reader permissions) |

---

## 🔍 Advanced Querying

### Filtering

Filter walks by specific fields:

```bash
# Filter by difficulty
GET /api/walks?filterOn=Difficulty&filterQuery=Hard

# Filter by name (case-insensitive)
GET /api/walks?filterOn=Name&filterQuery=track

# Filter by minimum length
GET /api/walks?filterOn=LengthInKm&filterQuery=20
```

### Sorting

Sort results by any field:

```bash
# Sort by length ascending
GET /api/walks?sortBy=LengthInKm&isAscending=true

# Sort by name descending
GET /api/walks?sortBy=Name&isAscending=false
```

### Pagination

Paginate large result sets:

```bash
# Get page 2 with 20 items per page
GET /api/walks?pageNumber=2&pageSize=20
```

### Combining Operations

Combine filtering, sorting, and pagination:

```bash
GET /api/walks?filterOn=Difficulty&filterQuery=Medium&sortBy=LengthInKm&isAscending=false&pageNumber=1&pageSize=5
```

---

## 📁 Project Structure

```
NZWalksAPI/
├── 📂 Controllers/
│   ├── WalksController.cs           # Walk endpoints
│   ├── RegionsController.cs         # Region endpoints
│   ├── AuthController.cs            # Authentication endpoints
│   └── DifficultyController.cs      # Difficulty endpoints
│
├── 📂 Models/
│   ├── 📂 Domain/                   # Entity models
│   │   ├── Walk.cs
│   │   ├── Region.cs
│   │   ├── Difficulty.cs
│   │   └── ApplicationUser.cs
│   │
│   └── 📂 DTO/                      # Data Transfer Objects
│       ├── WalkDto.cs
│       ├── CreateWalkDto.cs
│       ├── UpdateWalkDto.cs
│       ├── RegionDto.cs
│       └── AuthDto.cs
│
├── 📂 Repositories/
│   ├── 📂 Interfaces/
│   │   ├── IWalkRepository.cs
│   │   ├── IRegionRepository.cs
│   │   └── ITokenRepository.cs
│   │
│   └── 📂 Implementation/
│       ├── WalkRepository.cs
│       ├── RegionRepository.cs
│       └── TokenRepository.cs
│
├── 📂 Data/
│   ├── NZWalksDbContext.cs          # EF Core DbContext
│   ├── AuthDbContext.cs             # Identity DbContext
│   └── 📂 Migrations/               # Database migrations
│
├── 📂 Mappings/
│   └── AutoMapperProfiles.cs        # AutoMapper configuration
│
├── 📂 Middleware/
│   ├── ExceptionHandlerMiddleware.cs
│   └── LoggingMiddleware.cs
│
├── 📂 Validators/
│   ├── CreateWalkValidator.cs
│   └── UpdateWalkValidator.cs
│
├── appsettings.json                 # Configuration
├── appsettings.Development.json     # Dev configuration
└── Program.cs                       # Application entry point
```

---

## ⚙️ Configuration

### User Secrets (Development)

Store sensitive data securely during development:

```bash
# Initialize user secrets
dotnet user-secrets init

# Set JWT key
dotnet user-secrets set "Jwt:Key" "YourSuperSecretKey_MinLength32Chars!"

# Set connection string
dotnet user-secrets set "ConnectionStrings:NZWalksConnectionString" "YourConnectionString"
```

### Environment Variables (Production)

Set environment variables in production:

```bash
# Linux/Mac
export Jwt__Key="YourProductionKey"
export ConnectionStrings__NZWalksConnectionString="YourProductionConnectionString"

# Windows
set Jwt__Key=YourProductionKey
set ConnectionStrings__NZWalksConnectionString=YourProductionConnectionString
```

### Azure App Settings

For Azure deployments, configure in the Azure Portal:
- Navigate to your App Service
- Go to Configuration → Application Settings
- Add your connection strings and app settings

---

## 🧪 Testing

### Unit Tests

```bash
dotnet test NZWalks.Tests.Unit
```

### Integration Tests

```bash
dotnet test NZWalks.Tests.Integration
```

### API Testing with Postman

Import the provided Postman collection:

1. Open Postman
2. Click Import
3. Select `NZWalks.postman_collection.json`
4. Configure environment variables (base URL, token)

### Sample Test Data

```sql
-- Insert sample region
INSERT INTO Regions (Id, Code, Name, RegionImageUrl)
VALUES (NEWID(), 'TST', 'Test Region', 'https://example.com/test.jpg');

-- Insert sample difficulty
INSERT INTO Difficulties (Id, Name)
VALUES (NEWID(), 'Test');
```

---

## 🚀 Deployment

### Deploy to Azure App Service

```bash
# Login to Azure
az login

# Create resource group
az group create --name NZWalksRG --location eastus

# Create App Service plan
az appservice plan create --name NZWalksPlan --resource-group NZWalksRG --sku B1

# Create Web App
az webapp create --name NZWalksAPI --resource-group NZWalksRG --plan NZWalksPlan

# Deploy code
dotnet publish -c Release
cd bin/Release/net8.0/publish
az webapp deployment source config-zip --resource-group NZWalksRG --name NZWalksAPI --src ../publish.zip
```

### Docker Deployment

```dockerfile
# Dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["NZWalksAPI.csproj", "./"]
RUN dotnet restore
COPY . .
RUN dotnet build -c Release -o /app/build

FROM build AS publish
RUN dotnet publish -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "NZWalksAPI.dll"]
```

Build and run:

```bash
docker build -t nzwalks-api .
docker run -d -p 8080:80 --name nzwalks nzwalks-api
```

---

## 💡 Best Practices

### ✅ DO's

- ✅ Always use DTOs for API input/output
- ✅ Implement proper validation using FluentValidation
- ✅ Use async/await for all database operations
- ✅ Apply authorization attributes to protected endpoints
- ✅ Log important operations and errors
- ✅ Use dependency injection for loose coupling
- ✅ Implement proper exception handling
- ✅ Version your API (`/api/v1/walks`)
- ✅ Document all endpoints with XML comments
- ✅ Use meaningful HTTP status codes

### ❌ DON'Ts

- ❌ Don't expose domain entities directly in APIs
- ❌ Don't hardcode connection strings or secrets
- ❌ Don't use synchronous database operations
- ❌ Don't return sensitive data in error messages
- ❌ Don't skip input validation
- ❌ Don't use HTTP GET for operations with side effects
- ❌ Don't ignore security headers
- ❌ Don't skip database migrations

### Security Checklist

- [ ] JWT secrets stored securely (not in source code)
- [ ] HTTPS enforced in production
- [ ] CORS configured properly
- [ ] SQL injection prevention (parameterized queries)
- [ ] Rate limiting implemented
- [ ] Authentication required on protected endpoints
- [ ] Input validation on all endpoints
- [ ] Sensitive data not logged

---

## 🤝 Contributing

We welcome contributions! Here's how you can help:

### 1. Fork the Repository

Click the "Fork" button at the top right of this page.

### 2. Clone Your Fork

```bash
git clone https://github.com/YOUR_USERNAME/NZWalksAPI.git
cd NZWalksAPI
```

### 3. Create a Feature Branch

```bash
git checkout -b feature/amazing-feature
```

### 4. Make Your Changes

- Write clean, readable code
- Follow existing code style
- Add unit tests for new features
- Update documentation as needed

### 5. Commit Your Changes

```bash
git add .
git commit -m "Add amazing feature"
```

Use conventional commit messages:
- `feat:` New feature
- `fix:` Bug fix
- `docs:` Documentation changes
- `refactor:` Code refactoring
- `test:` Adding tests
- `chore:` Maintenance tasks

### 6. Push and Create Pull Request

```bash
git push origin feature/amazing-feature
```

Then create a Pull Request on GitHub with a clear description of your changes.

### Development Guidelines

- Follow C# coding conventions
- Write meaningful commit messages
- Add XML documentation comments
- Ensure all tests pass
- Update README if needed

---

## 📊 API Performance Tips

- Use pagination for large datasets
- Implement caching for frequently accessed data
- Use projection (select specific fields) to reduce payload
- Enable response compression
- Monitor database query performance
- Use connection pooling

---

## 🐛 Troubleshooting

### Common Issues

#### Database Connection Failed
```bash
# Verify SQL Server is running
# Check connection string in appsettings.json
# Ensure database exists: dotnet ef database update
```

#### JWT Authentication Failed
```bash
# Verify token is included in Authorization header
# Check token hasn't expired
# Ensure JWT key in appsettings matches token generation
```

#### Migration Errors
```bash
# Remove last migration
dotnet ef migrations remove

# Reset database (WARNING: deletes all data)
dotnet ef database drop
dotnet ef database update
```

---

## 📞 Support

- 📧 Email: support@nzwalks.com
- 💬 Discord: [Join our community](https://discord.gg/nzwalks)
- 🐛 Issues: [GitHub Issues](https://github.com/yourusername/NZWalksAPI/issues)
- 📖 Documentation: [Wiki](https://github.com/yourusername/NZWalksAPI/wiki)

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2025 NZWalks Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
```

---

## 🎯 Roadmap

### Version 1.0 (Current)
- ✅ Core CRUD operations
- ✅ JWT authentication
- ✅ Basic filtering and sorting
- ✅ Swagger documentation

### Version 1.1 (Q2 2025)
- 🔄 Image upload to Azure Blob Storage
- 🔄 Redis caching implementation
- 🔄 Rate limiting middleware
- 🔄 Advanced search capabilities

### Version 2.0 (Q3 2025)
- 📋 GraphQL support
- 📋 Multi-language region support
- 📋 Weather API integration
- 📋 Mobile app companion API
- 📋 Real-time notifications (SignalR)
- 📋 Offline support capabilities

### Version 3.0 (Q4 2025)
- 📋 Machine learning walk recommendations
- 📋 Social features (reviews, ratings)
- 📋 Route planning and navigation
- 📋 Elevation profile data
- 📋 Trail condition reporting

---

## 🙏 Acknowledgments

- **New Zealand Department of Conservation** - Trail data inspiration
- **ASP.NET Core Team** - Excellent framework
- **Community Contributors** - Thank you for your support!

---

## 📈 Project Stats

![GitHub stars](https://img.shields.io/github/stars/yourusername/NZWalksAPI?style=social)
![GitHub forks](https://img.shields.io/github/forks/yourusername/NZWalksAPI?style=social)
![GitHub issues](https://img.shields.io/github/issues/yourusername/NZWalksAPI)
![GitHub pull requests](https://img.shields.io/github/issues-pr/yourusername/NZWalksAPI)

---

<div align="center">

**Made with ❤️ by the Muhammad Junaid**

[⬆ Back to Top](#-nzwalks-api)

</div>
