# Multi-Region KYC Onboarding Library (Flutter) - Monorepo Approach

## Overview

The **Multi-Region KYC Main app project integrating KYC Onboarding Library** is a **Flutter package** designed for **Know Your Customer (KYC) onboarding and compliance verification** across multiple regions. It supports **dynamic region selection**, **AI-based identity verification**, **secure storage per region**, and **regulatory compliance (GDPR, CCPA, DFSA, etc.)**.

This project follows a **Monorepo Approach** for **scalability and efficient branch management**, allowing seamless integration of the **KYC Library** with the **Main Flutter App** while enabling independent deployment workflows.

## 1. Monorepo Structure

```
kyc_monorepo/
├── apps/                           # Main Flutter Applications
│   ├── main_app/                   # Primary app using KYC Library
│   │   ├── lib/                    
│   │   │   ├── main.dart            # Entry point for the app
│   │   │   ├── features/kyc/        # Integration of KYC Library
│   │   │   ├── pubspec.yaml         # Includes local dependency to KYC Library
│   ├── kyc_library/                 # Standalone KYC Library Package
│   │   ├── lib/                    
│   │   │   ├── kyc_region_library.dart  # Library entry point
│   │   │   ├── src/                  # Core library code
│   │   │   ├── pubspec.yaml           # Library dependencies
│
├── packages/                        # Shared Utility Packages
│   ├── api_services/                 # Common API service package
│   ├── ui_components/                # Shared UI widgets across apps
│
├── .github/                          # GitHub Actions CI/CD Workflow
│   ├── workflows/
│   │   ├── deploy_main_app.yml       # Deployment pipeline for main app
│   │   ├── deploy_kyc_library.yml    # Deployment pipeline for KYC Library
│
├── scripts/                          # Deployment & automation scripts
│   ├── deploy_main_app.sh            # Automated deployment script for main app
│   ├── deploy_kyc_library.sh         # Deployment script for KYC library
│
├── README.md                         # Documentation
```

## 2. Git Branch Management Strategy

To efficiently manage development and releases, we follow **Gitflow** branch management:

- **`main`** – Production-ready code.
- **`develop`** – Integration branch for new features.
- **`feature/{feature-name}`** – Individual feature branches.
- **`release/{version}`** – Stabilization branches before releases.
- **`hotfix/{fix-name}`** – Emergency bug fixes on production.

### **Branching Workflow for Library & Main App**

1. Create a feature branch:
   ```bash
   git checkout -b feature/add-new-kyc-feature
   ```
2. Work on the feature and commit changes:
   ```bash
   git add .
   git commit -m "Added new KYC feature"
   ```
3. Push and create a pull request:
   ```bash
   git push origin feature/add-new-kyc-feature
   ```
4. Merge into `develop` and later into `main`.

---

## 3. **Integrating KYC Library into the Main App**

1️⃣ **Clone the Monorepo**

```bash
git clone https://github.com/exinity/kyc_monorepo.git
cd kyc_monorepo
```

2️⃣ **Configure the Main App to Use the KYC Library**

Edit the **pubspec.yaml** of the **Main App** to include the **local KYC library**:

```yaml
dependencies:
  flutter:
    sdk: flutter
  kyc_region_library:
    path: ../../kyc_library
```

3️⃣ **Import and Use the KYC Library**

In `main.dart` of the **Main App**, integrate the **KYC Library**:

```dart
import 'package:kyc_region_library/kyc_region_library.dart';

void main() {
  String userRegion = RegionSelector.getRegion("US");

  runApp(KYCApp(region: userRegion));
}
```

4️⃣ **Run the Main App with KYC Integration**

```bash
flutter run --flavor dev
```

---

## 4. **Deployment Strategies**

### **🔹 GitHub Actions CI/CD for Main App Deployment**
`.github/workflows/deploy_main_app.yml`

```yaml
name: Deploy Main App

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v2

      - name: Setup Flutter
        uses: subosito/flutter-action@v2
        with:
          flutter-version: 3.10.0

      - name: Install Dependencies
        run: flutter pub get

      - name: Build APK
        run: flutter build apk --release

      - name: Deploy to Firebase
        run: firebase deploy --only hosting
```

### **🔹 GitHub Actions CI/CD for KYC Library Deployment**
`.github/workflows/deploy_kyc_library.yml`

```yaml
name: Deploy KYC Library

on:
  push:
    paths:
      - 'kyc_library/**'
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v2

      - name: Setup Flutter
        uses: subosito/flutter-action@v2
        with:
          flutter-version: 3.10.0

      - name: Install Dependencies
        run: flutter pub get

      - name: Run Tests
        run: flutter test

      - name: Publish to Pub.dev
        run: flutter pub publish --dry-run
```

---

## **5. Deployment Using CircleCI**

#### **CircleCI Config for Main App Deployment**

`.circleci/config.yml`

```yaml
version: 2.1

jobs:
  build:
    docker:
      - image: circleci/flutter:latest

    steps:
      - checkout
      - run:
          name: Install Dependencies
          command: flutter pub get
      - run:
          name: Run Tests
          command: flutter test
      - run:
          name: Build Release APK
          command: flutter build apk --release
      - persist_to_workspace:
          root: .
          paths:
            - build/app/outputs/apk/release/app-release.apk

workflows:
  version: 2
  build-and-deploy:
    jobs:
      - build
```

---

## **6. Future Enhancements**

