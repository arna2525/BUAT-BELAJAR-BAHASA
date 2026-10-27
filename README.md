# Nihongo Quest V6

Perubahan: katalog kotoba digabung (CSV + katalog lama + tambahan) dan dipakai penuh oleh aplikasi; data kanji kini dimuat (sebelumnya NQ_KANJIS tidak terdefinisi sehingga halaman kanji/latihan error); label "Bab" yang salah pada kotoba diganti label tingkat.

Jumlah data: kotoba 1479 entri unik, kanji 604 entri unik, bunpou 89 pola (sudah dimuat di halaman Bunpou).
Data kurasi belajar, bukan salinan resmi Minna no Nihongo/IRODORI; verifikasi dengan buku. Sumber resmi: lihat data/irodori_sources.txt.


## Kaiwa AI (Netlify Function)
- Fungsi: `netlify/functions/kaiwa.js`, frontend: `kaiwa-ai.js`.
- v7.js + v7.css: tampilan baru (font besar, detail bab, kartu kanji berputar, kategori latihan). Contoh kalimat bab dibuat AI lewat fungsi yang sama (mode contoh) dan disimpan di browser.
- Di Netlify > Site configuration > Environment variables, isi salah satu: `ANTHROPIC_API_KEY`, `OPENAI_API_KEY`, atau `GEMINI_API_KEY` (opsional `AI_MODEL`, mis. `gemini-3.5-flash`; kalau ada lebih dari satu key, urutan prioritas: Anthropic, OpenAI, Gemini).
- Setelah menambah/mengubah variabel, lakukan *Trigger deploy*.
- bc.css + v8.js: tema BLACK CLOVER (hitam-merah, motif semanggu daun empat, grimoire); menu bawah selalu terlihat (area halaman yang scroll, bukan seluruh layar).
- Urutan kanji: N5 (80) -> N4 (167) -> lainnya; dalam tiap level urut dari yang paling sederhana. Daftar level disusun dari daftar kanji JLPT yang beredar (tidak resmi); makna Indonesia dan contoh ditulis sendiri.
- furi.js: furigana dibuat dari kamus kotoba aplikasi (kanji + okurigana, mis. 食[た]べます).
- v9.js: beranda ala game (level + rank ksatria sihir + mana/XP, 3 Daily Quest, menu ringkas). XP: flashcard +5/+2, soal benar +10 (salah +2), kaiwa +15, putar kartu kanji +1; quest harian reset tiap hari, bonus +50 XP kalau 3 quest selesai. Data tersimpan di localStorage (kunci nqv9).
- v10.js: foto profil sendiri (pilih dari galeri, tersimpan di browser, kunci nqpfp). Aksen pedang hitam ada di hero beranda (bc.css + v9.js).
- bunpou.js + data/bunpou.js: halaman Bunpou (89 pola sering dipakai dari CSV, tiap pola punya rumus, 2 contoh kalimat, catatan, animasi, dan penanda "sudah paham"; kunci localStorage nqbp). data/bunpou.js dibuat dari data/bunpou_patterns_progress.csv ditambah satu contoh kedua per pola.
- data/ruby.js + furi.js: furigana yang sudah dicek manual untuk semua pola Bab 1-50 dan Bunpou (kalimat lain tetap memakai kamus kotoba). data/bushu.js + kanji2.js: halaman Kanji dengan level N5/N4/N3+, jadwal 15 kanji per hari (tabel berkolom atau kartu putar), kelompok bushu (N5/N4), penanda hafal (kunci localStorage nqkj).
- stories.js + data/stories.js: modul Cerita (2 cerita orisinal: Momotaro versi tulis ulang dan "Hari liburku"). Tampilan teks asli / bantuan hiragana / latihan katakana, romaji & terjemahan opsional, kosakata, kanji yang dipakai, kuis pemahaman & kosakata, favorit, riwayat, posisi baca (kunci localStorage nqst).
- jalur.js + data/jalur.js: Jalur kanji N5 -> Jembatan -> N4 (247 kanji, 17 tahap, urutan REKOMENDASI APLIKASI, bukan urutan resmi ujian). Status terkunci/tersedia/dipelajari/perlu diulang/dikuasai, Pelajari bebas, rekomendasi berikutnya, kuis uji diri, ulangi yang salah (kunci localStorage nqjalur; tanda hafal lama nqkj dimigrasi otomatis).
- Profil: ekspor/impor cadangan progres (semua kunci nq*).

## Fitur tambahan (v6 lanjutan)
- `util.js` — kana↔romaji, pengaturan (romaji opsional, Profil), SRS (kotak Leitner 1/2/4/8/16/32 hari, kunci `nqsrs`), riwayat kuis (`nqhist`).
- `data/verbs.js` + `konjugasi.js` — 651 kata kerja, 13 bentuk, diuji 22 kasus (0 salah).
- `bank.js` + `kuis.js` — Pusat Kuis: 13 mode (kotoba JP↔ID, ketik jawaban, bacaan kata, kanji arti/bacaan, urutan kanji, lengkapi kalimat, fungsi pola, susun kata, membaca, kosakata cerita, konjugasi) + Ulangi Cerdas. Jawaban tercatat otomatis ke SRS; kartu flashcard juga.
- `jft.js` — Simulasi JFT 30 soal/30 menit: timer berbasis waktu akhir tersimpan (bisa dilanjutkan setelah reload), nomor soal, tanda ragu, auto-kumpul saat waktu habis, pembahasan, riwayat. **Bukan ujian resmi, tanpa listening.**
- `stats.js` — statistik Profil dari data nyata + pencapaian + saklar romaji.
- PWA: `manifest.webmanifest`, `sw.js` (cache offline), ikon.

