# MealMind Backend Documentation

## Directory Structure

```
backend/
├── app/                           # Main Flask application directory
│   ├── __init__.py                # Flask app initialization
│   ├── ml/                        # Machine learning modules
│   │   ├── __init__.py
│   │   ├── advanced_recommendation_engine.py
│   │   ├── diet_progress_analyzer.py
│   │   ├── food_database.py
│   │   ├── model_serializer.py
│   │   ├── recommendation_engine.py
│   │   ├── tensorflow_models.py
│   │   └── tensorflow_ncf_lstm_example.py
│   ├── models/                    # SQLAlchemy database models
│   │   ├── __init__.py
│   │   ├── food.py
│   │   ├── recommendation.py
│   │   └── user.py
│   ├── routes/                    # API endpoint definitions
│   │   ├── __init__.py
│   │   ├── activities.py
│   │   ├── auth.py
│   │   ├── profile.py
│   │   ├── progress.py
│   │   ├── recommendations.py
│   │   └── user.py
│   └── utils/                     # Common utilities
│       └── __init__.py
│
├── config.py                      # Application configuration
├── data/                          # Data for models and database
├── migrations/                    # Flask-Migrate database migrations
├── models/                        # Trained machine learning models
├── scripts/                       # Utility and initialization scripts
├── Dockerfile                     # Docker configuration
├── requirements.txt               # Python dependencies
├── run.py                         # Main application runner
└── various other utility files
```

## API Structure

### Authentication (`/api/auth`)
- `POST /signup` - Register a new user
- `POST /login` - Login user and get JWT token
- `GET /me` - Get current user information

### User Profile (`/api/profile`)
- `POST /setup` - Create new user profile
- `GET /get` - Get user profile
- `PUT /update` - Update user profile
- `POST /reset` - Reset user profile

### Diet Recommendations (`/api/recommendations`)
- `GET /today` - Get today's food and activity recommendations
- `POST /regenerate/<meal_type>` - Regenerate recommendation for specific meal type
- `POST /checkin` - Daily check-in to track diet adherence
- `GET /history` - Get recommendation history
- `GET /month/<year>/<month>` - Get recommendations for specific month
- `GET /day/<date_str>` - Get recommendations for specific date
- `POST /generate_for_date` - Generate recommendations for specific date
- `POST /generate_month_ahead` - Generate recommendations for one month ahead

### Diet Progress (`/api/progress`)
- `GET /analysis` - Get comprehensive diet progress analysis
- `POST /weight/record` - Record new weight measurement
- `GET /nutritional-balance` - Get nutritional balance analysis
- `GET /adherence` - Get diet plan adherence analysis
- `GET /calorie-adjustment` - Get calorie adjustment suggestions

### Activities (`/api/activities`)
- `GET /list` - Get list of available physical activities

### User (`/api/user`)
- `GET /me` - Get user information
- `GET /stats` - Get user statistics

## Authentication Implementation

### JWT Protection

Most endpoints are protected with JWT using the `@jwt_required()` decorator, ensuring only authenticated users can access them. JWT tokens are created during login and then sent in the Authorization header for subsequent requests.

### Example Login Implementation

```python
@auth_bp.route('/login', methods=['POST'])
def login():
    try:
        data = request.get_json()
        
        if not data.get('email') or not data.get('password'):
            return jsonify({'error': 'Missing email or password'}), 400
        
        user = User.query.filter_by(email=data['email']).first()
        
        if not user:
            return jsonify({'error': 'Invalid credentials'}), 401
            
        if user and user.check_password(data['password']):
            # Convert user ID to string to avoid JWT subject type errors
            access_token = create_access_token(identity=str(user.id))
            
            # Check if user has profile
            has_profile = UserProfile.query.filter_by(user_id=user.id).first() is not None
            
            return jsonify({
                'access_token': access_token,
                'user': user.to_dict(),
                'has_profile': has_profile
            }), 200
        
        return jsonify({'error': 'Invalid credentials'}), 401
        
    except Exception as e:
        return jsonify({'error': str(e)}), 500
```

### Example Protected Endpoint

```python
@profile_bp.route('/get', methods=['GET'])
@jwt_required()  # Decorator to protect endpoint
def get_profile():
    try:
        # Get user ID from JWT token
        user_id = get_jwt_identity()
        
        # Access profile data based on user_id
        profile = UserProfile.query.filter_by(user_id=user_id).first()
        
        if not profile:
            return jsonify({'error': 'Profile not found'}), 404
        
        return jsonify({'profile': profile.to_dict()}), 200
        
    except Exception as e:
        return jsonify({'error': str(e)}), 500
```

## Blueprint Registration

```python
# Import blueprints
from app.routes.auth import auth_bp
from app.routes.profile import profile_bp
from app.routes.recommendations import recommendations_bp
from app.routes.activities import activities_bp
from app.routes.user import user_bp
from app.routes.progress import progress_bp

# Register blueprints
app.register_blueprint(auth_bp, url_prefix='/api/auth')
app.register_blueprint(profile_bp, url_prefix='/api/profile')
app.register_blueprint(recommendations_bp, url_prefix='/api/recommendations')
app.register_blueprint(activities_bp, url_prefix='/api/activities')
app.register_blueprint(user_bp, url_prefix='/api/user')
app.register_blueprint(progress_bp, url_prefix='/api/progress')
``` 

