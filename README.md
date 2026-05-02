# ⚔️ OtakuQuiz — Uji Pengetahuan Anime & Manga Kamu!

> Platform kuis anime & manga interaktif berbasis web dengan sistem leaderboard global, autentikasi pengguna, dan 450+ soal dari berbagai judul populer.

---

## 🌐 Tech Stack

| Layer | Teknologi | Keterangan |
|---|---|---|
| Frontend | HTML5 + CSS3 + Vanilla JS | Single file SPA, tanpa framework |
| Database | Supabase (PostgreSQL) | Cloud database + REST API |
| Auth | Custom via Supabase REST | Login/register tersimpan di cloud |
| Hosting | GitHub + Vercel | Auto-deploy setiap push ke main |
| Fonts | Google Fonts (CDN) | Bangers (display) + Nunito (body) |

### Frontend
- **Arsitektur:** Single-Page Application (SPA) dalam satu file `index.html` — tanpa build tool, bundler, atau framework
- **CSS:** Custom Properties untuk design system, dark theme, Flexbox + Grid layout, animasi keyframe (confetti, timer, score pop-in), glassmorphism pada card dan modal
- **JavaScript:** Vanilla ES2020+, async/await, localStorage untuk session cache, `performance.now()` + `requestAnimationFrame` untuk timer presisi tinggi, Fisher-Yates shuffle untuk randomisasi soal dan jawaban

### Backend — Supabase
```
Database: PostgreSQL (managed cloud)
Access:   REST API + Anon Key (langsung dari browser)
```

**Tabel:**

| Tabel | Fungsi |
|---|---|
| `users` | Menyimpan akun pengguna (username, password, avatar) |
| `scores` | History lengkap setiap match yang dimainkan |
| `best_scores` | Skor terbaik per player per difficulty — dipakai leaderboard |

**RPC Functions:**
- `upsert_best_score()` — Dipanggil setiap match selesai. Pakai `ON CONFLICT + GREATEST()` sehingga hanya skor tertinggi yang tersimpan, tidak pernah menimpa skor yang lebih baik
- `get_leaderboard(p_difficulty)` — Return top 20 pemain per difficulty, sudah terurut di sisi database — efisien dan akurat tanpa deduplikasi di client

---

## ✨ Fitur

### 🔐 Autentikasi
- **Register** — Pilih username unik, buat password, pilih avatar emoji (🦊🐉⚡🌸🔥🌙⚔️🎭)
- **Login** — Masuk dengan akun yang sudah terdaftar
- **Guest Mode** — Main langsung tanpa akun; skor tidak disimpan ke database
- **Persistensi sesi** — Data login tersimpan di localStorage, tidak perlu login ulang setiap kunjungan

### ⚔️ Difficulty Selector
Sebelum mulai, pemain memilih tingkat kesulitan yang menentukan pool soal:

| Mode | Pool | Deskripsi |
|---|---|---|
| 🎲 Random | 450+ soal | Campuran semua difficulty |
| ⚡ Easy | 167 soal | Fakta dasar, karakter ikonik, judul populer |
| 🔥 Medium | 136 soal | Teknik spesifik, nama kemampuan, detail arc |
| 💀 Hard | 150 soal | Lore mendalam, detail tersembunyi, true otaku only |

### 🎮 Sistem Quiz
- 10 soal per sesi, dipilih acak dari pool difficulty yang dipilih
- Pilihan jawaban (A/B/C/D) diacak tiap soal — posisi jawaban benar tidak selalu sama
- Timer countdown **15 detik** per soal dengan visualisasi lingkaran SVG animasi
- Badge kategori + difficulty ditampilkan di setiap soal
- Progress bar menunjukkan posisi soal saat ini (Q1–Q10)

### ⭐ Sistem Scoring
Poin dihitung dari kombinasi kebenaran + kecepatan menjawab:

```
Base score (benar)   = 100 poin
Time bonus           = sisa waktu × 10 poin  (maks 150 poin)
─────────────────────────────────────────────
Maks per soal        = 250 poin
Maks per sesi        = 2.500 poin  (10 soal)
Jawaban salah/timeout = 0 poin (tidak ada pengurangan)
```

### 🏆 Leaderboard 4-Kelas
Leaderboard dipisah per difficulty — setiap kelas ranking-nya independen:
- 4 tab: 🎲 Random | ⚡ Easy | 🔥 Medium | 💀 Hard
- Setiap tab hanya menampilkan **skor terbaik** per pemain (bukan setiap match)
- Podium visual Top 3 dengan avatar emoji (emas, perak, perunggu)
- Ranking #1–20 dengan nama, avatar, difficulty, jumlah kali main, dan skor terbaik
- Highlight otomatis baris pemain yang sedang login

### 🎉 Result Screen
Muncul instan setelah soal ke-10 (skor disimpan di background, tidak memblokir UI):
- Evaluasi berdasarkan persentase benar: 🏆 OTAKU SEJATI! ≥90% · 🎉 Keren Banget! ≥70% · 👍 Lumayan Nih! ≥50% · 📚 Belajar Lagi Yuk~ <50%
- Total poin, soal benar, soal salah, total waktu bermain
- Confetti animation untuk hasil ≥70%

### 📖 E-Catalogue
Modal yang menampilkan semua kategori soal beserta difficulty badge dan jumlah soal per kategori.

---

## 📚 Bank Soal

**Total: 453 soal** — 167 Easy · 136 Medium · 150 Hard

### Kategori yang Dicakup

