# Laravel Authorization

1. Buat middleware
```
php artisan make:middleware IsAdmin
```
2. Buka file ` IsAdmin.php ` yang ada di folder ` app/http/middleware/IsAdmin.php ` lalu masukan code dibawah pada method ` handle ` 
```php
if(!auth()->check() || !auth()->user()->is_admin) {
  abort(433);
 }
```
3. Daftarkan middleware yang telah dibuat dengan cara buka file dengan nama ` Kernel.php ` yang ada di folder ` app/http/Kernel.php ` lalu masukan kode di bawah ini pada variable ` $routeMeddleware `
```php
admin => \App\Http\Middleware\IsAdmin::class 
```
4. Buka ` web.php ` lalu tambahkan middleware yang telah dibuat ke route yang diinginkan
```php
->middleware('admin');
```
5. Buat ` gates ` dengan cara buka file ` AppServiceProvider.php ` yang ada di folder ` app/Providers/AppServiceProvider.php ` lalu tulis code dibawah ini pada method ` boot `
```php
Gate::define('admin', function(User $user) {
   return $user->is_admin;
});
```
6. Buka file blade yang ingin ditambahkan fitur authorization agar user lain tidak dapat melihat tampilan tersebut kecuali admin, lalu lakukan pengecekan seperti dibawah ini
```html
@can('admin')
  <a href="admin/dashboard">Admin Dashboard</a>
@endcan
```
7. Tambahkan field baru dengan nama is_admin ke dalam table users, lalu masukan code dibawah ini
```php
$table->boolean('is_admin')->default(false);
```
8. Masukan/ganti menjadi angka 1 pada field is_admin jika user tersebut adalah admin
