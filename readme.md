# Lab Management Software

A comprehensive web-based laboratory management system designed for educational institutions to manage equipment inventory, track allotments, handle requests, and schedule events.

## 🎯 Overview

This Lab Management Software is built specifically for ATL (Atal Tinkering Lab) at Airforce School Hebbal. It provides a complete solution for managing lab resources, equipment tracking, student requests, and event scheduling through an intuitive web interface.

## ✨ Features

### 1. **Dashboard**
- Real-time overview of lab operations
- Pending requests counter
- Event statistics (today, this week, this month)
- Low stock equipment alerts
- Google Meet integration for scheduled events
- What's New section for announcements
- Report generation (CSV and PDF formats)

### 2. **Equipment Management**
- Complete inventory tracking with item codes
- Equipment specifications and location tracking
- Picture URL storage for visual reference
- Quantity monitoring with low-stock alerts
- Search and filter functionality
- Add, edit, and delete equipment records
- Bulk import via CSV files

### 3. **Allotment System**
- Track equipment issued to students/staff
- Monitor active, consumed, and pending allotments
- Issue and return tracking
- Renewal management
- Automatic quantity adjustment on consumption

### 4. **Request Management**
- Student/staff equipment request submission
- Approval/denial workflow
- Email notifications (configurable)
- Request status tracking (pending, approved, denied)
- Date-based request logging

### 5. **Event Calendar**
- Interactive event scheduling
- Event details: time, venue, mode, incharge, mentor
- Visual calendar interface with date ranges
- Event editing and deletion
- Google Meet link integration

### 6. **Public Home Page**
- Equipment browsing for students
- Event calendar viewing
- Scheduled meeting information
- Chatbot integration (Landbot)
- What's New announcements

## 🛠️ Technology Stack

### Backend
- **Framework**: Flask 2.0.3
- **Database**: PostgreSQL (with SQLAlchemy ORM)
- **Database Driver**: psycopg2 2.9.3
- **PDF Generation**: FPDF 1.7.2
- **Date/Time**: Pendulum 2.1.2

### Frontend
- **HTML/CSS/JavaScript**
- **Template Engine**: Jinja2 3.1.2
- **Calendar**: Evo-calendar (custom CSS/JS)
- **Icons**: Font Awesome
- **Chatbot**: Landbot integration

### Deployment
- **Server**: Gunicorn 20.1.0
- **Platform**: Heroku (configured with Procfile)
- **Python Version**: 3.10.5

## 📋 Prerequisites

- Python 3.10.5 or higher
- PostgreSQL database
- pip (Python package manager)

## 🚀 Installation

### 1. Clone the Repository
```bash
git clone <repository-url>
cd "Lab Management Software"
```

### 2. Install Dependencies
```bash
pip install -r requirements.txt
```

### 3. Database Configuration

Edit `app.py` to configure your database:

**For Development (Local PostgreSQL):**
```python
ENV = 'dev'
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql://username:password@localhost/database_name'
```

**For Production:**
```python
ENV = 'prod'
app.config['SQLALCHEMY_DATABASE_URI'] = 'your_production_database_uri'
```

### 4. Initialize Database

The application uses the following database models:
- `Users` - Authentication
- `Equipments` - Inventory management
- `Requests` - Equipment requests
- `Allotment` - Equipment allocation tracking
- `Calendar` - Event scheduling
- `Gmeet` - Google Meet links
- `Whatnew` - Announcements

### 5. Run the Application

**Development:**
```bash
python app.py
```

**Production (Heroku):**
```bash
gunicorn app:app
```

The application will be available at `http://localhost:5000`

## 📁 Project Structure

```
Lab Management Software/
├── app.py                      # Main application file
├── requirements.txt            # Python dependencies
├── Procfile                    # Heroku deployment config
├── runtime.txt                 # Python version specification
├── static/                     # Static assets
│   ├── css/                    # Stylesheets
│   ├── js/                     # JavaScript files
│   └── *.png, *.jpg, *.gif     # Images
├── templates/                  # HTML templates
│   ├── base.html              # Base template
│   ├── home.html              # Public home page
│   ├── entry.html             # Login page
│   ├── dashboard.html         # Admin dashboard
│   ├── inventory.html         # Equipment management
│   ├── equipments.html        # Public equipment view
│   ├── allotment.html         # Allotment tracking
│   ├── requests.html          # Request management
│   ├── calendar.html          # Event calendar (admin)
│   └── calendarn.html         # Event calendar (public)
└── README.md                  # This file
```

