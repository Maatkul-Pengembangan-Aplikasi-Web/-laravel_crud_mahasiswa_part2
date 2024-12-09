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
2. pada file `MahasiswaController` masukan kode untuk function index halaman mahasiswa
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

=================
### Setup Halaman Mahasiswa - Fungsi Tambah Data (add)
1. buat file `create.blade.php` pada folder mahasiswa, masukan kode berikut
```
<x-app-layout>
    <x-slot name="header">
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            {{ __('Tambah Data Mahasiswa') }}
        </h2>
    </x-slot>

    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            @if (session()->has('error'))
                <div>
                    {{ session('error') }}
                </div>
            @endif
            <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6 text-gray-900">
                    <form action="{{ route('mahasiswa/save') }}" method="POST" enctype="multipart/form-data">
                        @csrf
                        <div class="row mb-3">
                            <div class="col"> Nama Lengkap
                                <input type="text" name="nama" class="form-control"
                                    placeholder="Aa Herdi Prayoga">
                                @error('nama')
                                    <span class="text-danger">{{ $message }}</span>
                                @enderror
                            </div>
                        </div>
                        <div class="row mb-3">
                            <div class="col"> NPM
                                <input type="number" name="npm" class="form-control" placeholder="191410001">
                                @error('npm')
                                    <span class="text-danger">{{ $message }}</span>
                                @enderror
                            </div>
                        </div>
                        <div class="row mb-3">
                            <div class="col">
                                <label for="prodi" class="form-label">Program Studi</label>
                                <select name="prodi" class="form-control" id="prodi">
                                    @foreach ($prodis as $prodi)
                                        <option value="{{ $prodi->nama }}">{{ $prodi->nama }}</option>
                                    @endforeach
                                </select>
                                @error('prodi')
                                    <span class="text-danger">{{ $message }}</span>
                                @enderror
                            </div>
                        </div>
                        <div class="row mb-3">
                            <div class="col"> Pas Foto
                                <input type="file" name="foto" class="form-control">
                                @error('foto')
                                    <span class="text-danger">{{ $message }}</span>
                                @enderror
                            </div>
                        </div>

                        <div class="row">
                            <div class="d-grid">
                                <button class="btn btn-primary">Simpan</button>
                            </div>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</x-app-layout>
```
2. edit file `index.blade.php` pada folder mahasiswa
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
                        @if (Session::has('success'))
                            <div class="alert alert-success">
                                {{ Session::get('success') }}
                            </div>
                        @endif
                        <div class="ml-auto d-flex">
                            <a href="{{ route('mahasiswa/create') }}" class="btn btn-primary mr-2">Tambah Mahasiswa</a>
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
                            @foreach ($mahasiswas as $mahasiswa)
                                <tr>
                                    <td>{{ $loop->iteration }}</td>
                                    <td>{{ $mahasiswa->nama }}</td>
                                    <td>{{ $mahasiswa->npm }}</td>
                                    <td>{{ $mahasiswa->prodi }}</td>
                                    <td>
                                        @if ($mahasiswa->foto)
                                            <img src="{{ asset('fotomahasiswa/' . $mahasiswa->foto) }}"
                                                alt="{{ $mahasiswa->nama }}" width="100">
                                        @else
                                            Tidak ada Fotonya
                                        @endif
                                    </td>
                                    <td>
                                        <a href="#" class="btn btn-secondary">Edit</a>
                                        <a href="#" type="button" class="btn btn-danger">Hapus</a>
                                    </td>
                                </tr>
                            @endforeach
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</x-app-layout>
```
3. tambahkan function create dan save pada `MahasiswaController.php` sebelum itu tambahkan juga model Prodi
```
use App\Models\Prodi;
```
```
    public function create()
    {
        $prodis = Prodi::all();
        return view('mahasiswa.create', compact('prodis'));
    }

    public function save(Request $request)
    {
        $validation = $request->validate([
            'nama' => 'required',
            'npm' => 'required|numeric',
            'prodi' => 'required',
            'foto' => 'nullable|image|mimes:jpeg,png,jpg,gif,svg|max:2048',
        ]);

        if ($request->hasFile('foto')) {
            $namaFoto = $request->npm . '.' . $request->foto->extension();
            $request->foto->move(public_path('fotomahasiswa'), $namaFoto);
            $validation['foto'] = $namaFoto;
        }

        $mahasiswa = Mahasiswa::create($validation);

        if ($mahasiswa) {
            session()->flash('success', 'Data Mahasiswa Berhasil di Tambahkan');
            return redirect(route('/mahasiswa'));
        } else {
            session()->flash('error', 'Ada Kesalahan');
            return redirect(route('mahasiswa/create'));
        }
    }
