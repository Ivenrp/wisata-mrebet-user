# Wisata Mrebet — Migrasi ke Laravel 13

Proyek ini adalah hasil migrasi landing page statis (`index.html` + `css/style.css` +
`js/script.js`) ke struktur resmi **Laravel 13** (di-clone langsung dari skeleton
`laravel/laravel` cabang `13.x` di GitHub). Tampilan dan perilaku halaman **100%
identik** dengan versi statis — tidak ada baris konten, markup, CSS, atau JS yang
diubah. Yang berubah hanya cara file di-serve:

| Statis (lama)              | Laravel (baru)                              |
|-----------------------------|----------------------------------------------|
| `index.html`                 | `resources/views/welcome.blade.php`          |
| `css/style.css`               | `public/css/style.css`                       |
| `js/script.js`                | `public/js/script.js`                        |
| `assets/img/*.jpg`            | `public/assets/img/*.jpg`                    |
| dibuka lewat file/browser     | di-serve lewat route `Route::get('/', ...)`  |

Satu-satunya perubahan teks di dalam `welcome.blade.php` adalah path asset yang
dibungkus helper Laravel `asset()`, misalnya:

```blade
<link rel="stylesheet" href="{{ asset('css/style.css') }}" />
<img src="{{ asset('assets/img/hero.jpg') }}" ...>
<script src="{{ asset('js/script.js') }}"></script>
```

Ini sudah diverifikasi baris-per-baris (di-diff otomatis) terhadap `index.html`
asli — tidak ada perubahan lain selain path tersebut.

## Kenapa belum ada folder `vendor/`?

Sandbox tempat saya bekerja tidak punya akses jaringan ke **packagist.org** (hanya
domain tertentu yang di-whitelist), jadi `composer install` tidak bisa dijalankan
di sini. Semua file inti Laravel 13 (struktur `app/`, `bootstrap/`, `config/`,
`routes/`, dll) sudah asli dari repo resmi `laravel/laravel@13.x`, jadi begitu
kamu jalankan `composer install` di komputer/servermu sendiri, proyek ini akan
jalan normal seperti proyek Laravel 13 pada umumnya.

## Cara menjalankan

```bash
cd wisata-mrebet-laravel
composer install
cp .env.example .env
php artisan key:generate
touch database/database.sqlite   # DB tidak dipakai halaman ini, tapi Laravel butuh koneksi default
php artisan serve
```

Buka `http://127.0.0.1:8000` — hasilnya harus identik dengan `index.html` asli.

> Catatan: `npm install` / build Vite **tidak diperlukan** karena halaman ini
> memakai CSS & JS statis langsung dari folder `public/`, bukan lewat pipeline
> Vite bawaan Laravel. Folder `resources/css/app.css` dan `resources/js/app.js`
> bawaan skeleton dibiarkan ada (tidak dipakai) kalau suatu saat kamu mau
> tambah Tailwind/build asset di halaman lain.

## Struktur route

```php
// routes/web.php
Route::get('/', function () {
    return view('welcome');
});
```

Kalau nanti mau nambah halaman lain, tinggal buat view baru di
`resources/views/` dan route baru di `routes/web.php`.
