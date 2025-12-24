# Uber Assignment API 🚗

A comprehensive RESTful API for managing ride assignments, driver locations, and ride requests similar to Uber. Built with **Java**, this API handles real-time matching of riders with drivers and manages the entire ride lifecycle.

## 📋 Overview

The Uber Assignment API is a backend service that simulates Uber's core functionality for assigning rides to drivers, tracking driver locations, managing ride requests, and calculating fares. This system demonstrates real-world distributed systems concepts and ride-sharing logistics.

## ✨ Key Features

🚗 **Driver Management** - Register, track, and manage driver profiles and locations

📍 **Location Tracking** - Real-time GPS tracking of driver locations

📞 **Ride Requests** - Create, accept, and complete ride requests

🎯 **Smart Matching** - Intelligent algorithm to match riders with nearest available drivers

💰 **Fare Calculation** - Calculate fares based on distance and time

⭐ **Rating System** - Driver and rider ratings after ride completion

🗺️ **Route Optimization** - Find optimal routes between pickup and dropoff points

📊 **Analytics** - Track rides, earnings, and performance metrics

## 🛠 Technology Stack

- **Language**: Java (Version 11+)
- **Framework**: Spring Boot / Java Servlets
- **Database**: MySQL / PostgreSQL (SQL database)
- **Build Tool**: Maven
- **API Architecture**: RESTful API
- **Location Services**: Google Maps API / Haversine formula
- **Testing**: JUnit, Mockito

## 📁 Project Structure

```
Uber-Assignment-API/
├── src/main/java/
│   └── com/uber/
│       ├── controller/          # API endpoints
│       ├── service/             # Business logic
│       ├── repository/          # Database access
│       ├── model/               # Data models
│       ├── dto/                 # Data Transfer Objects
│       ├── exception/           # Custom exceptions
│       ├── util/                # Utility classes
│       └── config/              # Configuration classes
├── src/main/resources/
│   └── application.properties    # Configuration
├── src/test/java/               # Unit tests
├── pom.xml                       # Maven dependencies
└── README.md
```

## 🚀 Installation & Setup

### Prerequisites

- Java 11 or higher
- Maven 3.6+
- MySQL 8.0+ or PostgreSQL 12+
- Git

### Step-by-Step Setup

1. **Clone the Repository**
   ```bash
   git clone https://github.com/Ashish961169/Uber-Assignment-API.git
   cd Uber-Assignment-API
   ```

2. **Create Database**
   ```sql
   CREATE DATABASE uber_assignment;
   ```

3. **Configure Database Connection**
   Edit `src/main/resources/application.properties`:
   ```properties
   spring.datasource.url=jdbc:mysql://localhost:3306/uber_assignment
   spring.datasource.username=root
   spring.datasource.password=password
   spring.jpa.hibernate.ddl-auto=update
   ```

4. **Build the Project**
   ```bash
   mvn clean install
   ```

5. **Run the Application**
   ```bash
   mvn spring-boot:run
   ```

6. **Access the API**
   - Base URL: `http://localhost:8080/api`

## 📚 API Endpoints

### Driver Endpoints

- `POST /api/drivers/register` - Register a new driver
- `GET /api/drivers/{id}` - Get driver details
- `PUT /api/drivers/{id}/location` - Update driver location (GPS)
- `GET /api/drivers/available` - Get list of available drivers
- `PUT /api/drivers/{id}/status` - Update driver status (online/offline)
- `GET /api/drivers/{id}/earnings` - Get driver earnings

### Ride Request Endpoints

- `POST /api/rides/request` - Create a new ride request
- `GET /api/rides/{id}` - Get ride details
- `PUT /api/rides/{id}/accept` - Driver accepts a ride
- `PUT /api/rides/{id}/start` - Start the ride
- `PUT /api/rides/{id}/complete` - Complete the ride
- `PUT /api/rides/{id}/cancel` - Cancel a ride
- `GET /api/rides/user/{userId}` - Get user's ride history

### Rider Endpoints

- `POST /api/riders/register` - Register a new rider
- `GET /api/riders/{id}` - Get rider profile
- `PUT /api/riders/{id}` - Update rider profile
- `GET /api/riders/{id}/rides` - Get rider's past rides

### Rating Endpoints

- `POST /api/ratings` - Submit rating for driver/rider
- `GET /api/ratings/driver/{driverId}` - Get driver's average rating
- `GET /api/ratings/rider/{riderId}` - Get rider's average rating