```
4. tambahkan route pada `web.php` 
```
Route::get('/mahasiswa/create', [MahasiswaController::class, 'create'])->name('mahasiswa/create');
Route::post('/mahasiswa/save', [MahasiswaController::class, 'save'])->name('mahasiswa/save');
```
=================
### Setup Halaman Mahasiswa - Fungsi Edit Data (edit)
1. buat file `edit.blade.php` pada folder mahasiswa, masukan kode berikut
```
<x-app-layout>
    <x-slot name="header">
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            {{ __('Tambah Data Mahasiswa') }}
        </h2>
    </x-slot>

    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            @if (session()->has('error'))
                <div>
                    {{ session('error') }}
                </div>
            @endif
            <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6 text-gray-900">
                    <form action="{{ route('mahasiswa/update', $mahasiswa->id) }}" method="POST"
                        enctype="multipart/form-data">
                        @csrf
                        @method('PUT')
                        <div class="row mb-3">
                            <div class="col"> Nama Lengkap
                                <input type="text" name="nama" class="form-control" placeholder="Aa Herdi Prayoga"
                                    value="{{ $mahasiswa->nama }}">
                                @error('nama')
                                    <span class="text-danger">{{ $message }}</span>
                                @enderror
                            </div>
                        </div>

                        <div class="row mb-3">
                            <div class="col"> NPM
                                <input type="number" name="npm" class="form-control" placeholder="191410001"
                                    value="{{ $mahasiswa->npm }}">
                                @error('npm')
                                    <span class="text-danger">{{ $message }}</span>
                                @enderror
                            </div>
                        </div>

                        <div class="row mb-3">
                            <div class="col">
                                <label for="prodi" class="form-label">Program Studi</label>
                                <select name="prodi" class="form-control" id="prodi">
                                    @foreach ($prodis as $prodi)
                                        <option value="{{ $prodi->nama }}"
                                            {{ $mahasiswa->prodi == $prodi->nama ? 'selected' : '' }}>
                                            {{ $prodi->nama }}
                                        </option>
                                    @endforeach
                                </select>
                                @error('prodi')
                                    <span class="text-danger">{{ $message }}</span>
                                @enderror
                            </div>
                        </div>

                        @if ($mahasiswa->foto)
                            <div class="mb-3">
                                <img src="{{ asset('fotomahasiswa/' . $mahasiswa->foto) }}" width="100">
                            </div>
                        @endif
                        <div class="row mb-3">
                            <div class="col"> Pas Foto
                                <input type="file" name="foto" class="form-control">
                                @error('foto')
                                    <span class="text-danger">{{ $message }}</span>
                                @enderror
                            </div>
                        </div>

                        <div class="row">
                            <div class="d-grid">
                                <button class="btn btn-primary">Simpan</button>
                            </div>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</x-app-layout>
```
2. edit file `index.blade.php` pada folder mahasiswa bagian tombol edit
```
<a href="{{ route('mahasiswa/edit', $mahasiswa->id) }}"class="btn btn-secondary">Edit</a>
```

3. tambahkan function edit dan update pada `MahasiswaController.php`
```
    public function edit($id)
    {
        $mahasiswa = Mahasiswa::findOrFail($id);
        $prodis = Prodi::all();
        return view('mahasiswa.edit', compact('mahasiswa', 'prodis'));
    }

    /**
     * Update the specified resource in storage.
     */
    public function update(Request $request, $id)
    {
        $mahasiswa = Mahasiswa::findOrFail($id);

        // Validasi termasuk gambar
        $validation = $request->validate([
            'nama' => 'required',
            'npm' => 'required|numeric',
            'prodi' => 'required',
            'foto' => 'nullable|image|mimes:jpeg,png,jpg,gif,svg|max:2048',
        ]);

        // Proses upload gambar baru
        if ($request->hasFile('foto')) {
            // Hapus gambar lama jika ada
            if ($mahasiswa->foto && file_exists(public_path('fotomahasiswa/' . $mahasiswa->foto))) {
                unlink(public_path('fotomahasiswa/' . $mahasiswa->foto));
            }
            
            $namaFoto = $request->npm . '.' . $request->foto->extension();
            $request->foto->move(public_path('fotomahasiswa'), $namaFoto);

            // Set gambar baru
            $mahasiswa->foto = $namaFoto;
        }

        // Update data produk lainnya
        $mahasiswa->update([
            'nama' => $request->nama,
            'npm' => $request->npm,
            'prodi' => $request->prodi,
        ]);

        session()->flash('success', 'Data Mahasiswa Berhasil di Perbaharui');
        return redirect(route('/mahasiswa'));
    }

