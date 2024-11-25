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
   protected $fillable = [
        'nama',
        'npm',
        'prodi',
        'foto',
    ];
```
5. lakukan migration untuk menambahkan tabel mahasiswas pada database dengan command `php artisan migrate`
