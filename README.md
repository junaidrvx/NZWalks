# NZWalks API

A .NET Core Web API for managing walks and trails across different regions in New Zealand.

## Features

- **Walk Management**
  - Create, Read, Update, and Delete walks
  - Filter walks based on various criteria
  - Sorting and pagination support
  - Image URL support for walks

- **Region Management**
  - Manage different regions of New Zealand
  - Region details including code, name, and image URL

- **Authentication & Authorization**
  - Secure API endpoints using ASP.NET Core Identity
  - Role-based access control (Reader/Writer roles)

- **Advanced Querying**
  - Filtering support using multiple parameters
  - Sorting capabilities
  - Pagination for better data handling

## Technical Stack

- **Framework**: ASP.NET Core
- **Database**: Entity Framework Core with SQL Server
- **Authentication**: ASP.NET Core Identity
- **API Documentation**: Swagger/OpenAPI
- **Architecture**: Repository Pattern
- **Data Transfer**: AutoMapper for DTO mappings

## Data Models

### Walk
- ID (GUID)
- Name
- Description
- Length in Kilometers
- Walk Image URL
- Difficulty Level
- Region Information

### Region
- ID (GUID)
- Code
- Name
- Region Image URL

## API Endpoints

### Walks
- GET `/api/walks` - Get all walks with filtering, sorting, and pagination
- GET `/api/walks/{id}` - Get a specific walk
- POST `/api/walks` - Create a new walk
- PUT `/api/walks/{id}` - Update an existing walk
- DELETE `/api/walks/{id}` - Delete a walk

Query Parameters for GET /api/walks:
- `filterOn`: Field to filter on
- `filterQuery`: Filter value
- `sortBy`: Field to sort by
- `isAscending`: Sort direction
- `pageNumber`: Page number for pagination
- `pageSize`: Number of items per page (default: 1000)

## Setup and Installation

1. Clone the repository
2. Update the connection strings in `appsettings.json`
3. Run Entity Framework migrations:
   ```bash
   dotnet ef database update
   ```
4. Run the application:
   ```bash
   dotnet run
   ```

## Contributing

1. Fork the repository
2. Create a new branch for your feature
3. Commit your changes
4. Push to the branch
5. Create a new Pull Request

## License

This project is open source and available under the [MIT License](LICENSE).