## 🔐 Authentication

The system uses session-based authentication:
- Login required for admin features
- Public access to home page and equipment browsing
- Session management with Flask sessions

**Default Login Route:** `/`

## 📊 Database Schema

### Users Table
- `username` (Primary Key)
- `password`

### Equipments Table
- `item_code` (Primary Key)
- `name`
- `quantity`
- `location`
- `paddress` (picture URL)
- `specifications`
- `extras`

### Requests Table
- `request_id` (Primary Key)
- `item_code`
- `item_name`
- `email_id`
- `date_of_request`
- `status`

### Allotment Table
- `request_id` (Primary Key)
- `item_code`
- `issued_to`
- `issued_by`
- `original_DOI`
- `status`
- `renewed_on`
- `renewed_by`

### Calendar Table
- `event_name` (Primary Key)
- `start_time`, `end_time`
- `incharge`, `guest`
- `mode`, `venue`
- `start_date`, `end_date`
- `description`

### Gmeet Table
- `event_name` (Primary Key)
- `meet_link`

### Whatnew Table
- `id` (Primary Key)
- `type`
- `text`
- `link`

## 🎨 Key Features Explained

### CSV Import
Upload CSV files to bulk import equipment data. Required columns:
- item_code
- name
- quantity
- location
- paddress
- specifications
- extras

### Report Generation
Generate reports in two formats:
- **CSV**: Simple comma-separated values
- **Fancy PDF**: Formatted PDF with branding

### Low Stock Alerts
Automatically displays equipment with quantity ≤ 5 on the dashboard, accounting for active and consumed allotments.

### Event Scheduling
- Date range support
- Time slots
- Mode (online/offline)
- Venue information
- Incharge and mentor assignment

## 🔧 Configuration

### Secret Key
Update the secret key in `app.py`:
```python
app.config['SECRET_KEY'] = "your-secret-key-here"
```

### Email Configuration
Email functionality is present but commented out. To enable:
1. Uncomment email code in `update_req` function
2. Configure SMTP settings
3. Update sender credentials

## 🌐 Deployment

### Heroku Deployment
The project is configured for Heroku deployment:

1. Create a Heroku app
2. Add PostgreSQL addon
3. Set environment variables
4. Deploy:
```bash
git push heroku main
```

### Environment Variables
- Database URL is automatically configured by Heroku
- Update `ENV` variable in `app.py` to 'prod'

## 📱 Routes

### Public Routes
- `/` - Login page
- `/home` - Public home page
- `/equipments` - Browse equipment
- `/atlcalendar` - View events

### Admin Routes (Authentication Required)
- `/dashboard` - Admin dashboard
- `/inventory` - Manage inventory
- `/allotment` - Track allotments
- `/requests` - Manage requests
- `/calendar` - Manage events
- `/report` - Generate reports

## 🤝 Contributing

This is an educational project. To contribute:
1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## 📝 License

This project is developed for educational purposes at Airforce School Hebbal.

## 👥 Authors

Developed for ATL (Atal Tinkering Lab) at Airforce School Hebbal, Bangalore.

## 🐛 Known Issues

- Email notification feature is disabled (commented out)
- Some hardcoded credentials should be moved to environment variables
- Session security can be enhanced

## 🔮 Future Enhancements

- [ ] User role management (admin, teacher, student)
- [ ] Advanced reporting with charts
- [ ] Mobile responsive design improvements
- [ ] Equipment maintenance tracking
- [ ] QR code generation for equipment
- [ ] Advanced search and filtering
- [ ] Export to Excel format
- [ ] Email notification system activation
- [ ] Equipment reservation system
- [ ] Damage/repair tracking

## 📞 Support

For issues and questions, please contact the lab administrator at Airforce School Hebbal.

---

**Note**: This software is designed specifically for laboratory management in educational institutions and can be customized for other similar use cases.