### Location & Matching

- `GET /api/matching/nearby-drivers` - Find nearby drivers
- `GET /api/routes/distance` - Calculate distance between two points
- `GET /api/fare/estimate` - Estimate fare for a trip

## 🔑 Key Concepts

### Smart Matching Algorithm

The system uses a location-based matching algorithm to find the nearest available driver:
1. User requests a ride
2. System filters available drivers within a radius
3. Selects driver with shortest distance
4. Calculates estimated pickup time
5. Sends request to driver

### Fare Calculation

```
Fare = Base Fare + (Distance × Distance Rate) + (Time × Time Rate) + Surge Multiplier
```

### Real-time Location Tracking

Driver locations are updated periodically using GPS coordinates and stored in the database for quick retrieval during ride matching.

## 📊 Data Models

### Driver
- driverId, name, email, phone
- licenseNumber, vehicleInfo
- currentLocation (latitude, longitude)
- status (online/offline)
- rating, totalRides

### Rider
- riderId, name, email, phone
- homeLocation, workLocation
- paymentMethods
- rating, totalRides

### Ride
- rideId, riderId, driverId
- pickupLocation, dropoffLocation
- status (requested/accepted/in-progress/completed/cancelled)
- fare, distance, duration
- startTime, endTime

## 🧪 Testing

Run tests with Maven:

```bash
# Run all tests
mvn test

# Run specific test class
mvn test -Dtest=DriverServiceTest

# Run with coverage
mvn jacoco:report
```

## 📝 Example API Usage

### Register a Driver
```bash
curl -X POST http://localhost:8080/api/drivers/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Driver",
    "email": "john@example.com",
    "phone": "9876543210",
    "licenseNumber": "DL123456"
  }'
```

### Request a Ride
```bash
curl -X POST http://localhost:8080/api/rides/request \
  -H "Content-Type: application/json" \
  -d '{
    "riderId": 1,
    "pickupLat": 28.7041,
    "pickupLng": 77.1025,
    "dropoffLat": 28.5355,
    "dropoffLng": 77.3910,
    "rideType": "UberX"
  }'
```

### Update Driver Location
```bash
curl -X PUT http://localhost:8080/api/drivers/1/location \
  -H "Content-Type: application/json" \
  -d '{
    "latitude": 28.7041,
    "longitude": 77.1025
  }'
```

## 🔍 Distance Calculation

The API uses the Haversine formula or Google Maps API to calculate distances between coordinates:

```java
Distance = 2 * R * asin(sqrt(sin²((lat2-lat1)/2) + cos(lat1)*cos(lat2)*sin²((lon2-lon1)/2)))
```

where R is Earth's radius (6371 km)

## 📈 Performance Optimization

- Database indexing on frequently queried columns
- Caching driver locations for fast lookup
- Async processing for notifications
- Connection pooling for database efficiency
- API rate limiting to prevent abuse

## 🔄 Future Enhancements

- 🗺️ Google Maps integration for better routing
- 💳 Payment gateway integration
- 📱 Mobile app API endpoints
- 🔔 Real-time notifications (WebSocket)
- 🌍 Multi-city support
- 🎯 Destination prediction
- 🚴 Alternative transport modes (bike, scooter)
- 📊 Advanced analytics and reporting
- 🤖 Machine learning for demand prediction
- 🛡️ Enhanced security and fraud detection

## 👥 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch
   ```bash
   git checkout -b feature/amazing-feature
   ```
3. Commit your changes
   ```bash
   git commit -m 'Add amazing feature'
   ```
4. Push to your branch
   ```bash
   git push origin feature/amazing-feature
   ```
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 💬 Support & Feedback

- 📧 Report issues on [GitHub Issues](https://github.com/Ashish961169/Uber-Assignment-API/issues)
- 💭 Share suggestions and feature requests
- ⭐ Star the repository if you find it helpful!

## 📞 Contact

**Developer**: Ashish961169
- GitHub: [@Ashish961169](https://github.com/Ashish961169)
- Email: Open an issue for inquiries

## 🙏 Acknowledgments

- Uber for the concept and inspiration
- Java community for excellent frameworks
- Google Maps for location services
- All contributors who help improve this project

---

**Made with ❤️ for ride-sharing enthusiasts and developers**

*Last Updated: 2025 | Active Development*
