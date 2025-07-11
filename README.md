# Praktikum Pemrograman Web 2 – CodeIgniter 4

Repository ini berisi hasil pengerjaan **Modul 4–6** pada mata kuliah *Pemrograman Web 2* menggunakan framework **CodeIgniter 4**.

---

## 📚 Modul 4 – Modul Login dan Auth

### ✅ Fitur:
- Sistem login menggunakan email dan password
- Hash password menggunakan `password_hash()`
- Filter Auth untuk membatasi akses ke halaman admin
- Session login & logout

### 🔐 Akun Sample:
- **Email:** admin@email.com  
- **Password:** admin123

## 🔐 Login & Auth (Modul 4)

Fitur login menggunakan email dan password terenkripsi. Filter `Auth` digunakan untuk melindungi halaman admin agar hanya dapat diakses oleh user yang sudah login.

**Struktur:**
- `app/Controllers/User.php`
- `app/Models/UserModel.php`
- `app/Views/user/login.php`
- `app/Filters/Auth.php`

**Contoh kode login:**
```php
if (password_verify($password, $user['userpassword'])) {
    session()->set([
        'user_id' => $user['id'],
        'user_name' => $user['username'],
        'logged_in' => TRUE
    ]);
    return redirect('admin/artikel');
}
```

**Filter Auth:**
```php
if (!session()->get('logged_in')) {
    return redirect()->to('/user/login');
}
```


### 📷 Screenshot:
_Tampilkan halaman login, session aktif, redirect saat belum login_

---

## 📚 Modul 5 – Pagination dan Pencarian

### ✅ Fitur:
- **Pagination otomatis** pada daftar artikel
- **Pencarian artikel** berdasarkan judul menggunakan `like`

### 🧠 Cuplikan Kode:
```php
// Controller
$q = $this->request->getVar('q') ?? '';
$artikel = $model->like('judul', $q)->paginate(10);
// View
<form method="get">
    <input type="text" name="q" value="<?= $q; ?>" placeholder="Cari artikel">
    <button type="submit">Cari</button>
</form>

<?= $pager->only(['q'])->links(); ?>
📷 Screenshot:
Tampilkan daftar artikel + hasil pencarian + tampilan pagination

## 🖼️ Modul 6 – Upload File Gambar

### ✨ Fitur Utama:
- Form tambah artikel disertai upload gambar
- Validasi judul wajib diisi
- Gambar disimpan di `public/gambar/`
- Nama file disimpan di database, ditampilkan pada halaman artikel

### 📦 Struktur Terkait:
- Controller: `Artikel.php`
- View: `form_add.php`
- Model: `ArtikelModel.php`
- Folder upload: `public/gambar`

### 🧠 Proses Upload File:
```php
$file = $this->request->getFile('gambar');
$file->move(ROOTPATH . 'public/gambar');

$artikel->insert([
    'judul' => $this->request->getPost('judul'),
    'isi' => $this->request->getPost('isi'),
    'slug' => url_title($this->request->getPost('judul')),
    'gambar' => $file->getName(),
    'status' => 1
]);
```

### 🧠 Tampilkan Gambar di View:
```php
<img src="<?= base_url('gambar/' . $row['gambar']); ?>" alt="<?= $row['judul']; ?>" width="300">
```

---

## 🗂️ Struktur Folder Utama

```
/app
  /Controllers
    Artikel.php
    User.php
  /Models
    ArtikelModel.php
    UserModel.php
  /Views
    /artikel
      form_add.php
      admin_index.php
    /user
      login.php
  /Filters
    Auth.php
/public
  /gambar/  ← folder upload gambar
.env
README.md

