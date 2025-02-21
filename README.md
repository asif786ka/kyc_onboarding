# kyc_onboarding

# TradingKYC - Flutter-based KYC Onboarding System

## Overview

TradingKYC is a **Flutter-based KYC Onboarding System** designed for **kyc onboarding**. It ensures **regulatory compliance** by integrating **AI/ML-powered identity verification, secure document handling, and compliance checks** against **sanctions/PEP databases**.

The project follows **Clean Architecture** principles using **BLoC & Riverpod** for state management, **Dio for API interactions**, and **Flutter Secure Storage** for handling sensitive user data.

## 1. System Architecture

The architecture consists of three key layers:

- **Presentation Layer**: UI screens, widgets, and state management using **BLoC/Riverpod**.
- **Domain Layer**: Business logic, use cases, and entities that interact with repositories.
- **Data Layer**: API services, repositories, secure storage, and compliance checks.

## 2. Setup Instructions

### **1. Clone the Repository**

```bash
git clone https://github.com/asif786ka/kyc_onboarding/.git
cd tradingkyc
```

### **2. Install Dependencies**

```bash
flutter pub get
```

### **3. Setup Environment Variables**

Create a `.env` file and add:

```env
API_BASE_URL="https://your-api.com"
AUTH_SECRET="your-secret-key"
```

### **4. Run the Application**

```bash
flutter run
```

## 3. Folder Structure & Class Explanations

Below is a breakdown of each **folder** and the **classes** within them:

### **📂 lib/**
This is the root directory where all Flutter application code is stored.

### **📂 core/**
Contains core utilities that are used throughout the app.

| File | Purpose |
|------|---------|
| `constants/app_strings.dart` | Defines text strings used across the app. |
| `constants/app_routes.dart` | Defines all navigation routes. |
| `constants/api_endpoints.dart` | Stores all backend API endpoints. |
| `error/failure.dart` | Defines failure models for handling errors. |
| `error/error_handler.dart` | Centralized error handling mechanism. |
| `utils/validators.dart` | Implements input validation logic. |
| `utils/image_picker.dart` | Handles document image selection. |
| `utils/secure_storage.dart` | Handles secure local storage for user data. |
| `network/api_client.dart` | API request management using **Dio**. |
| `network/api_interceptor.dart` | Adds authentication headers & logs API responses. |
| `dependency_injection/service_locator.dart` | Registers services, repositories & dependencies. |
| `theme/app_theme.dart` | Defines UI themes and styles. |
| `localization/app_localizations.dart` | Manages multi-language support. |
| `router.dart` | Configures named navigation routes. |

---

### **📂 features/**
Contains **feature-specific modules**. Each feature has its own **presentation, domain, and data layers**.

#### **📂 kyc_onboarding/**
Handles the **KYC onboarding process**, including **document uploads, face verification, and compliance checks**.

##### **📂 presentation/**
Responsible for UI screens, widgets, and state management.

| File | Purpose |
|------|---------|
| `blocs/kyc_bloc.dart` | BLoC for managing KYC state. |
| `blocs/kyc_event.dart` | Defines KYC-related events. |
| `blocs/kyc_state.dart` | Represents various states of KYC onboarding. |
| `providers/kyc_provider.dart` | Riverpod provider for managing KYC state. |
| `pages/kyc_screen.dart` | UI for KYC onboarding steps. |
| `pages/document_upload_screen.dart` | UI for uploading documents. |
| `pages/face_verification_screen.dart` | UI for face verification. |
| `pages/compliance_check_screen.dart` | UI for checking user compliance. |
| `widgets/kyc_progress_bar.dart` | Displays onboarding progress. |
| `widgets/document_picker.dart` | Handles document selection. |
| `widgets/face_scan_widget.dart` | Handles real-time face scanning. |

##### **📂 domain/**
Contains **business logic, use cases, and entity models**.

| File | Purpose |
|------|---------|
| `entities/user_entity.dart` | Defines business logic for a user. |
| `entities/document_entity.dart` | Represents document information. |
| `entities/compliance_entity.dart` | Represents compliance verification model. |
| `usecases/start_kyc_usecase.dart` | Starts the KYC process. |
| `usecases/upload_document_usecase.dart` | Handles document uploads. |
| `usecases/verify_face_usecase.dart` | Calls ML-based face verification API. |
| `usecases/check_compliance_usecase.dart` | Checks PEP/sanctions compliance. |

##### **📂 data/**
Handles **data persistence, API integration, and repositories**.

| File | Purpose |
|------|---------|
| `models/user_model.dart` | Defines user model for API interaction. |
| `models/document_model.dart` | Defines document model for API. |
| `models/compliance_model.dart` | Defines compliance model for API. |
| `repositories/kyc_repository.dart` | Abstract repository for KYC operations. |
| `repositories/kyc_repository_impl.dart` | Implements KYC repository logic. |
| `datasources/remote/kyc_remote_data_source.dart` | Fetches data from API. |
| `datasources/local/kyc_local_data_source.dart` | Manages local data storage. |
| `services/api_service.dart` | Manages API calls related to KYC. |
| `services/document_service.dart` | Handles document upload processing. |
| `services/ocr_service.dart` | Calls OCR API for text extraction. |
| `services/face_verification_service.dart` | Calls AI-based face verification API. |
| `services/compliance_service.dart` | Calls compliance verification API. |

---

### **📂 testing/**
Contains **unit and integration tests**.

| Folder | Purpose |
|------|---------|
| `unit/` | Unit tests for core logic. |
| `widget/` | UI tests for individual screens & components. |
| `integration/` | End-to-end tests for API interactions. |

---

## 4. Dependencies

- **State Management**: `flutter_bloc`, `riverpod`
- **Networking**: `dio`
- **Secure Storage**: `flutter_secure_storage`
- **File Handling**: `file_picker`, `image_picker`
- **AI/ML**: Custom API for OCR & Face Verification

## 5. Testing

Run unit tests with:

```bash
flutter test
```


## 6. Key Implementation Details for Exinity Trading App KYC

State Management: Uses BLoC for UI state and Riverpod for dependency injection and state management.
Repositories: Implements repository pattern for data retrieval and API communication.
Secure Storage: Uses flutter_secure_storage and Hive for storing user authentication and KYC data.
AI/ML Integration:
OCR Processing (Amazon Textract, Google Vision, or Tesseract OCR)
Face Verification (Amazon Rekognition, Jumio, or Onfido)
Compliance & Verification:
Sanctions, PEP, and DMV Checks using Third-Party APIs.
Secure Document Handling:
AWS S3, Firebase Storage, or PostgreSQL for compliance record keeping.
Notifications System:
AWS SES for email notifications.
Firebase Cloud Messaging for push notifications.


## 📌 Authors asif786ka@gmail.com

**Asif Ali Kazmi**
