# Network Security Phishing Detection System

A machine learning-based web application for detecting phishing websites using various URL and domain features. This project implements a complete ML pipeline from data ingestion to model deployment with FastAPI.

## About

This project is a comprehensive phishing detection system that analyzes website URLs and domains to identify potential phishing attempts. The system uses machine learning algorithms to classify websites as legitimate or phishing based on features like IP address presence, URL length, SSL certificate status, domain age, and other security indicators.

The application provides both training capabilities for the ML model and a prediction API for real-time phishing detection. It includes a complete MLOps pipeline with data validation, transformation, model training, and deployment capabilities.

## Tech Stack

### Core Technologies
- **Python 3.10** - Primary programming language
- **FastAPI** - Web framework for API development
- **Uvicorn** - ASGI server for running FastAPI applications
- **Jinja2** - Template engine for HTML rendering

### Machine Learning & Data Science
- **Scikit-learn** - Machine learning library for model training and evaluation
- **Pandas** - Data manipulation and analysis
- **NumPy** - Numerical computing
- **MLflow** - Model tracking and experiment management
- **DagsHub** - ML experiment tracking and model versioning

### Database & Storage
- **MongoDB** - NoSQL database for data storage
- **PyMongo** - MongoDB driver for Python
- **AWS S3** - Cloud storage for model artifacts

### DevOps & Deployment
- **Docker** - Containerization
- **AWS ECR** - Container registry
- **AWS ECS** - Container orchestration
- **GitHub Actions** - CI/CD pipeline
- **Self-hosted runners** - Deployment infrastructure

### Development Tools
- **Python-dotenv** - Environment variable management
- **Certifi** - SSL certificate handling
- **PyYAML** - YAML configuration parsing

## Prerequisites

- **Python 3.10** or higher
- **MongoDB** instance (local or cloud)
- **AWS Account** (for deployment)
- **DagsHub Account** (for ML experiment tracking)

## Installation

### 1. Clone the Repository
```bash
git clone https://github.com/sebastian-gm/networksecurity.git
cd networksecurity
```

### 2. Create Virtual Environment
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Environment Configuration
Create a `.env` file in the root directory:
```bash
# MongoDB Configuration
MONGODB_URL_KEY=mongodb+srv://username:password@cluster.mongodb.net/?retryWrites=true&w=majority

# DagsHub Configuration (for ML experiment tracking)
DAGSHUB_USER_TOKEN=your_dagshub_token

# AWS Configuration (for deployment)
AWS_ACCESS_KEY_ID=your_aws_access_key
AWS_SECRET_ACCESS_KEY=your_aws_secret_key
AWS_REGION=your_aws_region
```

### 5. Data Setup
Place your phishing dataset in the `Network_Data/` directory. The system expects a CSV file with the following features:
- having_IP_Address
- URL_Length
- Shortining_Service
- having_At_Symbol
- double_slash_redirecting
- Prefix_Suffix
- having_Sub_Domain
- SSLfinal_State
- Domain_registeration_length
- Favicon
- port
- HTTPS_token
- Request_URL
- URL_of_Anchor
- Links_in_tags
- SFH
- Submitting_to_email
- Abnormal_URL
- Redirect
- on_mouseover
- RightClick
- popUpWidnow
- Iframe
- age_of_domain
- DNSRecord
- web_traffic
- Page_Rank
- Google_Index
- Links_pointing_to_page
- Statistical_report
- Result (target variable)

## Usage

### Development Mode

#### 1. Start the Application
```bash
python app.py
```
The application will be available at `http://localhost:8000`

#### 2. Access the API Documentation
Navigate to `http://localhost:8000/docs` for interactive API documentation.

#### 3. Train the Model
```bash
# Via API
curl -X GET "http://localhost:8000/train"

# Or run the training pipeline directly
python main.py
```

#### 4. Make Predictions
Upload a CSV file with phishing features to the `/predict` endpoint:
```bash
curl -X POST "http://localhost:8000/predict" \
  -H "accept: application/json" \
  -H "Content-Type: multipart/form-data" \
  -F "file=@your_data.csv"
```

### Production Deployment