| Kategori | Soal | Difficulty |
|---|---|---|
| Fate Series (F/Zero, UBW, HF, Apocrypha) | 45+ | Easy · Medium · Hard |
| Opening Anime | 30+ | Medium · Hard |
| One Piece | 30+ | Easy · Medium · Hard |
| Naruto | 29+ | Easy · Medium · Hard |
| Isekai & Sci-Fi (Re:Zero, SAO, Overlord, Steins;Gate) | 29+ | Medium · Hard |
| Attack on Titan | 24+ | Easy · Medium · Hard |
| My Hero Academia | 22+ | Easy · Medium · Hard |
| Demon Slayer | 21+ | Easy · Medium · Hard |
| Hunter x Hunter | 21+ | Easy · Medium · Hard |
| Jujutsu Kaisen | 20+ | Easy · Medium · Hard |
| Bleach | 20+ | Easy · Medium · Hard |
| Fullmetal Alchemist Brotherhood | 19+ | Easy · Medium · Hard |
| Fairy Tail | 16+ | Easy · Medium · Hard |
| Action (Dragon Ball, One Punch Man) | 13+ | Easy · Medium |
| Sports Anime (Haikyuu!!) | 13+ | Easy · Medium · Hard |
| Trivia & Studio | 12+ | Easy · Medium |
| JoJo's Bizarre Adventure | 11+ | Medium · Hard |
| Death Note · Code Geass · Tokyo Ghoul | 10+ masing-masing | Medium · Hard |
| Manga (Berserk, Vagabond, Vinland Saga, Monster) | 9+ | Medium · Hard |
| Classic Anime (EVA, Cowboy Bebop, Slam Dunk) | 8+ | Easy · Medium · Hard |

---

## 🗄️ Database Setup (Supabase)

Jalankan SQL berikut di Supabase SQL Editor secara berurutan:

```sql
-- 1. Tabel users
CREATE TABLE users (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  username TEXT UNIQUE NOT NULL,
  password TEXT NOT NULL,
  avatar TEXT DEFAULT '🦊',
  created_at TIMESTAMP DEFAULT NOW()
);

-- 2. Tabel scores (history lengkap)
CREATE TABLE scores (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  username TEXT NOT NULL,
  avatar TEXT DEFAULT '🦊',
  score INTEGER NOT NULL,
  correct INTEGER DEFAULT 0,
  total INTEGER DEFAULT 10,
  difficulty TEXT DEFAULT 'random',
  created_at TIMESTAMP DEFAULT NOW()
);

-- 3. Tabel best_scores (untuk leaderboard)
CREATE TABLE best_scores (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  username TEXT NOT NULL,
  avatar TEXT DEFAULT '🦊',
  difficulty TEXT NOT NULL DEFAULT 'random',
  best_score INTEGER NOT NULL DEFAULT 0,
  best_correct INTEGER DEFAULT 0,
  games_played INTEGER DEFAULT 0,
  last_played TIMESTAMP DEFAULT NOW(),
  UNIQUE(username, difficulty)
);

-- 4. Index
CREATE INDEX idx_scores_score ON scores(score DESC);
CREATE INDEX idx_scores_difficulty ON scores(difficulty);
CREATE INDEX idx_best_scores_difficulty_score ON best_scores(difficulty, best_score DESC);

-- 5. RPC: upsert best score
CREATE OR REPLACE FUNCTION upsert_best_score(
  p_username TEXT, p_avatar TEXT, p_score INTEGER,
  p_correct INTEGER, p_difficulty TEXT
)
RETURNS void AS $$
BEGIN
  INSERT INTO best_scores (username, avatar, difficulty, best_score, best_correct, games_played, last_played)
  VALUES (p_username, p_avatar, p_difficulty, p_score, p_correct, 1, NOW())
  ON CONFLICT (username, difficulty)
  DO UPDATE SET
    avatar = EXCLUDED.avatar,
    best_score = GREATEST(best_scores.best_score, EXCLUDED.best_score),
    best_correct = CASE
      WHEN EXCLUDED.best_score > best_scores.best_score THEN EXCLUDED.best_correct
      ELSE best_scores.best_correct END,
    games_played = best_scores.games_played + 1,
    last_played = NOW();
END;
$$ LANGUAGE plpgsql;

-- 6. RPC: get leaderboard
CREATE OR REPLACE FUNCTION get_leaderboard(p_difficulty TEXT)
RETURNS TABLE(username TEXT, avatar TEXT, best_score INTEGER,
              best_correct INTEGER, games_played INTEGER) AS $$
BEGIN
  RETURN QUERY
  SELECT bs.username, bs.avatar, bs.best_score, bs.best_correct, bs.games_played
  FROM best_scores bs
  WHERE bs.difficulty = p_difficulty
  ORDER BY bs.best_score DESC LIMIT 20;
END;
$$ LANGUAGE plpgsql;

-- 7. RLS policies
ALTER TABLE best_scores ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Anyone can read best_scores" ON best_scores FOR SELECT USING (true);
CREATE POLICY "Service can insert best_scores" ON best_scores FOR INSERT WITH CHECK (true);
CREATE POLICY "Service can update best_scores" ON best_scores FOR UPDATE USING (true);
```

---

## 🚀 Deployment

```
1. Clone / fork repo ini
2. Setup Supabase (jalankan SQL di atas)
3. Ganti SUPABASE_URL dan SUPABASE_KEY di index.html dengan milik kamu
4. Push ke GitHub
5. Connect repo ke Vercel → auto-deploy selesai
```

---

## 📁 Struktur Project

```
otakuquiz/
└── index.html        ← Seluruh aplikasi (HTML + CSS + JS dalam satu file)
└── README.md         ← Dokumentasi ini
```

---

## 👤 Author

**Faizal Kurniawan** — XI PPLG A · 2026

---

*OtakuQuiz 2026 — Can you reach the top? 🏆*
