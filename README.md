# 🔐 Secure File Sharing System

## 📋 Project Overview

This comprehensive file-sharing platform was developed as a backend engineering assessment for **EZ Works**. The system demonstrates enterprise-level security practices while providing a robust API for file management operations. It's designed to facilitate secure file exchange between different user roles with strict access controls and security measures.

### 🎯 Purpose and Goals

The primary objective of this system is to create a secure, scalable file-sharing solution that addresses common security vulnerabilities found in traditional file-sharing platforms. By implementing role-based access control, content-based file validation, and encrypted download mechanisms, this system ensures that sensitive data remains protected throughout its lifecycle.

---

## ✨ Key Features & Capabilities

### 🏗️ Technology Architecture

Our technology stack has been carefully selected to provide optimal security, performance, and maintainability:

- **🌐 Web Framework**: Flask - A lightweight yet powerful Python web framework that provides flexibility and ease of development
- **🗄️ Database**: MongoDB - A NoSQL database chosen for its scalability and flexible document structure, perfect for storing file metadata and user information
- **🔑 Authentication System**: JWT (JSON Web Tokens) - Stateless authentication mechanism that ensures secure user sessions without server-side session storage
- **🔍 File Analysis**: python-magic library - Advanced file type detection that goes beyond simple extension checking by analyzing file headers and content structure

### 🛡️ Advanced Security Features

The system implements multiple layers of security to protect against common attack vectors:

- **Content-Based File Validation**: Unlike traditional systems that rely solely on file extensions, our system analyzes the actual file content to determine its true type, preventing malicious files from being disguised with innocent extensions
- **Encrypted Download URLs**: All download links are generated with encrypted tokens that have configurable expiration times, ensuring that file access is time-limited and cannot be shared indefinitely
- **Secure Password Management**: User passwords are hashed using industry-standard algorithms before storage, ensuring that even database breaches cannot expose user credentials
- **Role-Based Access Control**: Different user types (Operation Users and Client Users) have distinct permissions, ensuring proper segregation of duties

---

## 🔌 API Documentation

### 🔐 Authentication Endpoints

These endpoints handle user registration, email verification, and login processes:

- **POST `/signup`**: Creates a new user account with email verification
  - Validates user input, creates secure password hashes, and sends verification emails
- **GET `/verify-email/<token>`**: Confirms user email addresses using secure tokens
  - Prevents unauthorized account creation and ensures valid contact information
- **POST `/login`**: Authenticates users and provides JWT tokens for subsequent requests
  - Returns access tokens that must be included in all authenticated API calls

### 📁 File Management Endpoints

These endpoints provide comprehensive file operations with proper authorization:

- **POST `/upload`**: Allows Operation Users to securely upload files to the system
  - Validates file types, checks user permissions, and stores files with metadata
- **GET `/files`**: Retrieves a list of all uploaded files with their metadata
  - Returns file information including upload dates, sizes, and access permissions
- **GET `/download/<file_id>`**: Generates secure, time-limited download URLs
  - Creates encrypted tokens that expire after a specified duration for security
- **GET `/secure-download/<token>`**: Processes secure download requests using encrypted tokens
  - Validates tokens, checks expiration, and serves files to authorized users

---

## 🚀 Future Enhancement Roadmap

The following improvements are planned to further enhance the system's capabilities:

- **⚡ Rate Limiting Implementation**: Prevent abuse by limiting the number of requests per user/IP address within specific time windows
- **🔒 File Encryption at Rest**: Encrypt stored files using AES-256 encryption to protect data even if storage is compromised
- **👥 Granular Permission System**: Implement fine-grained access controls allowing specific file permissions per user or group
- **📊 Comprehensive Logging & Monitoring**: Add detailed audit trails and real-time monitoring for security events and system performance

---

## 🧪 Testing & Quality Assurance

### Comprehensive Test Suite

The system includes a robust testing framework to ensure reliability and security:

```shell
pytest -v tests/
```

Our testing approach includes multiple layers of validation:

- **Unit Tests**: Individual component testing to verify core functionality
- **Integration Tests**: End-to-end testing of API endpoints and user workflows
- **Security Tests**: Validation of authentication, authorization, and file handling security

