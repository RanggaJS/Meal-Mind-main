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

### Struktur Backend

```
backend/
├── app/                           # Direktori utama aplikasi Flask
│   ├── __init__.py                # Inisialisasi aplikasi Flask
│   ├── ml/                        # Modul machine learning
│   │   ├── __init__.py
│   │   ├── advanced_recommendation_engine.py
│   │   ├── diet_progress_analyzer.py
│   │   ├── food_database.py
│   │   ├── model_serializer.py
│   │   ├── recommendation_engine.py
│   │   ├── tensorflow_models.py
│   │   └── tensorflow_ncf_lstm_example.py
│   ├── models/                    # Model database SQLAlchemy
│   │   ├── __init__.py
│   │   ├── food.py
│   │   ├── recommendation.py
│   │   └── user.py
│   ├── routes/                    # Definisi endpoint API
│   │   ├── __init__.py
│   │   ├── activities.py
│   │   ├── auth.py
│   │   ├── profile.py
│   │   ├── progress.py
│   │   ├── recommendations.py
│   │   └── user.py
│   └── utils/                     # Utilitas umum
│       └── __init__.py
│
├── config.py                      # Konfigurasi aplikasi
├── data/                          # Data untuk model dan database
├── migrations/                    # Migrasi database Flask-Migrate
├── models/                        # Model machine learning terlatih
├── scripts/                       # Script utilitas dan inisialisasi
├── Dockerfile                     # Konfigurasi Docker
├── requirements.txt               # Dependensi Python
├── run.py                         # Script utama untuk menjalankan aplikasi
└── berbagai file utilitas lainnya
```

### Struktur Frontend

```
frontend/
├── public/                        # Aset statis publik
├── src/                           # Kode sumber utama
│   ├── assets/                    # Aset seperti gambar, font, dll
│   ├── components/                # Komponen React yang dapat digunakan kembali
│   ├── context/                   # Context API untuk state management
│   ├── pages/                     # Komponen halaman utama
│   ├── services/                  # Layanan API dan integrasi
│   ├── utils/                     # Fungsi utilitas dan helper
│   ├── views/                     # Komponen view alternatif
│   ├── App.jsx                    # Komponen utama aplikasi
│   ├── App.css                    # Styling untuk komponen App
│   ├── index.css                  # Styling global
│   └── main.jsx                   # Entry point aplikasi
│
├── Dockerfile                     # Konfigurasi Docker untuk frontend
├── index.html                     # HTML template utama
├── package.json                   # Dependensi dan script npm
├── tailwind.config.js             # Konfigurasi Tailwind CSS
├── vite.config.js                 # Konfigurasi Vite
└── berbagai file konfigurasi lainnya
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
