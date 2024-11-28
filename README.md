### Setup Migration + Model + Controller untuk Mahasiswa
1. pada terminal masukan command `php artisan make:model Mahasiswa -mc -r` untuk menggenerate migration & model tabel mahasiswa dan MahasiswaController beserta resource nya
2. buka file migration mahasiswa pada `database\migrations\xxxx_create_mahasiswas_table.php` masukan query berikut untuk menambahkan isi tabel mahasiswas ketika dilakuakn migration
```
   public function up(): void
    {
        Schema::create('mahasiswas', function (Blueprint $table) {
            $table->id();
            $table->string('nama');
            $table->integer('npm');
            $table->string('prodi');
            $table->string('foto')->nullable();
            $table->timestamps();
        });
    }
```
3. pada file model Mahasiswa.php masukan query berikut
```
    use HasFactory;
    protected $table = 'mahasiswas';
    protected $fillable = [
        'nama',
        'npm',
        'prodi',
        'foto',
    ];
```
5. lakukan migration untuk menambahkan tabel mahasiswas pada database dengan command `php artisan migrate`

=================
### Setup Halaman Mahasiswa
1. buat folder mahasiswa dan file index.blade.php pada folder views, masukan kode berikut untuk tampilan halaman mahasiswa
```
<x-app-layout>
    <x-slot name="header">
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            {{ __('Mahasiswa') }}
        </h2>
    </x-slot>

    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6 text-gray-900">
                    <div class="d-flex justify-content-between align-items-center mb-3">
                        <div class="ml-auto d-flex">
                            <a href="#" class="btn btn-primary mr-2">Tambah Mahasiswa</a>
                            <form action="#" method="GET" class="d-flex">
                                <input type="text" name="search" class="form-control" placeholder="Pencarian"
                                    value="#">
                                <button class="btn btn-primary ml-2" type="submit">
                                    <i class="bi bi-search"></i>
                                </button>
                            </form>
                        </div>
                    </div>

                    <table class="table table-hover">
                        <thead class="table-primary">
                            <tr>
                                <th>No</th>
                                <th>Nama</th>
                                <th>NPM</th>
                                <th>Program Studi</th>
                                <th>Foto</th>
                                <th>Aksi</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td></td>
                                <td></td>
                                <td></td>
                                <td></td>
                                <td>
                                    <img src="3">
                                </td>
                                <td>
                                    <a href="#" class="btn btn-secondary">Edit</a>
                                    <a href="#" type="button" class="btn btn-danger">Hapus</a>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</x-app-layout>
```
2. pada file `ProdiController` masukan kode untuk function index halaman mahasiswa
```
    public function index()
    {
        return view('mahasiswa.index');
    }
```
3. buat route ke halaman mahasiswa `web.php`
```
Route::get('/mahasiswa', [MahasiswaController::class, 'index'])->name('/mahasiswa');
```
4. agar halaman mahasiswa masuk ke menu navigasi tambahkan pada file `navigation.blade.php`, tambahkan kode berikut pada bagian navigation links
```
<x-nav-link :href="route('/mahasiswa')" :active="request()->routeIs('/mahasiswa')">
{{ __('Mahasiswa') }}
</x-nav-link>
```