## Catatan lisensi
Semua soal dihasilkan dari data aplikasi sendiri; cerita buatan sendiri. Tidak ada kode yang disalin dari repositori lain. Konten buku (Minna no Nihongo / JFT) hanya dirujuk sebagai topik, bukan disalin. Periksa sendiri bacaan/arti terhadap buku Anda.

## Data dari file PDF pengguna (terbaru)
- **Kotoba Bab 1–50**: `data/kotoba.js` kini berisi kotoba dari PDF *Kotoba Minna no Nihongo Bab 1–50* (label `Bab N`), ditambah kata katalog lama di luar buku berlabel `Tambahan`. Kategori (Kata benda/kerja/sifat/ungkapan) otomatis dari pola kata, bisa keliru. Beberapa kata Bab 50 di PDF tidak punya arti (dibiarkan kosong dan tidak dipakai dalam kuis).
- **Kanji**: `data/kanji.js` mengikuti PDF JLPT kanji.sh: N5 103, N4 181, N3 361 (+182 kanji tambahan berlabel N3+). Bacaan onyomi/kunyomi dari PDF. Arti bahasa Indonesia ditulis ulang; contoh kata tersedia hanya untuk kanji lama (197 kanji baru belum punya contoh).
- **Jalur kanji**: dibangun ulang dari daftar PDF: N5 103 → Jembatan 17 → N4 164 (19 tahap).

## Pembaruan terbaru (v11)
- **Kotoba**: jenis kata dikoreksi ulang (data/kotoba.js): kata benda 1650, kata kerja 1037, sifat い 87, sifat な 55, keterangan 46, ungkapan 103, konjungsi 10, kata ganti 3. Memilih satu jenis hanya menampilkan jenis itu. Bacaan yang tercampur catatan ［...］ dibersihkan, 64 entri kembar dibuang, dan beberapa salah ketik hasil konversi PDF diperbaiki (行ます→行きます, お世話になりす→お世話になります, いらしゃいませ→いらっしゃいませ, ダイニングキチン→ダイニングキッチン). Klasifikasi berbasis aturan + daftar, bukan kamus resmi: periksa kata yang meragukan.
- **Rentang bab**: di Kotoba dan di Latihan bisa memilih "Bab X sampai Bab Y" (atau Tambahan), digabung dengan jenis kata; tombol "Latihan · Bab 1–5 · jenis" langsung memulai kuis dari kata-kata itu. Pilihan tersimpan (kunci nqrange).
- **Latihan** jadi hub kartu: Kanji, Kotoba (kata benda / kata sifat / kata kerja / campur), JFT, Kaiwa, Cerita, dan Pusat Kuis.
- **Materi** jadi hub kategori: Pelajaran Bab 1–50, Bunpou, Partikel, Kata kerja, Kotoba, Kanji. Bunpou dipindah ke sini (tombol kembali menuju Materi).
- **Beranda**: jalur belajar 3 langkah (Belajar Materi, Hafalan, Latihan) menggantikan 9 tombol menu.
- File baru: v11.js, v11.css. Service worker naik ke nq-v7 (buka aplikasi lalu muat ulang sekali-dua kali agar versi baru terpakai).

## Pembaruan data kotoba (v12)
Seluruh kotoba Bab 1–50 diperiksa ulang satu per satu. Hasil: 189 entri dikoreksi, 1 entri sampah dihapus (だです。), 2 entri yang hilang ditambahkan (インド di Bab 1, 気をつけます di Bab 23), dan 45 entri yang artinya kosong (39 di Bab 50) diisi dengan arti Indonesia.
Contoh koreksi: アメキカ→アメリカ (Amerika Serikat), イギリス (India→Inggris), 韓国 (Thailand→Korea Selatan), 友達を会います→友達に会います, ミリク→ミルク, 貨します→貸します, 消しゴマ→消しゴム, かたかな→カタカナ (arti "huruf katakana"), 払います/返します/出かけます (jepang, bacaan, arti yang bergeser), 平均寿命/比べる (arti tertukar), 受賞する, 緊張する (きんちょう), 成人式 (せいじんしき), 本日休業 (ほんじつ), dll.
Kotoba "Tambahan" (929 entri) hanya dibaca sekilas; tidak ditemukan kesalahan yang jelas. Arti yang kurang pas secara nuansa masih mungkin ada: periksa terhadap buku Minna no Nihongo.
Service worker naik ke nq-v8.

## Pembaruan v13
- **Flashcard bebas pilih** (flash2.js): tab Kotoba (rentang bab + jenis kata), Kanji (rentang tahap jalur 1–19 atau "Lainnya" = kanji di luar jalur), Masih sulit, dan Hari ini (hafalan harian). Tombol Acak mengacak urutan kartu. Pilihan tersimpan (kunci nqfd).
- **Kanji**: mode "Harian · 15/hari" dihapus (sudah digantikan Jalur N5→N4). Di Jalur ada **Target hafalan harian** (pilih 5–40 atau ketik angka bebas; "Hafalkan" membuka flashcard, "Quiz hari ini" menguji kanji itu) dan **Acak quiz** (pilih tahap dari–sampai, mulai dari tahap pertama, jumlah soal 5/10/20/30; tiga jenis soal: arti, bacaan, kanji dari arti). Hasil memperbarui status kanji (dikuasai / perlu diulang) dan riwayat skor tersimpan.
- **Cerita**: 10 cerita (8 baru, orisinal untuk aplikasi ini), lengkap dengan furigana, mode katakana, romaji, kosakata, dan kuis.
- **Zoom**: ketuk dua kali tidak lagi memperbesar halaman (touch-action: manipulation); cubit dua jari tetap bisa.
- Service worker naik ke nq-v9.
