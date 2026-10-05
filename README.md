# Tugas Individu 2 - Computational Thinking (Perpustakaan)

Nama: Kenzie Gabriel
NIM: 1124160008

## Business Rules Perpustakaan

Aturan bisnis yang diterapkan pada program sistem perpustakaan ini adalah:
- BR-001 Peminjam maksimal hanya diperbolehkan meminjam 3 buku dalam satu kali transaksi peminjaman.
- BR-002 Buku yang berstatus sedang dipinjam oleh orang lain, tidak bisa dipinjam lagi sampai buku tersebut dikembalikan.
- BR-003 Jika peminjam terlambat mengembalikan buku, maka akan otomatis dikenakan sanksi denda sebesar Rp1.000 untuk setiap satu hari keterlambatan.

## Source Code
```dart
// Enum Untuk memilih status buku bro

enum StatusBuku { tersedia, dipinjam}

// untuk menyimpan data buku

class Buku {
  String judul;
  StatusBuku status;
  
  Buku(this.judul, this.status);
}

// untuk menyimpan data peminjaman

class Peminjaman {
  String namaPeminjam;
  List <Buku> daftarBuku;
  int hariTerlambat;
  
  Peminjaman(this.namaPeminjam, this.daftarBuku, this.hariTerlambat);
}

// Fungsi utama untuk mengecek aturan perpus

void prosesPeminjaman(Peminjaman data) {
  print("=== PROSES PEMINJAMAN: ${data.namaPeminjam} ===");
  
  // ATURAN 1: Max pinjem 3 Buku 
  
  if (data.daftarBuku.length > 3) {
    print("Gagal: Maksimal pinjam cuman boleh 3 buku aja bro!");
    print("--------------------------------------------------");
    return;
  }
  
  // ATURAN 2: Cek apakah ada buku yang lagi di pinjam org lain
  
  for (var buku in data.daftarBuku) {
    if (buku.status == StatusBuku.dipinjam) {
      print("Gagal: Buku '${buku.judul}' lagi di pinjam orang lain!");
      return; // fungsinya adalah untuk berhenti memproses Bro--->
    }
  }
  
  // kalau aman semua, kita ubah status bukunya menjadi di pinjam
  
  print("Berhasil pinjam Buku:");
  for (var buku in data.daftarBuku) {
    buku.status = StatusBuku.dipinjam;
    print("- ${buku.judul}");
  }
  
  // ATURAN 3: Hitung denda (Rp 1.000 perhari jika terlambat)
  
  if (data.hariTerlambat > 0) {
    int denda = data.hariTerlambat = 1000;
    print("peringatan: Terlambat ${data.hariTerlambat} hari.");
    print("Total Denda: Rp$denda");
  } else {
    print("ketrlambatan: Tidak ada (aman)");
  }
  print("---------------------------------------------------");
}

void main() {
  Buku buku1 = Buku("Belajar Dart", StatusBuku.tersedia);
  Buku buku2 = Buku("Buku Fiksi", StatusBuku.dipinjam);
  Buku buku3 = Buku("Buku Sejarah", StatusBuku.tersedia);
  Buku buku4 = Buku("Buku Komik", StatusBuku.tersedia);
  Buku buku5 = Buku("Buku Masak", StatusBuku.tersedia);
  
  // skenario uji coba
  
  // Skenario 1: Sukses pinjam 2 buku  & agak telat
  Peminjaman pinjam1 = Peminjaman("Rapli MBG", [buku1, buku3], 0);
  prosesPeminjaman(pinjam1);
  
  //Skenario 2: Gagal Karna Maruk/Rakus pinjam 4 buku
  Peminjaman pinjam2 = Peminjaman("CAHYOONO", [buku1, buku3, buku4, buku5, ], 0);
  prosesPeminjaman(pinjam2);
  
  // Skenario 3: Gagal Karna buku2 sudah di pinjam org lain
  Peminjaman pinjam3 = Peminjaman("Budi", [buku2, buku4], 0);
  prosesPeminjaman(pinjam3);
  
  // Skenario 4: Sukses Pinjam Tapi kena denda telat 5 hari 
  Peminjaman pinjam4 = Peminjaman("Citra", [buku4], 5);
  prosesPeminjaman(pinjam4);
}
```