# 🍽️ MealMind - Aplikasi Perencanaan Diet Cerdas

[![Status](https://img.shields.io/badge/status-active-success.svg)]()
[![License](https://img.shields.io/badge/license-MIT-blue.svg)]()

<p align="center">
  <img src="docs/logo.png" alt="MealMind Logo" width="200" height="200" style="max-width: 100%;">
</p>

## 📋 Deskripsi

**MealMind** adalah aplikasi diet cerdas berbasis machine learning yang memberikan rekomendasi makanan, pelacakan nutrisi, dan saran aktivitas fisik secara personal sesuai kebutuhan dan preferensi pengguna. Aplikasi ini dirancang untuk membantu pengguna mencapai tujuan diet mereka dengan cara yang sehat dan berkelanjutan.

## ✨ Fitur Utama

- **🧠 Rekomendasi Makanan Cerdas** - Dapatkan saran makanan berdasarkan preferensi, alergi, dan tujuan diet Anda
- **📊 Pelacakan Nutrisi Otomatis** - Monitor asupan kalori, protein, karbohidrat, dan lemak harian
- **📈 Analisis Progress Diet** - Lihat perkembangan diet Anda dari waktu ke waktu dengan grafik intuitif
- **📆 Perencanaan Meal Mingguan** - Susun rencana makan mingguan dengan rekomendasi otomatis
- **🏃 Saran Aktivitas Fisik** - Terima rekomendasi olahraga yang sesuai dengan tujuan dan kondisi fisik Anda
- **🔔 Pengingat Check-in** - Dapatkan notifikasi untuk check-in harian dan jadwal makan
- **📱 Antarmuka Responsif** - Akses aplikasi dengan nyaman dari berbagai perangkat

## 🛠️ Teknologi

### Backend

- **Flask** - Python web framework
- **SQLite** - Database ringan
- **Pandas** - Analisis data
- **Scikit-learn** - Machine learning untuk rekomendasi diet

### Frontend

- **React** - Framework JavaScript untuk UI
- **Tailwind CSS** - Framework CSS untuk styling
- **Chart.js** - Visualisasi data dan grafik progress
- **Axios** - HTTP client untuk API requests

## 🚀 Instalasi dan Penggunaan

### Prasyarat

- Python 3.8+
- Node.js 14+
- npm atau yarn

### Langkah-langkah Instalasi

#### Backend

```bash
# Clone repository
git clone https://github.com/yourusername/meal-mind-project.git
cd meal-mind-project/backend

# Buat virtual environment
python -m venv venv

# Aktifkan virtual environment
# Windows
venv\Scripts\activate
# Linux/Mac
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Siapkan database
flask db init
flask db migrate
flask db upgrade

# Jalankan server
flask run
```

#### Frontend

```bash
# Pindah ke direktori frontend
cd ../frontend

# Install dependencies
npm install

# Jalankan aplikasi
npm run dev
```

## 📷 Screenshots

<div align="center">
  <img src="docs/screenshot-dashboard.png" alt="Dashboard" width="45%">
  <img src="docs/screenshot-meal-plan.png" alt="Meal Planning" width="45%">
  <br><br>
  <img src="docs/screenshot-progress.png" alt="Progress Tracking" width="45%">
  <img src="docs/screenshot-recommendations.png" alt="Food Recommendations" width="45%">
</div>

## 🧪 Struktur Proyek

```
meal-mind-project/
├── backend/
│   ├── app/
│   │   ├── models/       # Model database
│   │   ├── routes/       # API endpoints
│   │   └── ml/           # Algoritma machine learning
│   ├── migrations/       # Database migrations
│   └── instance/         # Instance database
├── frontend/
│   ├── public/           # Aset statis
│   └── src/
│       ├── components/   # Komponen React
│       ├── hooks/        # Custom hooks
│       └── pages/        # Halaman aplikasi
└── docs/                 # Dokumentasi
```

## 🤝 Kontribusi

Kontribusi sangat diterima dan diharapkan! Berikut adalah langkah-langkah untuk berkontribusi:

1. Fork repository
2. Buat branch fitur baru (`git checkout -b feature/AmazingFeature`)
3. Commit perubahan Anda (`git commit -m 'Add some AmazingFeature'`)
4. Push ke branch (`git push origin feature/AmazingFeature`)
5. Buka Pull Request

## 📝 Lisensi

Proyek ini dilisensikan di bawah Lisensi MIT - lihat file [LICENSE](LICENSE) untuk detailnya.

## 📞 Kontak

Nama Anda - [@twitter_handle](https://twitter.com/twitter_handle) - email@example.com

Link Proyek: [https://github.com/yourusername/meal-mind-project](https://github.com/yourusername/meal-mind-project)

---

<p align="center">
  Dibuat dengan ❤️ oleh Tim MealMind
</p>
# Meal-Mind