### 📊 Test Results & Documentation

For transparency and quality assurance, we provide detailed test documentation:

- **📋 Pytest Results**: Review detailed test execution logs at [tests/test_results.log](tests/test_results.log)
- **🔍 Postman Test Summary**: Comprehensive API testing results available at [assets/Secure File Sharing System.postman_test_run.json](assets/Secure%20File%20Sharing%20System.postman_test_run.json)

<p align="center">
  <img width="700" src="assets/Postman%20Test%20Summary.png" alt="Postman Test Results Summary">
</p>

### 🛠️ API Testing with Postman

To facilitate API testing and development, we provide a complete Postman collection:

- **Import Collection**: Use the Postman collection file at [assets/Secure File Sharing System.postman_collection.json](assets/Secure%20File%20Sharing%20System.postman_collection.json)
- **Pre-configured Requests**: All API endpoints with sample data and authentication headers
- **Environment Variables**: Easily switch between development, testing, and production environments

---

## 🛠️ Development Setup & Installation

### Prerequisites

Before beginning the installation process, ensure your development environment meets these requirements:

- **Python Version**: Python 3.12 or later (recommended for optimal performance and security features)
- **Operating System**: Compatible with Windows, macOS, and Linux distributions
- **Memory**: Minimum 4GB RAM for development environment
- **Storage**: At least 1GB free space for dependencies and file uploads

### Step-by-Step Installation Guide

Follow these detailed instructions to set up your local development environment:

#### 1. 📥 Repository Setup
Clone the repository and navigate to the project directory:
```shell
git clone https://github.com/yashanksingh/Secure-File-Sharing-System.git
cd Secure-File-Sharing-System
```

#### 2. 🐍 Virtual Environment Configuration
Create an isolated Python environment to avoid dependency conflicts:
```shell
python -m venv venv

# Windows activation
./venv/Scripts/activate

# Linux/macOS activation
source venv/bin/activate
```

#### 3. 📦 Dependency Installation
Install all required packages and platform-specific dependencies:
```shell
# Core dependencies
pip install -r requirements.txt

# Windows-specific file type detection
pip install python-magic-bin~=0.4.14  # Windows only

# Linux-specific system libraries
sudo apt update && sudo apt install -y libmagic-dev  # Linux only
```

#### 4. ⚙️ Environment Configuration
Create a `.env` file in the root directory with the following configuration variables:

```env
# Security Configuration
SECRET_KEY=your_secret_key_here

# Database Configuration
MONGO_URI=mongodb://localhost:27017/secure_file_sharing

# File Storage Configuration
UPLOAD_FOLDER=./uploads

# Email Service Configuration (SMTP2GO)
SMTP2GO_API_KEY=your_smtp2go_api_key
SMTP2GO_SENDER='Your App Name <noreply@yourdomain.com>'

# Application Configuration
BASE_URL=http://127.0.0.1:5000  # Leave empty for default localhost:5000
```

#### 5. 🚀 Application Launch
Start the development server:
```shell
python run.py
```

The application will be accessible at `http://127.0.0.1:5000`

---

## 🐳 Production Deployment with Docker

### Containerization Strategy

Docker deployment provides a consistent, scalable, and portable solution for running the Secure File Sharing System in production environments. This approach ensures that the application runs identically across different environments and simplifies deployment processes.

### Step-by-Step Deployment Guide

#### 1. 📄 Dockerfile Creation

Create a `Dockerfile` in the root directory with optimized production settings:

```dockerfile
FROM python:3.12-slim

# Set working directory
WORKDIR /SFSS

# Copy application files
COPY . .

# Install system dependencies and Python packages
RUN pip install -r requirements.txt && \
    apt update && apt install -y libmagic-dev && \
    pip install waitress && \
    mkdir uploads && \
    apt clean && rm -rf /var/lib/apt/lists/*

# Expose application port
EXPOSE 5000

# Use production WSGI server
CMD ["waitress-serve", "--port=5000", "--call", "app:create_app"]
```

#### 2. 🚫 Docker Ignore Configuration

Create a `.dockerignore` file to optimize build performance and security:

```dockerignore
# Development files
.idea/
venv/
.pytest_cache
*.pyc
__pycache__/

# Version control
.git/
.gitignore

# Runtime directories
uploads/

# Documentation and tests
tests/
README.md
Dockerfile
.dockerignore

# Environment files (should be configured separately)
.env
```

#### 3. 🏗️ Image Building

Build the Docker image with proper tagging:

```shell
# Build the image
sudo docker build -t secure-file-sharing-system:latest .

# Optional: Tag for version control
sudo docker tag secure-file-sharing-system:latest sfss:v1.0.0
```

#### 4. 🚀 Container Deployment

Deploy the container with production-ready configuration:

```shell
# Run with volume mounting for persistent storage
sudo docker run \
  --name sfss-production \
  -d \
  -p 5000:5000 \
  -v $(pwd)/uploads:/uploads \
  -v $(pwd)/.env:/SFSS/.env \
  --restart unless-stopped \
  secure-file-sharing-system:latest
```

**Command Explanation:**
- **`-d`**: Detached mode - container runs in the background
- **`-p 5000:5000`**: Port mapping - exposes container port 5000 to host port 5000
- **`-v $(pwd)/uploads:/uploads`**: Volume mount for persistent file storage
- **`-v $(pwd)/.env:/SFSS/.env`**: Mount environment configuration
- **`--restart unless-stopped`**: Automatic restart policy for production reliability

#### 5. 🌐 Application Access

Once deployed, the application will be accessible at:
- **Local Access**: `http://localhost:5000`
- **Network Access**: `http://[server-ip]:5000`

### 🔧 Production Considerations

- **Environment Variables**: Ensure all production environment variables are properly configured
- **SSL/TLS**: Consider using a reverse proxy (nginx) for HTTPS termination
- **Monitoring**: Implement container health checks and logging
- **Scaling**: Use Docker Compose or Kubernetes for multi-container deployments

---

## 🤝 Contributing & Community

### Contributing Guidelines

While this project was initially developed as an assessment for EZ Works, we welcome community contributions to enhance its functionality and security features. Here's how you can contribute:

#### 🐛 Reporting Issues
- Use the GitHub Issues tab to report bugs or security vulnerabilities
- Provide detailed reproduction steps and environment information
- Include relevant logs and error messages

#### 💡 Suggesting Enhancements
- Open feature requests with clear use cases and benefits
- Discuss implementation approaches before submitting large changes
- Consider backward compatibility and security implications

#### 🔧 Code Contributions
- Fork the repository and create feature branches
- Follow existing code style and documentation standards
- Include comprehensive tests for new functionality
- Submit pull requests with detailed descriptions

### 📞 Support & Communication

For questions, discussions, or support:
- **Issues**: GitHub Issues for bug reports and feature requests
- **Discussions**: GitHub Discussions for general questions and community interaction
- **Security**: Report security vulnerabilities through private channels

---

## 📜 License & Legal

### MIT License

This project is licensed under the MIT License, which provides:

- **Freedom to Use**: Use the software for any purpose, including commercial applications
- **Freedom to Modify**: Modify the source code to suit your needs
- **Freedom to Distribute**: Share the software with others
- **Freedom to Sublicense**: Include the software in larger projects with different licenses

**Full License Text**: See the [LICENSE](LICENSE) file for complete terms and conditions.

### 🔒 Security Disclaimer

While this system implements multiple security measures, users are responsible for:
- Keeping dependencies updated
- Configuring secure environment variables
- Implementing additional security measures as needed for their specific use case
- Regular security audits and penetration testing

---

## 📈 Project Statistics & Acknowledgments

### 🏆 Assessment Achievement

This project successfully demonstrates:
- **Enterprise-level security practices**
- **Scalable architecture design**
- **Comprehensive testing methodologies**
- **Production-ready deployment strategies**

### 🙏 Acknowledgments

- **EZ Works**: For providing the opportunity to showcase backend engineering skills
- **Flask Community**: For the excellent web framework and documentation
- **MongoDB**: For the robust NoSQL database solution
- **Open Source Contributors**: For the various libraries and tools that made this project possible

---

*Built with ❤️ for secure, scalable file sharing*
