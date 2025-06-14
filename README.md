# ALX Travel App 0x00 - Database Modeling and Data Seeding

A Django-based travel booking application with comprehensive database models, serializers, and data seeding functionality.

## Project Overview

This project implements database modeling and data seeding for a travel booking application. It includes three main models (Listing, Booking, Review), their corresponding serializers for API data representation, and a management command for populating the database with sample data.

## Features Implemented

### 1. Database Models
- **Listing Model**: Travel property listings with host information, pricing, and availability
- **Booking Model**: Booking management with date validation and status tracking  
- **Review Model**: User reviews with ratings and comments

### 2. API Serializers
- Complete serializers for all models with nested relationships
- Separate create serializers with validation
- Calculated fields like average ratings and total nights

### 3. Data Seeding
- Management command to populate database with realistic sample data
- Configurable data generation (users, listings, bookings, reviews)
- Data integrity validation and relationship management
- **Booking System**: Book listings with date validation and conflict checking
- **Review System**: Leave reviews for completed bookings
- **User Management**: User authentication and profile management
- **Admin Interface**: Django admin for managing all data

## Models

### Listing
- Unique identifier (UUID)
- Host (User relationship)
- Basic information (title, description, category, location)
- Pricing and capacity (price_per_night, max_guests)
- Availability dates
- Timestamps

### Booking
- Unique identifier (UUID)
- Listing and User relationships
- Booking details (check-in/out dates, guests, total_price)
- Status tracking (pending, confirmed, cancelled, completed)
- Automatic price calculation
- Date validation constraints

### Review
- Unique identifier (UUID)
- Listing, User, and optional Booking relationships
- Rating (1-5 scale) and comment
- Unique constraint per user-listing pair

## API Serializers

- **ListingSerializer**: Full listing data with reviews and ratings
- **ListingCreateSerializer**: Simplified serializer for creating listings
- **BookingSerializer**: Full booking data with calculated fields
- **BookingCreateSerializer**: Booking creation with validation
- **ReviewSerializer**: Review data with user information
- **ReviewCreateSerializer**: Review creation with validation

## Setup Instructions

### Prerequisites
- Python 3.8+
- MySQL
- RabbitMQ (for Celery)

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd alx_travel_app_0x00
   ```

2. **Create virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Environment Setup**
   Create a `.env` file in the project root:
   ```env
   SECRET_KEY=your-secret-key-here
   DEBUG=True
   DATABASE_URL=mysql://username:password@localhost:3306/alx_travel_db
   CELERY_BROKER_URL=amqp://guest:guest@localhost:5672//
   ```

5. **Database Setup**
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

6. **Create Superuser**
   ```bash
   python manage.py createsuperuser
   ```

7. **Seed Database (Optional)**
   ```bash
   python manage.py seed --listings 20 --users 10 --bookings 30 --reviews 25
   ```

8. **Run Development Server**
   ```bash
   python manage.py runserver
   ```

## Management Commands

### Seed Command
The `seed` management command populates the database with sample data:

```bash
python manage.py seed [options]
```

**Options:**
- `--listings N`: Number of listings to create (default: 20)
- `--users N`: Number of users to create (default: 10)
- `--bookings N`: Number of bookings to create (default: 30)
- `--reviews N`: Number of reviews to create (default: 25)
- `--clear`: Clear existing data before seeding

**Examples:**
```bash
# Seed with default values
python manage.py seed

# Seed with custom values
python manage.py seed --listings 50 --users 20 --bookings 100 --reviews 80

# Clear existing data and reseed
python manage.py seed --clear --listings 10 --users 5
```

## API Endpoints

The application provides RESTful API endpoints for:
- Listings CRUD operations
- Booking management
- Review system
- User authentication

## Admin Interface

Access the Django admin interface at `/admin/` to manage:
- Users
- Listings
- Bookings
- Reviews

## Database Schema

### Key Relationships
- **User → Listings**: One-to-many (host relationship)
- **User → Bookings**: One-to-many (guest relationship)
- **User → Reviews**: One-to-many (reviewer relationship)
- **Listing → Bookings**: One-to-many
- **Listing → Reviews**: One-to-many
- **Booking → Review**: One-to-one (optional)

### Constraints
- Check-out date must be after check-in date
- Booking guests cannot exceed listing capacity
- Users can only review each listing once
- Booking date conflicts are prevented

## Data Validation

- **Listings**: Date validation, capacity constraints
- **Bookings**: Date validation, availability checking, conflict detection
- **Reviews**: Rating range validation (1-5), uniqueness constraints

## File Structure

```
alx_travel_app_0x00/
├── manage.py
├── requirements.txt
├── README.md
├── alx_travel_app/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── wsgi.py
│   └── asgi.py
└── listings/
    ├── __init__.py
    ├── models.py
    ├── serializers.py
    ├── admin.py
    ├── views.py
    ├── apps.py
    ├── tests.py
    ├── migrations/
    └── management/
        └── commands/
            └── seed.py
```

## Development Notes

- Uses UUID primary keys for security
- Implements proper model relationships and constraints
- Includes comprehensive validation in serializers
- Provides flexible seeding for development and testing
- Uses Django best practices for model design

## Testing

Run tests with:
```bash
python manage.py test
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

This project is part of the ALX Software Engineering program.