#### Docker Deployment
```bash
# Build the Docker image
docker build -t networksecurity .

# Run the container
docker run -p 8000:8000 \
  -e MONGODB_URL_KEY="your_mongodb_url" \
  -e DAGSHUB_USER_TOKEN="your_token" \
  networksecurity
```

#### AWS ECS Deployment
The project includes GitHub Actions workflows for automated deployment to AWS ECS. Configure the following secrets in your GitHub repository:
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_REGION`
- `ECR_REGISTRY`
- `ECR_REPOSITORY_NAME`
- `DAGSHUB_USER_TOKEN`

## Testing

### Run Unit Tests
```bash
# Add test commands when test suite is implemented
echo "Running unit tests"
```

### Test MongoDB Connection
```bash
python test_mongodb.py
```

## Configuration

### Environment Variables

| Variable | Description | Required | Default |
|----------|-------------|----------|---------|
| `MONGODB_URL_KEY` | MongoDB connection string | Yes | - |
| `DAGSHUB_USER_TOKEN` | DagsHub authentication token | Yes | - |
| `AWS_ACCESS_KEY_ID` | AWS access key for deployment | Yes | - |
| `AWS_SECRET_ACCESS_KEY` | AWS secret key for deployment | Yes | - |
| `AWS_REGION` | AWS region for deployment | Yes | - |

### Data Schema
The project uses a YAML schema file (`data_schema/schema.yaml`) to validate input data structure and types.

## Project Structure

```
networksecurity/
├── app.py                      # FastAPI application entry point
├── main.py                     # Training pipeline entry point
├── push_data.py               # Data ingestion utility
├── requirements.txt           # Python dependencies
├── Dockerfile                 # Docker configuration
├── setup.py                   # Package configuration
├── .env                       # Environment variables (create this)
├── .github/workflows/         # CI/CD pipelines
├── networksecurity/           # Main package
│   ├── components/           # ML pipeline components
│   ├── pipeline/            # Training pipeline
│   ├── utils/               # Utility functions
│   ├── entity/              # Configuration entities
│   ├── constant/            # Constants and configurations
│   ├── exception/           # Custom exceptions
│   └── logging/             # Logging configuration
├── templates/                # HTML templates
├── final_model/             # Trained model artifacts
├── prediction_output/        # Prediction results
├── Network_Data/            # Dataset directory
├── data_schema/             # Data validation schemas
└── notebooks/               # Jupyter notebooks (if any)
```

## Contributing

### Development Workflow
1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Make your changes and commit: `git commit -m 'Add feature'`
4. Push to the branch: `git push origin feature/your-feature-name`
5. Create a Pull Request

### Code Style
- Follow PEP 8 Python style guidelines
- Use meaningful variable and function names
- Add docstrings to functions and classes
- Include type hints where appropriate

### Testing Guidelines
- Write unit tests for new functionality
- Ensure all tests pass before submitting PR
- Add integration tests for API endpoints

## Troubleshooting

### Common Issues

#### MongoDB Connection Issues
- Verify your MongoDB connection string in `.env`
- Ensure MongoDB instance is running and accessible
- Check network connectivity and firewall settings

#### Model Training Failures
- Verify dataset format matches schema requirements
- Check available memory for large datasets
- Ensure all dependencies are installed correctly

#### Docker Build Issues
- Ensure Docker is running
- Check Dockerfile syntax and base image availability
- Verify all required files are present in build context

#### AWS Deployment Issues
- Verify AWS credentials and permissions
- Check ECR repository exists and is accessible
- Ensure ECS cluster has sufficient resources

### Logs and Debugging
- Application logs are available in the console output
- MLflow tracking UI shows experiment metrics
- Check Docker logs: `docker logs container_name`


## Contact

- **Author**: Sebastian Gonzalez
- **Email**: sebastiangm.dev@gmail.com
- **GitHub**: [sebastian-gm](https://github.com/sebastian-gm)

## Acknowledgments

- Dataset contributors for phishing detection data
- Open source community for ML and web development tools
- AWS for cloud infrastructure services

---

**Note**: This project is designed for educational and research purposes. Always validate predictions in production environments and follow security best practices.