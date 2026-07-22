# 📦 Boxy - Modern Parcel Delivery & Logistics Web Application

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-3.0.0-000000?style=for-the-badge&logo=flask&logoColor=white)](https://flask.palletsprojects.com/)
[![MySQL](https://img.shields.io/badge/MySQL-8.0%2B-4479A1?style=for-the-badge&logo=mysql&logoColor=white)](https://www.mysql.com/)
[![Bootstrap](https://img.shields.io/badge/Bootstrap-5.3-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white)](https://getbootstrap.com/)
[![Razorpay](https://img.shields.io/badge/Razorpay-Integration-0C2340?style=for-the-badge&logo=razorpay&logoColor=blue)](https://razorpay.com/)
[![License](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)](LICENSE)

**Boxy** is a full-stack, responsive parcel delivery and logistics web application. Built with **Flask**, **MySQL**, **Bootstrap 5**, and **Vanilla JavaScript**, Boxy simplifies shipment management with multi-parcel booking, multi-stop drop-offs, real-time parcel tracking, Razorpay payment gateway integration, automated SMTP email updates, and an interactive Delivery Partner portal.

---

## 🚀 Key Features

- 📦 **Multi-Parcel & Multi-Stop Delivery Booking**:
  - Book single or multiple packages under a single delivery ID.
  - Specify custom dimensions, weights, parcel types (Documents, Electronics, Clothing, etc.), and special handling instructions.
  - Multi-stop drop-off support with individual receiver contact details and addresses.

- 📍 **Real-Time Parcel Tracking**:
  - Track shipments instantly via unique Booking IDs.
  - Visual status timeline tracking package stages: `Available` ➔ `Accepted` ➔ `Picked Up` ➔ `On The Way` ➔ `Delivered`.

- 💳 **Flexible Payments & Razorpay Integration**:
  - Seamless online payment gateway integration powered by **Razorpay**.
  - Support for **Cash on Delivery (COD)** with status verification.
  - Automatic invoice/receipt status tracking (`Pending`, `Paid`, `Pending Cash`).

- 📧 **Automated SMTP Email Notifications**:
  - Instant booking confirmation sent to senders with tracking links.
  - Real-time email notifications sent to receivers upon dispatch and delivery.
  - HTML email templates powered by Python's `smtplib`.

- 🚚 **Delivery Partner Portal**:
  - Dedicated driver login and onboarding interface.
  - Live status toggle (`Online` / `Offline`).
  - View available deliveries in real-time, accept assignments, and update package delivery progress step-by-step.

- 📊 **Admin Panel**:
  - Centralized dashboard for managing partners, deliveries, system statistics, and pending approvals.

- 📱 **Modern & Responsive UI/UX**:
  - Sleek glassmorphism aesthetics with customizable dark/light theme accents.
  - Mobile-first layout using Bootstrap 5 and FontAwesome iconography.

---

## 🛠️ Tech Stack

| Layer | Technology |
| :--- | :--- |
| **Backend** | Python 3.10+, Flask 3.0.0, Werkzeug 3.0.1 |
| **Database** | MySQL Server 8.0+ (`mysql-connector-python`) |
| **Frontend** | HTML5, CSS3 (Custom Glassmorphism & Animations), JavaScript (ES6+) |
| **UI Framework** | Bootstrap 5.3.2, Font Awesome 6.5.1 |
| **Payments** | Razorpay Checkout API Integration |
| **Mailing** | SMTP Email Service (Gmail TLS Integration) |

---

## 🗄️ Database Architecture

The system uses a relational MySQL database structure optimized for high throughput parcel tracking and multi-stop logistics operations:

```mermaid
erDiagram
    PARTNERS ||--o{ DELIVERIES : "accepts & fulfills"
    DELIVERIES ||--|{ DELIVERY_STOPS : "has multiple drop-offs"
    DELIVERIES ||--|{ PARCELS : "contains multiple items"

    PARTNERS {
        varchar id PK
        string first_name
        string last_name
        string email UK
        string phone
        string vehicle_type
        string vehicle_number
        enum status "'online', 'offline'"
        boolean approved
    }

    DELIVERIES {
        varchar id PK
        string sender_name
        text sender_address
        string sender_email
        string receiver_name
        text receiver_address
        string receiver_phone
        enum status "'available', 'accepted', 'picked', 'on_the_way', 'delivered'"
        varchar partner_id FK
        decimal total_amount
        enum payment_status "'pending', 'paid', 'pending_cash'"
        enum payment_method "'online', 'cash'"
    }

    DELIVERY_STOPS {
        int id PK
        varchar booking_id FK
        int stop_number
        text drop_address
        string receiver_name
        string receiver_phone
        enum status "'pending', 'delivered'"
    }

    PARCELS {
        int id PK
        varchar booking_id FK
        int parcel_number
        string parcel_name
        string parcel_type
        decimal weight
        string size
    }