✅ **Automated Release Versioning** – Implement GitHub Actions to auto-bump versions.  
✅ **Multi-Region API Load Balancing** – Deploy via AWS Global Accelerator.  
✅ **Flutter Web Integration** – Extend KYC onboarding for web platforms.  



-----------------------------------------------------------------------------------------------------------------------------------------
# Multi-Region KYC Onboarding Library (Flutter) as an independent project which can be integrated by other projects

## Overview

The **Multi-Region KYC Onboarding Library** is a **Flutter package** designed to handle **Know Your Customer (KYC) onboarding and compliance verification** across multiple regions. It supports **dynamic region selection**, **AI-based identity verification**, **secure storage per region**, and **regulatory compliance (GDPR, CCPA, DFSA, etc.)**.

The project follows the **Clean architecture**, leveraging **BLoC & Riverpod** for state management, **Dio for API interactions**, and **Flutter Secure Storage** for handling sensitive user data.

## 1. System Architecture

The library is structured into **four key layers**:

- **Data Layer**: Multi-region API management, secure storage handling, and compliance checks.
- **Domain Layer**: Business logic, use cases, and entity models.
- **Business Logic Layer**: Handles state management using BLoC/Riverpod.
- **Presentation Layer**: UI screens and reusable components.

## 2. Folder Structure

```
kyc_region_library/
├── lib/
│   ├── kyc_region_library.dart        # Entry point for the library package
│   │
│   ├── src/
│   │   ├── regions/                   # Multi-region configuration
│   │   │   ├── region_selector.dart   # Auto-detects user's region
│   │   │   ├── region_config.dart     # Configuration based on region
│   │   │   ├── us/                    # US-specific implementation
│   │   │   │   ├── us_api_provider.dart
│   │   │   │   ├── us_compliance.dart
│   │   │   │   ├── us_secure_storage.dart
│   │   │   ├── eu/                    # EU-specific implementation
│   │   │   │   ├── eu_api_provider.dart
│   │   │   │   ├── eu_compliance.dart
│   │   │   │   ├── eu_secure_storage.dart
│   │   │   ├── uk/                    # UK-specific implementation
│   │   │   │   ├── uk_api_provider.dart
│   │   │   │   ├── uk_compliance.dart
│   │   │   │   ├── uk_secure_storage.dart
│   │   │   ├── ae/                    # UAE-specific implementation
│   │   │   │   ├── ae_api_provider.dart
│   │   │   │   ├── ae_compliance.dart
│   │   │   │   ├── ae_secure_storage.dart
│   │
│   │   ├── api/                       # API Service Layer
│   │   │   ├── api_service.dart       # Handles region-based API calls
│   │   │   ├── kyc_repository.dart    # Abstract KYC repository
│   │
│   │   ├── storage/                   # Secure storage handling
│   │   │   ├── secure_storage.dart    # Global storage implementation
│   │
│   │   ├── compliance/                # Regulatory compliance per region
│   │   │   ├── compliance_checker.dart
│   │
│   │   ├── ui/                        # UI Components for KYC
│   │   │   ├── kyc_screen.dart        # Main KYC screen
│   │   │   ├── document_upload.dart   # Document upload UI
│   │   │   ├── face_verification.dart # Face verification UI
│   │
│   ├── models/                        # Data models for API responses
│   │   ├── kyc_user_model.dart
│   │   ├── compliance_model.dart
│   │   ├── document_model.dart
│
├── example/                           # Example Flutter app for testing
│   ├── main.dart                      # Shows how to use the package
│
├── pubspec.yaml                        # Package dependencies and metadata
├── README.md                           # Documentation on how to use the library
```

## 3. Features

✅ **Multi-Region API Handling** – Dynamically selects API endpoints per user region.  
✅ **Secure Storage Per Region** – Implements **GDPR, CCPA, DFSA** compliance.  
✅ **AI-Based KYC Verification** – OCR-based document verification and face matching.  
✅ **Dynamic UI Adaptation** – UI components adjust based on compliance requirements.  
✅ **Scalable Architecture** – Uses **Very Good Flutter Architecture** for modularity.  

## 4. Setup Instructions

### **1️⃣ Clone the Repository**

```bash
git clone https://github.com/exinity/kyc_region_library.git
cd kyc_region_library
```

### **2️⃣ Install Dependencies**

```bash
flutter pub get
```

### **3️⃣ Configure API and Storage per Region**

Modify `.env` file to include **region-specific endpoints**:

```env
API_BASE_URL_US="https://api.us-kyc.com"
API_BASE_URL_EU="https://api.eu-kyc.com"
API_BASE_URL_UK="https://api.uk-kyc.com"
API_BASE_URL_AE="https://api.ae-kyc.com"
```

### **4️⃣ Implement in a Flutter App**

```dart
import 'package:kyc_region_library/kyc_region_library.dart';

void main() {
  String userRegion = RegionSelector.getRegion("US"); // Detect user's region

  runApp(KYCApp(region: userRegion));
}
```

## 5. Deployment Strategy

✅ **AWS Global Accelerator** for multi-region traffic routing.  
✅ **CloudFront for API Gateway** ensuring low-latency connections.  
✅ **Docker & Kubernetes** for region-based microservices.  
✅ **CI/CD Pipelines** (GitHub Actions, Bitbucket Pipelines) for automated deployment.  

## 6. Future Enhancements

- 🔹 **Blockchain-based Identity Verification**  
- 🔹 **AI-powered Fraud Detection Models**  
- 🔹 **Real-time KYC Status Monitoring Dashboard** 

---

## 📌 Authors Asif Ali Kazmi

**Asif and Email : asif786ka@gmail.com**
