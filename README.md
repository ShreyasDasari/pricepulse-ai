# PricePulse AI - Smart Price Drop Hunter

PricePulse AI is an intelligent web application designed to help users find the best deals on products by tracking price drops and analyzing historical data. It uses AI to predict optimal buying times and notifies users when prices are at their lowest.

## Features

- **Product Tracking**: Monitor prices of products from various online retailers.
- **Price Drop Detection**: Automatically detects when a product's price drops below a certain threshold.
- **Historical Analysis**: Visualizes price history to identify trends and patterns.
- **AI-Powered Recommendations**: Uses machine learning to predict future price drops and suggest the best time to buy.
- **Email Notifications**: Sends alerts when a product is available at a desired price.
- **User Dashboard**: A centralized place to manage tracked products and view insights.

## Tech Stack

- **Frontend**: React, Tailwind CSS
- **Backend**: FastAPI (Python)
- **Database**: PostgreSQL
- **AI/ML**: Scikit-learn, Pandas, NumPy
- **Deployment**: Docker, Docker Compose

## Getting Started

### Prerequisites

- Docker
- Docker Compose

### Installation

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd pricepulse-ai
   ```

2. Start the application using Docker Compose:
   ```bash
   docker-compose up --build
   ```

3. Access the application:
   - Web App: http://localhost:3000
   - API Docs: http://localhost:8000/docs

## Project Structure

```
pricepulse-ai/
├── backend/          # FastAPI backend application
│   ├── app/
│   │   ├── api/        # API endpoints
│   │   ├── core/       # Core configurations
│   │   ├── models/     # Database models
│   │   ├── schemas/    # Pydantic schemas
│   │   ├── services/   # Business logic
│   │   └── main.py     # Application entry point
│   ├── requirements.txt
│   └── Dockerfile
├── frontend/         # React frontend application
│   ├── src/
│   │   ├── components/ # React components
│   │   ├── pages/      # Page components
│   │   ├── services/   # API services
│   │   ├── utils/      # Utility functions
│   │   └── App.jsx     # Main application component
│   ├── package.json
│   └── Dockerfile
├── docker-compose.yml
├── .env              # Environment variables (not in git)
└── README.md
```

## Configuration

Create a `.env` file in the root directory with the following variables:

```env
# Database Configuration
DB_USER=postgres
DB_PASSWORD=your_password
DB_NAME=pricepulse
DB_HOST=db
DB_PORT=5432

# API Configuration
API_PORT=8000

# Frontend Configuration
FRONTEND_PORT=3000

# Email Configuration (for notifications)
EMAIL_HOST=smtp.example.com
EMAIL_PORT=587
EMAIL_USER=your_email
EMAIL_PASSWORD=your_password
```

## Usage

### Adding a Product

1. Go to the PricePulse AI web application at http://localhost:3000
2. Click on "Add Product"
3. Enter the product URL and your desired price
4. Click "Track Product"

### Viewing Price History

On the dashboard, you can view:
- Current price of tracked products
- Price drop percentage
- Price history charts
- AI-generated insights

## Development

### Running in Development Mode

To run the application without Docker:

1. **Backend**:
   ```bash
   cd backend
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   pip install -r requirements.txt
   uvicorn app.main:app --reload
   ```

2. **Frontend**:
   ```bash
   cd frontend
   npm install
   npm start
   ```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.