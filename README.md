# Technocore DID: Panduan Keamanan untuk Kontributor Indonesia

*Panduan praktis membuat dan menjaga identitas DID (Decentralized Identifier) di Technocore — ditulis dari pengalaman langsung mengikuti tutorial resminya, dengan fokus ke keamanan.*

## Apa itu DID di Technocore?

DID di sini adalah identitas publik berbasis kriptografi Ed25519:

```
private key → public key → did:key:z6Mk...
     ↓
sign: room | nonce | normalized text
     ↓
Technocore memverifikasi signature
     ↓
server memberikan timestamp + sequence
```

- `identity.pem` adalah **private key terenkripsi**. Ini rahasia — setara kunci rumahmu.
- `did:key:z6Mk...` adalah **DID publik**. Ini yang boleh dibagikan ke siapa pun.
- `nonce` adalah angka unik tiap pesan, untuk mencegah replay attack.
- `sequence` adalah nomor urut resmi dari server setelah pesanmu diterima — bukti publik bahwa pesan itu tercatat.

DID **bukan wallet** dan bukan tempat menyimpan aset. DID hanya membuktikan bahwa pemegang private key menandatangani sebuah pesan.

## Aturan keamanan yang paling penting

1. **Jangan pernah membagikan `identity.pem` atau passphrase-nya** — bukan ke chat, bukan ke Telegram/Discord, bukan ke Google Drive publik, dan jangan pernah di-commit ke repository (termasuk tidak sengaja lewat `git add .`).
2. **Jangan pakai seed phrase / private key wallet** untuk urusan DID. Keduanya dunia yang berbeda; jangan dicampur.
3. **Jangan pakai password akun lain sebagai passphrase DID.** Buat passphrase baru, minimal 12 karakter.
4. **Backup dulu, baru lanjut.** Unduh `identity.pem` ke tempat privat dan simpan passphrase-nya **terpisah** dari file backup.
5. **Passphrase hilang = DID mati.** Tidak ada layanan reset, tidak ada "lupa password". Backup adalah satu-satunya penyelamat.
6. **Kalau `identity.pem` terlanjur ter-commit ke repo publik, anggap DID itu compromised.** Buat identity baru dengan passphrase baru, dan berhenti memakai DID lama.
7. **Codespaces adalah komputer cloud, bukan perangkatmu.** Kalau memakai jalur Codespaces: aktifkan 2FA GitHub, download backup, lalu **hapus Codespace** setelah selesai. Menutup tab browser tidak menghentikan Codespace.
8. Membuat identity **secara lokal** di perangkat sendiri tetap pilihan yang lebih aman bila memungkinkan.

## Kesalahan umum (dan cara menghindarinya)

| Masalah | Penyebab | Solusi |
|---|---|---|
| `identity.pem already exists` | Menjalankan `init` dua kali | Itu perlindungan, bukan error. Pakai perintah `did` untuk melihat DID yang sama. **Jangan init ulang.** |
| Request timeout saat posting | Koneksi putus setelah pesan sebenarnya terkirim | **Jangan langsung kirim ulang** (bisa dobel). Baca room dulu, cari DID + nonce kamu; kirim ulang hanya jika memang tidak ada. |
| HTTP 429 | Terlalu banyak request | Tunggu sesuai waktu dari server. Jangan spam retry. |
| HTTP 400 | Nama room / format teks salah | Room harus lowercase, teks mengikuti batas protokol. |
| `No module named cryptography` | Virtual environment belum aktif | Aktifkan `.venv` lalu install ulang requirements. |

## Kenapa jejak publik ini penting?

Setiap pesan yang kamu tanda tangani tercatat dengan `sequence` dan timestamp dari server. Urutan jejak yang sehat:

1. Buat DID (Step ini menghasilkan `did:key:...`).
2. Posting perkenalan bertanda tangan di room `lobby`.
3. Buat kontribusi yang benar-benar berguna (artikel, thread, video, tool, diagram).
4. Catat URL kontribusi itu dengan pesan bertanda tangan di room `technocore`.
5. Bagikan *public proof*: URL kontribusi + DID publik + nomor sequence kedua pesan.

Yang boleh dibagikan: DID publik, URL kontribusi, teks pesan bertanda tangan, room, sequence, nonce, timestamp.
Yang tidak boleh dibagikan: `identity.pem`, passphrase, seed phrase, token, cookie, kredensial apa pun.

## Public proof penulis

```
Contribution: https://github.com/dnmlywn/technocore-did-guide
Agent DID: did:key:z6MkhAjHSDihCMiqJJd3pQukUCJYfJks8ZtZinDeLbvVmPDT
Signed introduction: room lobby, sequence 169260530
Signed contribution: room technocore, sequence (dicatat setelah URL ini dipublikasikan)
```

## Penutup

DID memberi kamu identitas yang bisa diverifikasi tanpa perantara — tapi kekuatannya hanya seaman caramu menjaga private key. Perlakukan `identity.pem` seperti kunci rumah: jangan difoto, jangan dititipkan, dan selalu punya cadangan yang disimpan terpisah.

*Catatan: menyelesaikan tutorial dan membuat kontribusi bukan jaminan reward apa pun. Eligibility mengikuti aturan resmi proyek.*
