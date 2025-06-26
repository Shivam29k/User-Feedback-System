# User Feedback System

A full-stack user feedback system built with React (frontend) and Node.js/Express (backend) that allows users to submit feedback and administrators to view and manage feedback through a dashboard.

## Features

### Frontend Features
- **Feedback Submission Form**: Clean, responsive form for users to submit feedback
- **Category Selection**: Support for different feedback types (Bug Report, Feature Request, Suggestion, General)
- **Feedback Dashboard**: Administrative interface to view and manage all feedback
- **Advanced Filtering**: Filter by category, status, and search functionality
- **Sorting Options**: Sort feedback by date (newest/oldest) or category
- **Responsive Design**: Mobile-friendly interface built with Tailwind CSS
- **Real-time Statistics**: Visual stats showing feedback breakdown by category

### Backend Features (To Be Implemented)
- **RESTful API**: Express.js server with proper REST endpoints
- **Database Integration**: MongoDB or PostgreSQL for data persistence
- **Data Validation**: Server-side validation for all inputs
- **Error Handling**: Comprehensive error handling and logging
- **CORS Support**: Cross-origin resource sharing configuration

## Technology Stack

### Frontend
- **React 19** with TypeScript
- **React Router** for navigation
- **Tailwind CSS** for styling
- **Lucide React** for icons
- **Vite** for build tooling

### Backend (To Be Implemented)
- **Node.js** with Express.js
- **MongoDB** or **PostgreSQL** for database
- **Joi** or **Yup** for validation
- **CORS** for cross-origin requests
- **Morgan** for logging

## API Specifications

The following APIs need to be implemented in the backend:

### Base URL
```
http://localhost:5000/api
```

### Endpoints

#### 1. Submit Feedback
**POST** `/feedback`

Submit new user feedback.

**Request Body:**
```json
{
  "userName": "string (required, 1-100 characters)",
  "email": "string (required, valid email format)",
  "feedbackText": "string (required, 10-1000 characters)",
  "category": "string (required, enum: ['suggestion', 'bug-report', 'feature-request', 'general'])"
}
```

**Response (201 Created):**
```json
{
  "success": true,
  "message": "Feedback submitted successfully",
  "data": {
    "id": "string",
    "userName": "string",
    "email": "string",
    "feedbackText": "string",
    "category": "string",
    "timestamp": "ISO 8601 date string",
    "status": "pending"
  }
}
```

**Error Response (400 Bad Request):**
```json
{
  "success": false,
  "message": "Validation error",
  "errors": [
    {
      "field": "string",
      "message": "string"
    }
  ]
}
```

#### 2. Get All Feedback
**GET** `/feedback`

Retrieve all feedback with optional filtering and sorting.

**Query Parameters:**
- `category` (optional): Filter by category (`suggestion`, `bug-report`, `feature-request`, `general`)
- `status` (optional): Filter by status (`pending`, `reviewed`, `resolved`)
- `sortBy` (optional): Sort order (`newest`, `oldest`, `category`)
- `search` (optional): Search in userName, email, or feedbackText
- `page` (optional): Page number for pagination (default: 1)
- `limit` (optional): Items per page (default: 10, max: 100)

**Example Request:**
```
GET /feedback?category=bug-report&status=pending&sortBy=newest&page=1&limit=10
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "feedback": [
      {
        "id": "string",
        "userName": "string",
        "email": "string",
        "feedbackText": "string",
        "category": "string",
        "timestamp": "ISO 8601 date string",
        "status": "string"
      }
    ],
    "pagination": {
      "currentPage": 1,
      "totalPages": 5,
      "totalItems": 50,
      "itemsPerPage": 10
    },
    "stats": {
      "total": 50,
      "byCategory": {
        "suggestion": 15,
        "bug-report": 20,
        "feature-request": 10,
        "general": 5
      },
      "byStatus": {
        "pending": 30,
        "reviewed": 15,
        "resolved": 5
      }
    }
  }
}
```

#### 3. Get Feedback by ID
**GET** `/feedback/:id`

Retrieve a specific feedback item by ID.

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "string",
    "userName": "string",
    "email": "string",
    "feedbackText": "string",
    "category": "string",
    "timestamp": "ISO 8601 date string",
    "status": "string"
  }
}
```

**Error Response (404 Not Found):**
```json
{
  "success": false,
  "message": "Feedback not found"
}
```

#### 4. Update Feedback Status (Admin Only)
**PUT** `/feedback/:id/status`

Update the status of a feedback item.

**Request Body:**
```json
{
  "status": "string (required, enum: ['pending', 'reviewed', 'resolved'])"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Feedback status updated successfully",
  "data": {
    "id": "string",
    "status": "string"
  }
}
```

#### 5. Delete Feedback (Admin Only)
**DELETE** `/feedback/:id`

Delete a specific feedback item.

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Feedback deleted successfully"
}
```

#### 6. Get Feedback Statistics
**GET** `/feedback/stats`

Get aggregated statistics about feedback.

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "total": 100,
    "byCategory": {
      "suggestion": 30,
      "bug-report": 40,
      "feature-request": 20,
      "general": 10
    },
    "byStatus": {
      "pending": 60,
      "reviewed": 30,
      "resolved": 10
    },
    "recentActivity": {
      "today": 5,
      "thisWeek": 25,
      "thisMonth": 80
    }
  }
}
```

## Database Schema

### Feedback Collection/Table

```javascript
{
  _id: ObjectId, // MongoDB ID or auto-increment ID for PostgreSQL
  userName: String, // Required, 1-100 characters
  email: String, // Required, valid email format
  feedbackText: String, // Required, 10-1000 characters
  category: String, // Enum: ['suggestion', 'bug-report', 'feature-request', 'general']
  status: String, // Enum: ['pending', 'reviewed', 'resolved'], default: 'pending'
  timestamp: Date, // Auto-generated timestamp
  createdAt: Date, // Auto-generated
  updatedAt: Date // Auto-generated
}
```

### Indexes
- `category` - For filtering by category
- `status` - For filtering by status
- `timestamp` - For sorting by date
- `email` - For searching by email

## Installation & Setup

### Frontend
```bash
cd frontend
npm install
npm run dev
```

### Backend (To Be Implemented)
```bash
cd backend
npm install
npm run dev
```

## Environment Variables

### Backend Environment Variables (To Be Created)
```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/feedback-system
# OR for PostgreSQL
DATABASE_URL=postgresql://username:password@localhost:5432/feedback_system
NODE_ENV=development
CORS_ORIGIN=http://localhost:5173
```

## Development Guidelines

### Error Handling
- All API responses should follow the standard format with `success`, `message`, and `data` fields
- Validation errors should return specific field-level error messages
- Server errors should be logged and return generic error messages to clients

### Security Considerations
- Input validation and sanitization
- Rate limiting for API endpoints
- CORS configuration
- SQL injection prevention (for PostgreSQL)
- NoSQL injection prevention (for MongoDB)

### Performance Optimization
- Database indexing for frequently queried fields
- Pagination for large datasets
- Caching for statistics endpoints
- Connection pooling for database connections

## Future Enhancements

- User authentication and authorization
- Email notifications for feedback status updates
- File attachment support
- Feedback categories management
- Advanced analytics and reporting
- Export functionality (CSV, PDF)
- Real-time updates using WebSockets
- Mobile app support

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Submit a pull request

## License

This project is licensed under the MIT License.
