# Peer-to-Peer Encrypted Chat using Pure DES (Data Encryption Standard)

Aplikasi chat peer-to-peer (P2P) sederhana berbasis Python Socket dan Threading yang dilengkapi dengan enkripsi end-to-end menggunakan implementasi murni algoritma **DES (Data Encryption Standard)** tanpa pustaka kriptografi eksternal (`pycryptodome`, `cryptography`, dll).

---

## 📁 Struktur Berkas

| Berkas | Deskripsi |
| :--- | :--- |
| `des_manual.py` | Modul kriptografi inti yang berisi implementasi manual algoritma DES 64-bit (permutasi, S-Box, Feistel Network, PKCS#7 padding). |
| `peer_a.py` | Node Peer A yang bertindak sebagai Server (Listener pada port `65432`) dan dapat saling bertukar pesan terenkripsi. |
| `peer_b.py` | Node Peer B yang bertindak sebagai Client (terhubung ke Peer A pada port `65432`) dan dapat saling bertukar pesan terenkripsi. |

---

## ⚙️ Fitur Utama

1. **Implementasi Manual DES (64-bit)**:
   - **Key Scheduling**: Pembangkitan 16 round subkey (48-bit) menggunakan tabel `PC-1`, `PC-2`, dan circular left shift (`SHIFTS`).
   - **Feistel Cipher Network**: 16 round operasi Feistel dengan Expansion Table (`E`), 8 substitution boxes (`S-Box`), dan Permutasi (`P`).
   - **Initial & Final Permutation**: Memetakan bit masukan (`IP`) dan inversinya (`FP`).
   - **PKCS#7 Padding**: Penanganan ukuran plaintext agar selalu sesuai dengan kelipatan 8 byte (64 bit).
2. **Komunikasi Socket Full-Duplex**:
   - Memanfaatkan modul `threading` sehingga proses pengiriman (`input`) dan penerimaan (`recv`) pesan dapat berjalan bersamaan (asinkron).
3. **Inspeksi Ciphertext**:
   - Menampilkan data ciphertext mentah dalam format heksadesimal (`hex`) di terminal sebelum proses transmisi dan sebelum didekripsi.

---

## 🔑 Kunci Simetris (Shared Key)

Kedua peer menggunakan kunci rahasia bersama berukuran 8 karakter (64 bit):
```python
SHARED_KEY = "KUNCI8BY"
```
> **Catatan**: Kunci wajib terdiri dari 8 karakter/byte agar sesuai dengan spesifikasi ukuran kunci DES.

---

## 🚀 Panduan Menjalankan

### Prasyarat
- Python versi 3.8 atau lebih baru.
- Tidak diperlukan instalasi pustaka pihak ketiga (`pip`), hanya menggunakan modul standar Python (`socket`, `threading`).

### Langkah Eksekusi

Buka dua jendela terminal terpisah pada direktori yang sama:

#### 1. Jalankan Peer A (Server)
Di terminal pertama, jalankan:
```bash
python peer_a.py
```
*Terminal akan menampilkan:*
```text
[*] Menunggu koneksi dari Peer B di 127.0.0.1:65432...
```

#### 2. Jalankan Peer B (Client)
Di terminal kedua, jalankan:
```bash
python peer_b.py
```
*Kedua terminal akan terhubung:*
```text
[+] Terhubung ke Peer A di 127.0.0.1:65432
Kirim pesan: 
```

#### 3. Mengirim & Menerima Pesan
- Masukkan teks pada prompt `Kirim pesan: ` di salah satu peer dan tekan `Enter`.
- Pesan akan dienkripsi ke bentuk byte DES, dikirim melalui socket, dan penerima akan menampilkan representasi ciphertext (Hex) serta hasil dekripsinya.
- Untuk keluar dari obrolan, ketik `exit`.

---

## 🖥️ Contoh Tampilan Terminal

### Peer A (Pengirim):
```text
Kirim pesan: Halo Peer B!
[Transmisi Ciphertext (Hex)]: 9b7a42ec73f1d8205f41065118c7c13a
Kirim pesan: 
```

### Peer B (Penerima):
```text
[Raw Ciphertext Diterima (Hex)]: 9b7a42ec73f1d8205f41065118c7c13a
[Pesan Didekripsi]: Halo Peer B!
Kirim pesan: 
```

---

## ⚠️ Catatan Keamanan
Implementasi ini ditujukan untuk **tujuan edukasi dan pembelajaran kriptografi dasar**. Algoritma DES klasik memiliki ruang kunci 56-bit yang rentan terhadap serangan *brute-force* dan tidak direkomendasikan untuk sistem produksi (gunakan AES-GCM untuk keamanan modern).