```
4. tambahkan route pada `web.php` 
```
Route::get('/mahasiswa/edit/{id}', [MahasiswaController::class, 'edit'])->name('mahasiswa/edit');
Route::put('/mahasiswa/edit/{id}', [MahasiswaController::class, 'update'])->name('mahasiswa/update');
```
=================
### Setup Halaman Mahasiswa - Fungsi Hapus Data (delete)
1. edit file `index.blade.php` pada folder mahasiswa bagian tombol delete
```
<a href="javascript:void(0)" onclick="deleteFunction({{ $mahasiswa->id }})" type="button" class="btn btn-danger">Hapus</a>
```
selain itu juga masukan kode script dan modal untuk tombol peringatan ketika akan melakukan penghapusan data
```
<script>
    function deleteFunction(id) {
        // Set the product ID in the hidden input field
        document.getElementById('delete_id').value = id;

        // Update the form action dynamically
        var form = document.getElementById('deleteForm');
        form.action = '/mahasiswa/delete/' + id;

        // Show the modal
        $("#modalDelete").modal('show');
    }
</script>

<!-- Modal -->
<div class="modal fade" id="modalDelete" tabindex="-1" aria-labelledby="modalDeleteTitle" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content">
            <form action="" method="post" id="deleteForm">
                @csrf
                @method('DELETE')
                <input type="hidden" name="mahasiswa_id" id="delete_id">

                <!-- Header Modal -->
                <div class="modal-header bg-danger text-white">
                    <h5 class="modal-title" id="modalDeleteTitle">Konfirmasi Penghapusan Data</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>

                <!-- Body Modal -->
                <div class="modal-body text-center">
                    <p class="fs-5">Apakah kamu yakin untuk menghapus data tersebut?</p>
                    <p class="text-muted">data tidak dapat di kembalikan apabila sudah terhapus</p>
                </div>

                <!-- Footer Modal -->
                <div class="modal-footer justify-content-center">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Batalkan</button>
                    <button type="submit" class="btn btn-danger">Hapus</button>
                </div>
            </form>
        </div>
    </div>
</div>
```
2. tambahkan function delete pada `MahasiswaController.php`
```
    public function delete($id)
    {
        $mahasiswa = Mahasiswa::findOrFail($id);

        // Hapus gambar jika ada
        if ($mahasiswa->foto) {
            if (file_exists(public_path('fotomahasiswa/' . $mahasiswa->foto))) {
                unlink(public_path('fotomahasiswa/' . $mahasiswa->foto));
            }
        }

        // Hapus produk
        $mahasiswa->delete();

        session()->flash('success', 'Data Mahasiswa Berhasil di Hapus');
        return redirect(route('/mahasiswa'));
    }
```
3. tambahkan route pada `web.php` 
```
Route::delete('/mahasiswa/delete/{id}', [MahasiswaController::class, 'delete'])->name('mahasiswa/delete');
```
=================
### Setup Halaman Mahasiswa - Fungsi Pencarian Data (search)
1. edit file `index.blade.php` pada folder mahasiswa bagian kolom pencarian
```
<form action="{{ route('/mahasiswa') }}" method="GET" class="d-flex">
    <input type="text" name="search" class="form-control" placeholder="Pencarian" value="{{ old('search', $search) }}">
    <button class="btn btn-primary ml-2" type="submit"> <i class="bi bi-search"></i> </button>
</form>
```
2. edit function index pada file MahasiswaController.php
```
    public function index(Request $request)
    {
        $search = $request->input('search');
        $mahasiswas = Mahasiswa::where('nama', 'like', '%' . $search . '%')->orderBy('id', 'desc')->get();

        return view('mahasiswa.index', compact('mahasiswas', 'search'));
    }
```
