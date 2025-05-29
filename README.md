<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Arduino Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Arduino Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [

  { "en": "Abadi?", "id": "Kekal." },
  { "en": "Abah?", "id": "Ayah." },
  { "en": "Abai?", "id": "Lalai." },
  { "en": "Abdi?", "id": "Hamba." },
  { "en": "Abece?", "id": "Alfabet." },
  { "en": "Aberasi?", "id": "Penyimpangan." },
  { "en": "Absah?", "id": "Sah." },
  { "en": "Absen?", "id": "Mangkir." },
  { "en": "Absolut?", "id": "Mutlak." },
  { "en": "Abstain?", "id": "Tidak memilih." },
  { "en": "Abstrak?", "id": "Maya." },
  { "en": "Absurd?", "id": "Konyol." },
  { "en": "Abu?", "id": "Debu." },
  { "en": "Acap?", "id": "Sering." },
  { "en": "Acara?", "id": "Agenda." },
  { "en": "Acu?", "id": "Rujuk." },
  { "en": "Acuh?", "id": "Peduli." },
  { "en": "Ada?", "id": "Eksis." },
  { "en": "Adab?", "id": "Etika." },
  { "en": "Adagio?", "id": "Lambat (musik)." },
  { "en": "Adaptasi?", "id": "Penyesuaian." },
  { "en": "Adat?", "id": "Tradisi." },
  { "en": "Adegan?", "id": "Babak." },
  { "en": "Adem?", "id": "Dingin." },
  { "en": "Adhesi?", "id": "Pelekatan." },
  { "en": "Adi?", "id": "Unggul." },
  { "en": "Adicita?", "id": "Ideologi." },
  { "en": "Adil?", "id": "Saksama." },
  { "en": "Adinda?", "id": "Adik." },
  { "en": "Adjektiva?", "id": "Kata sifat." },
  { "en": "Administrasi?", "id": "Tata usaha." },
  { "en": "Adopsi?", "id": "Pengangkatan." },
  { "en": "Adu?", "id": "Tanding." },
  { "en": "Aduan?", "id": "Keluhan." },
  { "en": "Aduhai?", "id": "Amboi." },
  { "en": "Adun?", "id": "Hias." },
  { "en": "Afeksi?", "id": "Kasih sayang." },
  { "en": "Afirmatif?", "id": "Mengiyakan." },
  { "en": "Afiks?", "id": "Imbuhan." },
  { "en": "Afront?", "id": "Penghinaan." },
  { "en": "Afsun?", "id": "Mantra." },
  { "en": "Aga?", "id": "Mulia (bangsawan)." },
  { "en": "Agah?", "id": "Godaan." },
  { "en": "Agak?", "id": "Kira-kira." },
  { "en": "Agama?", "id": "Religi." },
  { "en": "Agar?", "id": "Supaya." },
  { "en": "Agas?", "id": "Nyamuk kecil." },
  { "en": "Agen?", "id": "Perwakilan." },
  { "en": "Agenda?", "id": "Jadwal." },
  { "en": "Agitasi?", "id": "Hasutan." },
  { "en": "Agnostik?", "id": "Peragu Tuhan." },
  { "en": "Agregat?", "id": "Kumpulan." },
  { "en": "Agresi?", "id": "Serangan." },
  { "en": "Agronomi?", "id": "Ilmu pertanian." },
  { "en": "Agung?", "id": "Besar." },
  { "en": "Agunan?", "id": "Jaminan." },
  { "en": "Agustus?", "id": "Bulan kedelapan." },
  { "en": "Ahad?", "id": "Minggu (hari)." },
  { "en": "Ahistoris?", "id": "Tidak sesuai sejarah." },
  { "en": "Ahkam?", "id": "Hukum." },
  { "en": "Akhir?", "id": "Ujung." },
  { "en": "Akhlak?", "id": "Budi pekerti." },
  { "en": "Ahli?", "id": "Pakar." },
  { "en": "Ahwal?", "id": "Perihal." },
  { "en": "Aib?", "id": "Malu." },
  { "en": "Aidilfitri?", "id": "Idul Fitri." },
  { "en": "Air?", "id": "Banyu." },
  { "en": "Ajaib?", "id": "Anomali." },
  { "en": "Ajak?", "id": "Undang." },
  { "en": "Ajal?", "id": "Kematian." },
  { "en": "Ajar?", "id": "Didik." },
  { "en": "Ajeg?", "id": "Tetap." },
  { "en": "Ajudan?", "id": "Asisten." },
  { "en": "Akad?", "id": "Perjanjian." },
  { "en": "Akademi?", "id": "Lembaga pendidikan." },
  { "en": "Akal?", "id": "Pikiran." },
  { "en": "Akan?", "id": "Bakal." },
  { "en": "Akar?", "id": "Dasar." },
  { "en": "Akas?", "id": "Tangkas." },
  { "en": "Akbar?", "id": "Besar." },
  { "en": "Akibat?", "id": "Dampak." },
  { "en": "Akidah?", "id": "Kepercayaan." },
  { "en": "Akil balig?", "id": "Dewasa." },
  { "en": "Aklamasi?", "id": "Persetujuan bulat." },
  { "en": "Aklimatisasi?", "id": "Penyesuaian suhu." },
  { "en": "Akomodasi?", "id": "Penginapan." },
  { "en": "Akrab?", "id": "Dekat." },
  { "en": "Akreditasi?", "id": "Pengakuan." },
  { "en": "Aksara?", "id": "Huruf." },
  { "en": "Akselerasi?", "id": "Percepatan." },
  { "en": "Aksen?", "id": "Logat." },
  { "en": "Akseptor?", "id": "Penerima." },
  { "en": "Akses?", "id": "Jalan masuk." },
  { "en": "Aksi?", "id": "Tindakan." },
  { "en": "Akta?", "id": "Surat resmi." },
  { "en": "Aktif?", "id": "Giat." },
  { "en": "Aktivitas?", "id": "Kegiatan." },
  { "en": "Aktor?", "id": "Pemeran." },
  { "en": "Aktris?", "id": "Pemeran wanita." },
  { "en": "Aktual?", "id": "Terkini." },
  { "en": "Akumulasi?", "id": "Pengumpulan." },
  { "en": "Akun?", "id": "Rekening." },
  { "en": "Akur?", "id": "Rukun." },
  { "en": "Akurat?", "id": "Tepat." },
  { "en": "Akut?", "id": "Parah." },
  { "en": "Alamat?", "id": "Tujuan surat." },
  { "en": "Alam?", "id": "Dunia." },
  { "en": "Alami?", "id": "Natural." },
  { "en": "Alang-alang?", "id": "Ilalang." },
  { "en": "Alas?", "id": "Dasar." },
  { "en": "Alat?", "id": "Perkakas." },
  { "en": "Album?", "id": "Kumpulan foto." },
  { "en": "Alkohol?", "id": "Minuman keras." },
  { "en": "Alergi?", "id": "Reaksi tubuh." },
  { "en": "Alfabetis?", "id": "Menurut abjad." },
  { "en": "Algojo?", "id": "Eksekutor." },
  { "en": "Algoritma?", "id": "Langkah-langkah." },
  { "en": "Alhasil?", "id": "Akhirnya." },
  { "en": "Ali-ali?", "id": "Ketapel." },
  { "en": "Aliansi?", "id": "Persekutuan." },
  { "en": "Alias?", "id": "Nama lain." },
  { "en": "Alibi?", "id": "Bukti (ketidakhadiran)." },
  { "en": "Alih?", "id": "Pindah." },
  { "en": "Alim?", "id": "Saleh." },
  { "en": "Alir?", "id": "Mengalir." },
  { "en": "Aliran?", "id": "Arus." },
  { "en": "Alkisah?", "id": "Tersebutlah." },
  { "en": "Allah?", "id": "Tuhan." },
  { "en": "Almanak?", "id": "Kalender." },
  { "en": "Almarhum?", "id": "Mendiang." },
  { "en": "Almari?", "id": "Lemari." },
  { "en": "Alpa?", "id": "Lalai." },
  { "en": "Alternatif?", "id": "Pilihan lain." },
  { "en": "Alto?", "id": "Suara rendah (wanita)." },
  { "en": "Alum?", "id": "Layu." },
  { "en": "Alumnus?", "id": "Lulusan." },
  { "en": "Alun-alun?", "id": "Lapangan." },
  { "en": "Alur?", "id": "Jalan cerita." },
  { "en": "Alus?", "id": "Halus." },
  { "en": "Amalgamasi?", "id": "Pencampuran." },
  { "en": "Amal?", "id": "Perbuatan baik." },
  { "en": "Aman?", "id": "Sentosa." },
  { "en": "Amanah?", "id": "Kepercayaan." },
  { "en": "Amanat?", "id": "Pesan." },
  { "en": "Amarah?", "id": "Murka." },
  { "en": "Amat?", "id": "Sangat." },
  { "en": "Amatir?", "id": "Pemula." },
  { "en": "Ambal?", "id": "Karpet." },
  { "en": "Ambang?", "id": "Batas." },
  { "en": "Ambar?", "id": "Wangi-wangian." },
  { "en": "Ambigu?", "id": "Taksa." },
  { "en": "Ambisi?", "id": "Keinginan kuat." },
  { "en": "Ambles?", "id": "Turun (tanah)." },
  { "en": "Ambulans?", "id": "Mobil jenazah." },
  { "en": "Amendemen?", "id": "Perubahan." },
  { "en": "Amfibi?", "id": "Dua alam." },
  { "en": "Ami?", "id": "Ibu (arkais)." },
  { "en": "Amikal?", "id": "Bersahabat." },
  { "en": "Amis?", "id": "Anyir." },
  { "en": "Amnesti?", "id": "Pengampunan." },
  { "en": "Amoral?", "id": "Tidak bermoral." },
  { "en": "Amorf?", "id": "Tanpa bentuk." },
  { "en": "Ampas?", "id": "Sisa." },
  { "en": "Ampel?", "id": "Bambu." },
  { "en": "Ampuh?", "id": "Manjur." },
  { "en": "Amputasi?", "id": "Pemotongan." },
  { "en": "Amuk?", "id": "Mengamuk." },
  { "en": "Anak?", "id": "Putra." },
  { "en": "Analis?", "id": "Penganalisis." },
  { "en": "Analisis?", "id": "Kajian." },
  { "en": "Analogi?", "id": "Persamaan." },
  { "en": "Anarki?", "id": "Kekacauan." },
  { "en": "Anatomi?", "id": "Susunan tubuh." },
  { "en": "Ancaman?", "id": "Bahaya." },
  { "en": "Anda?", "id": "Kamu (formal)." },
  { "en": "Andal?", "id": "Tangguh." },
  { "en": "Andalan?", "id": "Utama." },
  { "en": "Andam?", "id": "Hiasan rambut." },
  { "en": "Andil?", "id": "Peran." },
  { "en": "Aneh?", "id": "Ganjil." },
  { "en": "Aneka?", "id": "Beragam." },
  { "en": "Aneksasi?", "id": "Pencaplokan." },
  { "en": "Angan?", "id": "Khayal." },
  { "en": "Angan-angan?", "id": "Cita-cita." },
  { "en": "Angelfis?", "id": "Ikan malaikat." },
  { "en": "Anggara?", "id": "Selasa (arkais)." },
  { "en": "Anggaran?", "id": "Bujet." },
  { "en": "Anggota?", "id": "Personel." },
  { "en": "Anggit?", "id": "Gubah." },
  { "en": "Angka?", "id": "Nomor." },
  { "en": "Angkara?", "id": "Keserakahan." },
  { "en": "Angkasa?", "id": "Langit." },
  { "en": "Angkat?", "id": "Junjung." },
  { "en": "Angker?", "id": "Seram." },
  { "en": "Angket?", "id": "Kuesioner." },
  { "en": "Angkuh?", "id": "Sombong." },
  { "en": "Angkut?", "id": "Bawa." },
  { "en": "Angler?", "id": "Pemancing." },
  { "en": "Angin?", "id": "Bayu." },
  { "en": "Angot?", "id": "Kumat." },
  { "en": "Angsur?", "id": "Cicil." },
  { "en": "Aniaya?", "id": "Siksa." },
  { "en": "Animasi?", "id": "Gambar hidup." },
  { "en": "Animo?", "id": "Minat." },
  { "en": "Anjung?", "id": "Panggung." },
  { "en": "Anjuran?", "id": "Saran." },
  { "en": "Anomali?", "id": "Keanehan." },
  { "en": "Anonim?", "id": "Tanpa nama." },
  { "en": "Antagonis?", "id": "Lawan." },
  { "en": "Antap?", "id": "Tenang." },
  { "en": "Antar?", "id": "Kirim." },
  { "en": "Antara?", "id": "Jarak." },
  { "en": "Antarbangsa?", "id": "Internasional." },
  { "en": "Antartika?", "id": "Kutub Selatan." },
  { "en": "Anteken?", "id": "Catatan." },
  { "en": "Antelas?", "id": "Satin." },
  { "en": "Antem?", "id": "Pukul." },
  { "en": "Antena?", "id": "Penerima sinyal." },
  { "en": "Anteng?", "id": "Diam." },
  { "en": "Antero?", "id": "Seluruh." },
  { "en": "Anti?", "id": "Lawan." },
  { "en": "Antipati?", "id": "Kebencian." },
  { "en": "Antiseptik?", "id": "Pembasmi kuman." },
  { "en": "Antitesis?", "id": "Lawan pendapat." },
  { "en": "Antik?", "id": "Kuno." },
  { "en": "Antologi?", "id": "Kumpulan karya." },
  { "en": "Antre?", "id": "Berbaris." },
  { "en": "Antropologi?", "id": "Ilmu manusia." },
  { "en": "Antuk?", "id": "Mengantuk." },
  { "en": "Anual?", "id": "Tahunan." },
  { "en": "Anugerah?", "id": "Karunia." },
  { "en": "Anulir?", "id": "Batalkan." },
  { "en": "Anyam?", "id": "Jalin." },
  { "en": "Anyar?", "id": "Baru." },
  { "en": "Anyelir?", "id": "Bunga." },
  { "en": "Apa?", "id": "Apakah." },
  { "en": "Apabila?", "id": "Jika." },
  { "en": "Apalagi?", "id": "Terlebih." },
  { "en": "Aparat?", "id": "Petugas." },
  { "en": "Apartemen?", "id": "Hunian." },
  { "en": "Apati?", "id": "Tidak peduli." },
  { "en": "Ape?", "id": "Kue." },
  { "en": "Apersepsi?", "id": "Pemahaman." },
  { "en": "Apik?", "id": "Rapi." },
  { "en": "Aplikasi?", "id": "Penerapan." },
  { "en": "Apkir?", "id": "Tolak." },
  { "en": "Aplaus?", "id": "Tepuk tangan." },
  { "en": "Apoge?", "id": "Puncak." },
  { "en": "Apokrif?", "id": "Diragukan." },
  { "en": "Apologi?", "id": "Pembelaan." },
  { "en": "Aposteriori?", "id": "Setelah pengalaman." },
  { "en": "Apostolik?", "id": "Kerasulan." },
  { "en": "Apotek?", "id": "Toko obat." },
  { "en": "Apresiasi?", "id": "Penghargaan." },
  { "en": "April?", "id": "Bulan keempat." },
  { "en": "Apriori?", "id": "Sebelum pengalaman." },
  { "en": "Ara?", "id": "Pohon." },
  { "en": "Arab?", "id": "Timur Tengah." },
  { "en": "Arah?", "id": "Jurusan." },
  { "en": "Arak?", "id": "Minuman keras." },
  { "en": "Aral?", "id": "Halangan." },
  { "en": "Arang?", "id": "Karbon." },
  { "en": "Aransemen?", "id": "Gubahan." },
  { "en": "Aras?", "id": "Tingkat (arkais)." },
  { "en": "Arbab?", "id": "Tuan." },
  { "en": "Arbiter?", "id": "Penengah." },
  { "en": "Arbitrer?", "id": "Sewenang-wenang." },
  { "en": "Arca?", "id": "Patung." },
  { "en": "Area?", "id": "Kawasan." },
  { "en": "Arena?", "id": "Gelanggang." },
  { "en": "Arga?", "id": "Gunung (arkais)." },
  { "en": "Argumen?", "id": "Alasan." },
  { "en": "Argumentasi?", "id": "Pembuktian." },
  { "en": "Ari?", "id": "Adik." },
  { "en": "Arif?", "id": "Bijaksana." },
  { "en": "Aring?", "id": "Bau pesing." },
  { "en": "Aristokrasi?", "id": "Bangsawan." },
  { "en": "Aritmetika?", "id": "Ilmu hitung." },
  { "en": "Arkade?", "id": "Lorong." },
  { "en": "Arkais?", "id": "Kuno." },
  { "en": "Arketipe?", "id": "Purwarupa." },
  { "en": "Armada?", "id": "Angkatan laut." },
  { "en": "Aroma?", "id": "Bau." },
  { "en": "Arpa?", "id": "Harpa." },
  { "en": "Arsir?", "id": "Garis bayang." },
  { "en": "Arsis?", "id": "Aksen (musik)." },
  { "en": "Arsonis?", "id": "Pembakar." },
  { "en": "Artefak?", "id": "Benda purbakala." },
  { "en": "Arteri?", "id": "Pembuluh nadi." },
  { "en": "Artesis?", "id": "Sumur bor." },
  { "en": "Arti?", "id": "Makna." },
  { "en": "Artifisial?", "id": "Buatan." },
  { "en": "Artikel?", "id": "Tulisan." },
  { "en": "Artikulasi?", "id": "Pengucapan." },
  { "en": "Artileri?", "id": "Persenjataan berat." },
  { "en": "Artis?", "id": "Seniman." },
  { "en": "Artistik?", "id": "Berseni." },
  { "en": "Arus?", "id": "Aliran." },
  { "en": "Arwah?", "id": "Roh." },
  { "en": "Asa?", "id": "Harapan." },
  { "en": "Asabat?", "id": "Otot." },
  { "en": "Asah?", "id": "Tajamkan." },
  { "en": "Asak?", "id": "Penuh." },
  { "en": "Asal?", "id": "Mula." },
  { "en": "Asam?", "id": "Masam." },
  { "en": "Asap?", "id": "Uap." },
  { "en": "Asas?", "id": "Dasar." },
  { "en": "Asasi?", "id": "Mendasar." },
  { "en": "Aset?", "id": "Harta." },
  { "en": "Asih?", "id": "Kasih." },
  { "en": "Asimilasi?", "id": "Pembauran." },
  { "en": "Asin?", "id": "Masin." },
  { "en": "Asing?", "id": "Luar." },
  { "en": "Asisten?", "id": "Pembantu." },
  { "en": "Asli?", "id": "Orisinal." },
  { "en": "Asma?", "id": "Bengek." },
  { "en": "Asmara?", "id": "Cinta." },
  { "en": "Asonansi?", "id": "Persamaan vokal." },
  { "en": "Asosiasi?", "id": "Perkumpulan." },
  { "en": "Aspal?", "id": "Ter." },
  { "en": "Aspek?", "id": "Sisi." },
  { "en": "Aspirasi?", "id": "Keinginan." },
  { "en": "Asprak?", "id": "Kata (arkais)." },
  { "en": "Asri?", "id": "Indah." },
  { "en": "Astaga?", "id": "Aduh." },
  { "en": "Astrologi?", "id": "Ilmu perbintangan (ramal)." },
  { "en": "Astronomi?", "id": "Ilmu bintang." },
  { "en": "Asuh?", "id": "Bimbing." },
  { "en": "Asumsi?", "id": "Anggapan." },
  { "en": "Asuransi?", "id": "Pertanggungan." },
  { "en": "Asyik?", "id": "Menyenangkan." },
  { "en": "Atap?", "id": "Plafon (bagian atas)." },
  { "en": "Atar?", "id": "Minyak wangi (arkais)." },
  { "en": "Atas?", "id": "Luhur." },
  { "en": "Atase?", "id": "Pejabat diplomatik." },
  { "en": "Ateis?", "id": "Tidak bertuhan." },
  { "en": "Atlet?", "id": "Olahragawan." },
  { "en": "Atlas?", "id": "Kumpulan peta." },
  { "en": "Atma?", "id": "Jiwa." },
  { "en": "Atmosfer?", "id": "Lapisan udara." },
  { "en": "Atom?", "id": "Partikel terkecil." },
  { "en": "Atraktif?", "id": "Menarik." },
  { "en": "Atribut?", "id": "Kelengkapan." },
  { "en": "Audiens?", "id": "Penonton." },
  { "en": "Audio?", "id": "Suara." },
  { "en": "Audit?", "id": "Pemeriksaan keuangan." },
  { "en": "Auditorium?", "id": "Ruang pertemuan." },
  { "en": "Augmentasi?", "id": "Penambahan." },
  { "en": "Aula?", "id": "Balai." },
  { "en": "Aum?", "id": "Raung." },
  { "en": "Aura?", "id": "Pancaran." },
  { "en": "Autarki?", "id": "Swasembada." },
  { "en": "Autentik?", "id": "Sahih." },
  { "en": "Autobiografi?", "id": "Riwayat hidup pribadi." },
  { "en": "Autodidak?", "id": "Belajar sendiri." },
  { "en": "Autograf?", "id": "Tanda tangan." },
  { "en": "Autokrasi?", "id": "Pemerintahan tunggal." },
  { "en": "Autonomi?", "id": "Otonomi." },
  { "en": "Autopsi?", "id": "Bedah mayat." },
  { "en": "Avatar?", "id": "Penjelmaan." },
  { "en": "Aversi?", "id": "Keengganan." },
  { "en": "Aviari?", "id": "Kandang burung besar." },
  { "en": "Aviasi?", "id": "Penerbangan." },
  { "en": "Awalan?", "id": "Prefiks." },
  { "en": "Awal?", "id": "Mula." },
  { "en": "Awam?", "id": "Umum." },
  { "en": "Awan?", "id": "Mega." },
  { "en": "Awas?", "id": "Siaga." },
  { "en": "Awet?", "id": "Tahan lama." },
  { "en": "Azab?", "id": "Siksa." },
  { "en": "Azal?", "id": "Permulaan (waktu)." },
  { "en": "Azan?", "id": "Seruan salat." },
  { "en": "Azimat?", "id": "Jimat." },
  { "en": "Aziz?", "id": "Mulia (Tuhan)." },
  { "en": "Baba?", "id": "Ayah (Peranakan)." },
  { "en": "Babak?", "id": "Bagian." },
  { "en": "Babar?", "id": "Bentang." },
  { "en": "Babat?", "id": "Tebas." },
  { "en": "Babil?", "id": "Keras kepala." },
  { "en": "Bablas?", "id": "Terus." },
  { "en": "Babon?", "id": "Induk ayam." },
  { "en": "Babu?", "id": "Inang pengasuh." },
  { "en": "Bacar?", "id": "Cerewet." },
  { "en": "Bacok?", "id": "Tebas." },
  { "en": "Badai?", "id": "Topan." },
  { "en": "Badak?", "id": "Hewan bercula." },
  { "en": "Badal?", "id": "Pengganti." },
  { "en": "Badan?", "id": "Tubuh." },
  { "en": "Badani?", "id": "Jasmaniah." },
  { "en": "Badik?", "id": "Senjata tikam." },
  { "en": "Badui?", "id": "Suku." },
  { "en": "Badung?", "id": "Nakal." },
  { "en": "Badut?", "id": "Pelawak." },
  { "en": "Bagai?", "id": "Seperti." },
  { "en": "Bagak?", "id": "Berani." },
  { "en": "Bagan?", "id": "Skema." },
  { "en": "Bagaskara?", "id": "Matahari (sastra)." },
  { "en": "Bagi?", "id": "Untuk." },
  { "en": "Bagian?", "id": "Porsi." },
  { "en": "Baginda?", "id": "Raja." },
  { "en": "Bagong?", "id": "Tokoh wayang." },
  { "en": "Bagur?", "id": "Besar tegap." },
  { "en": "Bagus?", "id": "Baik." },
  { "en": "Bahadur?", "id": "Pahlawan." },
  { "en": "Bahagia?", "id": "Senang." },
  { "en": "Bahak?", "id": "Tertawa keras." },
  { "en": "Bahala?", "id": "Bencana." },
  { "en": "Bahan?", "id": "Materi." },
  { "en": "Bahana?", "id": "Gema." },
  { "en": "Bahari?", "id": "Laut." },
  { "en": "Baharu?", "id": "Baru." },
  { "en": "Bahas?", "id": "Diskusi." },
  { "en": "Bahasa?", "id": "Tutur." },
  { "en": "Bahaya?", "id": "Risiko." },
  { "en": "Bahkan?", "id": "Malah." },
  { "en": "Bahtera?", "id": "Kapal." },
  { "en": "Bahu?", "id": "Pundak." },
  { "en": "Bahwa?", "id": "Sesungguhnya." },
  { "en": "Baiat?", "id": "Sumpah setia." },
  { "en": "Baiduri?", "id": "Permata." },
  { "en": "Baik?", "id": "Bagus." },
  { "en": "Bait?", "id": "Baris sajak." },
  { "en": "Baja?", "id": "Logam." },
  { "en": "Bajak?", "id": "Rompak." },
  { "en": "Bajan?", "id": "Wadah." },
  { "en": "Bajang?", "id": "Kerdil." },
  { "en": "Baji?", "id": "Pasak." },
  { "en": "Bajik?", "id": "Kebajikan." },
  { "en": "Baju?", "id": "Pakaian." },
  { "en": "Baka?", "id": "Kekal." },
  { "en": "Bakal?", "id": "Calon." },
  { "en": "Bakar?", "id": "Bakar." },
  { "en": "Bakat?", "id": "Talenta." },
  { "en": "Bakau?", "id": "Pohon pantai." },
  { "en": "Bakhil?", "id": "Kikir." },
  { "en": "Baki?", "id": "Nampan." },
  { "en": "Bakmi?", "id": "Mi." },
  { "en": "Bakpao?", "id": "Kue." },
  { "en": "Bakti?", "id": "Pengabdian." },
  { "en": "Baku?", "id": "Standar." },
  { "en": "Bakul?", "id": "Keranjang." },
  { "en": "Bakup?", "id": "Bengkak." },
  { "en": "Bala?", "id": "Bencana." },
  { "en": "Balad?", "id": "Negeri." },
  { "en": "Balada?", "id": "Nyanyian kisah." },
  { "en": "Balai?", "id": "Gedung." },
  { "en": "Balairung?", "id": "Aula." },
  { "en": "Balak?", "id": "Kayu besar." },
  { "en": "Balalaika?", "id": "Alat musik." },
  { "en": "Balapan?", "id": "Lomba cepat." },
  { "en": "Balas?", "id": "Jawab." },
  { "en": "Balela?", "id": "Membangkang." },
  { "en": "Balerina?", "id": "Penari balet." },
  { "en": "Balet?", "id": "Tarian." },
  { "en": "Balgam?", "id": "Dahak." },
  { "en": "Baliho?", "id": "Papan reklame." },
  { "en": "Balik?", "id": "Kembali." },
  { "en": "Balistik?", "id": "Ilmu peluru." },
  { "en": "Balita?", "id": "Anak bawah lima tahun." },
  { "en": "Balkon?", "id": "Serambi atas." },
  { "en": "Balneoterapi?", "id": "Terapi mandi." },
  { "en": "Balok?", "id": "Batang." },
  { "en": "Balon?", "id": "Gelembung." },
  { "en": "Balsam?", "id": "Minyak gosok." },
  { "en": "Balu?", "id": "Janda." },
  { "en": "Baluarti?", "id": "Benteng." },
  { "en": "Balun?", "id": "Gulung." },
  { "en": "Balung?", "id": "Tulang." },
  { "en": "Balur?", "id": "Kristal." },
  { "en": "Bambu?", "id": "Buluh." },
  { "en": "Ban?", "id": "Roda karet." },
  { "en": "Banal?", "id": "Biasa." },
  { "en": "Banat?", "id": "Serat." },
  { "en": "Bancakan?", "id": "Kenduri." },
  { "en": "Banci?", "id": "Waria." },
  { "en": "Bancut?", "id": "Gagal." },
  { "en": "Banda?", "id": "Harta." },
  { "en": "Bandan?", "id": "Pohon." },
  { "en": "Bandar?", "id": "Pelabuhan." },
  { "en": "Bandara?", "id": "Pelabuhan udara." },
  { "en": "Bandarsah?", "id": "Madrasah." },
  { "en": "Bandel?", "id": "Nakal." },
  { "en": "Bandela?", "id": "Peti kemas." },
  { "en": "Bandeng?", "id": "Ikan." },
  { "en": "Bandering?", "id": "Meriam kecil." },
  { "en": "Bandit?", "id": "Penjahat." },
  { "en": "Bando?", "id": "Hiasan kepala." },
  { "en": "Bandos?", "id": "Kue." },
  { "en": "Bandot?", "id": "Kambing jantan." },
  { "en": "Bandul?", "id": "Pendulum." },
  { "en": "Bandut?", "id": "Tawanan." },
  { "en": "Bangai?", "id": "Terbengkalai." },
  { "en": "Bangar?", "id": "Busuk (kayu)." },
  { "en": "Bangat?", "id": "Sangat." },
  { "en": "Bangau?", "id": "Burung." },
  { "en": "Bangbung?", "id": "Bunyi." },
  { "en": "Banget?", "id": "Sangat." },
  { "en": "Bangga?", "id": "Megah." },
  { "en": "Bangir?", "id": "Mancung." },
  { "en": "Bangka?", "id": "Kaku." },
  { "en": "Bangkai?", "id": "Mayat." },
  { "en": "Bangkang?", "id": "Bantah." },
  { "en": "Bangker?", "id": "Bungker." },
  { "en": "Bangkit?", "id": "Bangun." },
  { "en": "Bangkrut?", "id": "Pailit." },
  { "en": "Bangku?", "id": "Kursi panjang." },
  { "en": "Banglas?", "id": "Luas." },
  { "en": "Bangsa?", "id": "Nasional." },
  { "en": "Bangsal?", "id": "Ruang besar." },
  { "en": "Bangsawan?", "id": "Aristokrat." },
  { "en": "Bangsi?", "id": "Seruling." },
  { "en": "Bangun?", "id": "Sadar." },
  { "en": "Bangunan?", "id": "Gedung." },
  { "en": "Banjaran?", "id": "Deretan." },
  { "en": "Banjar?", "id": "Desa (Bali)." },
  { "en": "Banjir?", "id": "Bah." },
  { "en": "Bantah?", "id": "Sanggah." },
  { "en": "Bantai?", "id": "Bunuh." },
  { "en": "Bantal?", "id": "Alas kepala." },
  { "en": "Bantaran?", "id": "Pinggir sungai." },
  { "en": "Bantat?", "id": "Tidak mengembang." },
  { "en": "Bantau?", "id": "Bantu (arkais)." },
  { "en": "Banteng?", "id": "Sapi hutan." },
  { "en": "Banter?", "id": "Kencang." },
  { "en": "Banting?", "hempas": "Hempas." },
  { "en": "Bantu?", "id": "Tolong." },
  { "en": "Bantuan?", "id": "Pertolongan." },
  { "en": "Bantun?", "id": "Cabut." },
  { "en": "Banyak?", "id": "Jumlah besar." },
  { "en": "Banyar?", "id": "Ikan." },
  { "en": "Banyol?", "id": "Lucu." },
  { "en": "Banyolan?", "id": "Lelucon." },
  { "en": "Banyu?", "id": "Air (Jawa)." },
  { "en": "Bapa?", "id": "Ayah." },
  { "en": "Bapak?", "id": "Ayah." },
  { "en": "Bapanda?", "id": "Ayahanda." },
  { "en": "Bapang?", "id": "Panggilan (laki-laki)." },
  { "en": "Baptis?", "id": "Permandian." },
  { "en": "Bara?", "id": "Api menyala." },
  { "en": "Barah?", "id": "Bisul besar." },
  { "en": "Barai?", "id": "Cerai-berai." },
  { "en": "Baran?", "id": "Bengkak." },
  { "en": "Barang?", "id": "Benda." },
  { "en": "Barangkali?", "id": "Mungkin." },
  { "en": "Barat?", "id": "Arah matahari terbenam." },
  { "en": "Barau-barau?", "id": "Burung." },
  { "en": "Barbar?", "id": "Biadab." },
  { "en": "Barbel?", "id": "Alat angkat beban." },
  { "en": "Barbiturat?", "id": "Obat tidur." },
  { "en": "Barca?", "id": "Perahu." },
  { "en": "Bardi?", "id": "Papirus." },
  { "en": "Bared?", "id": "Luka gores." },
  { "en": "Bareh?", "id": "Beras (Minang)." },
  { "en": "Barel?", "id": "Tong." },
  { "en": "Barem?", "id": "Tapai ketan." },
  { "en": "Barep?", "id": "Sulung (Jawa)." },
  { "en": "Baret?", "id": "Topi." },
  { "en": "Bari-bari?", "id": "Serangga." },
  { "en": "Barid?", "id": "Dingin (Arab)." },
  { "en": "Barik?", "id": "Belang." },
  { "en": "Barikade?", "id": "Rintangan." },
  { "en": "Baring?", "id": "Tidur." },
  { "en": "Baris?", "id": "Deret." },
  { "en": "Barisan?", "id": "Jajaran." },
  { "en": "Barister?", "id": "Pengacara." },
  { "en": "Barit?", "id": "Gores." },
  { "en": "Bariton?", "id": "Suara sedang (pria)." },
  { "en": "Barium?", "id": "Unsur kimia." },
  { "en": "Barkas?", "id": "Perahu motor." },
  { "en": "Barli?", "id": "Jelai." },
  { "en": "Barometer?", "id": "Pengukur tekanan udara." },
  { "en": "Baron?", "id": "Bangsawan." },
  { "en": "Barong?", "id": "Tarian." },
  { "en": "Barongan?", "id": "Kesenian." },
  { "en": "Barongsai?", "id": "Tarian singa." },
  { "en": "Barter?", "id": "Tukar barang." },
  { "en": "Baru?", "id": "Anyar." },
  { "en": "Barua?", "id": "Perantara jahat." },
  { "en": "Baruh?", "id": "Dataran rendah." },
  { "en": "Barun?", "id": "Baru (arkais)." },
  { "en": "Baruna?", "id": "Dewa laut." },
  { "en": "Barusan?", "id": "Tadi." },
  { "en": "Barut?", "id": "Pembalut luka." },
  { "en": "Basah?", "id": "Lembap." },
  { "en": "Basal?", "id": "Dasar." },
  { "en": "Basau?", "id": "Basi (Minang)." },
  { "en": "Basi?", "id": "Tidak segar." },
  { "en": "Basil?", "id": "Bakteri." },
  { "en": "Basis?", "id": "Pangkalan." },
  { "en": "Basmi?", "id": "Berantas." },
  { "en": "Basoka?", "id": "Senjata pelontar." },
  { "en": "Basuh?", "id": "Cuci." },
  { "en": "Basung?", "id": "Kantung (Minang)." },
  { "en": "Bata?", "id": "Batu." },
  { "en": "Batagor?", "id": "Makanan." },
  { "en": "Batal?", "id": "Gagal." },
  { "en": "Batalion?", "id": "Pasukan." },
  { "en": "Batang?", "id": "Tangkai." },
  { "en": "Batara?", "id": "Dewa." },
  { "en": "Batas?", "id": "Had." },
  { "en": "Batek?", "id": "Batik (arkais)." },
  { "en": "Batel?", "id": "Perahu." },
  { "en": "Baterai?", "id": "Sumber daya." },
  { "en": "Bati?", "id": "Untung." },
  { "en": "Batik?", "id": "Kain." },
  { "en": "Batil?", "id": "Wadah." },
  { "en": "Batin?", "id": "Jiwa." },
  { "en": "Batiplon?", "id": "Kapal selam mini." },
  { "en": "Batok?", "id": "Tempurung." },
  { "en": "Baton?", "id": "Tongkat dirigen." },
  { "en": "Batu?", "id": "Wadas." },
  { "en": "Batuan?", "id": "Formasi geologi." },
  { "en": "Batuk?", "id": "Uhuk." },
  { "en": "Batun?", "id": "Susun (arkais)." },
  { "en": "Bau?", "id": "Aroma." },
  { "en": "Baud?", "id": "Sekrup." },
  { "en": "Bauksit?", "id": "Bijih aluminium." },
  { "en": "Baun?", "id": "Bau (arkais)." },
  { "en": "Baur?", "id": "Campur." },
  { "en": "Bausastra?", "id": "Kamus (Jawa)." },
  { "en": "Bausuku?", "id": "Bendahara." },
  { "en": "Baut?", "id": "Mur." },
  { "en": "Bawa?", "id": "Angkut." },
  { "en": "Bawab?", "id": "Penjaga pintu." },
  { "en": "Bawah?", "id": "Dasar." },
  { "en": "Bawahan?", "id": "Anak buah." },
  { "en": "Bawal?", "id": "Ikan." },
  { "en": "Bawang?", "id": "Umbilapis." },
  { "en": "Bawasir?", "id": "Ambeien." },
  { "en": "Bawel?", "id": "Cerewet." },
  { "en": "Baya?", "id": "Usia." },
  { "en": "Bayak?", "id": "Besar." },
  { "en": "Bayam?", "id": "Sayuran." },
  { "en": "Bayan?", "id": "Burung nuri." },
  { "en": "Bayang?", "id": "Imaji." },
  { "en": "Bayangkara?", "id": "Pengawal." },
  { "en": "Bayar?", "id": "Lunas." },
  { "en": "Bayas?", "id": "Pucat." },
  { "en": "Bayat?", "id": "Basi." },
  { "en": "Bayati?", "id": "Lagu (Arab)." },
  { "en": "Bayi?", "id": "Orok." },
  { "en": "Bayonet?", "id": "Senjata tajam." },
  { "en": "Bayu?", "id": "Angin." },
  { "en": "Bazar?", "id": "Pasar amal." },
  { "en": "Bea?", "id": "Cukai." },
  { "en": "Beasiswa?", "id": "Tunjangan belajar." },
  { "en": "Bebal?", "id": "Bodoh." },
  { "en": "Beban?", "id": "Muatan." },
  { "en": "Bebar?", "id": "Buyar." },
  { "en": "Bebas?", "id": "Merdeka." },
  { "en": "Bebat?", "id": "Pembalut." },
  { "en": "Bebek?", "id": "Itik." },
  { "en": "Beber?", "id": "Bentang." },
  { "en": "Beberapa?", "id": "Sejumlah." },
  { "en": "Becak?", "id": "Kendaraan roda tiga." },
  { "en": "Becek?", "id": "Berair." },
  { "en": "Becokok?", "id": "Buaya (arkais)." },
  { "en": "Becuk?", "id": "Ikan." },
  { "en": "Becus?", "id": "Mampu." },
  { "en": "Beda?", "id": "Lain." },
  { "en": "Bedah?", "id": "Operasi." },
  { "en": "Bedak?", "id": "Pupur." },
  { "en": "Bedama?", "id": "Pedang pendek." },
  { "en": "Bedan?", "id": "Bidan (arkais)." },
  { "en": "Bedar?", "id": "Perahu." },
  { "en": "Bedaru?", "id": "Kayu." },
  { "en": "Bedawi?", "id": "Nomaden." },
  { "en": "Bedaya?", "id": "Tarian." },
  { "en": "Bedebah?", "id": "Celaka." },
  { "en": "Bedegap?", "id": "Tegap." },
  { "en": "Bedel?", "id": "Operasi (Belanda)." },
  { "en": "Bedeng?", "id": "Barak." },
  { "en": "Bedi?", "id": "Senapan." },
  { "en": "Bedian?", "id": "Senjata." },
  { "en": "Bedil?", "id": "Senjata api." },
  { "en": "Bedinde?", "id": "Pelayan wanita." },
  { "en": "Bedol?", "id": "Cabut." },
  { "en": "Bedu?", "id": "Genderang." },
  { "en": "Beduin?", "id": "Pengembara Arab." },
  { "en": "Begadang?", "id": "Tidak tidur." },
  { "en": "Begah?", "id": "Kenyang sekali." },
  { "en": "Begal?", "id": "Perampok." },
  { "en": "Begana?", "id": "Lauk (arkais)." },
  { "en": "Begar?", "id": "Keras." },
  { "en": "Begawan?", "id": "Pertapa." },
  { "en": "Begeg?", "id": "Kaku (Minang)." },
  { "en": "Begini?", "id": "Seperti ini." },
  { "en": "Begitu?", "id": "Seperti itu." },
  { "en": "Bego?", "id": "Bodoh (kasar)." },
  { "en": "Begonia?", "id": "Tanaman hias." },
  { "en": "Beha?", "id": "Kutang." },
  { "en": "Behandel?", "id": "Tangani (Belanda)." },
  { "en": "Bekal?", "id": "Sangu." },
  { "en": "Bekam?", "id": "Terapi." },
  { "en": "Bekantan?", "id": "Monyet hidung panjang." },
  { "en": "Bekas?", "id": "Jejak." },
  { "en": "Bekat?", "id": "Penuh (arkais)." },
  { "en": "Bekatul?", "id": "Dedak halus." },
  { "en": "Bekel?", "id": "Lurah desa (Jawa)." },
  { "en": "Beker?", "id": "Jam weker." },
  { "en": "Bekernyut?", "id": "Mengerut." },
  { "en": "Bekicot?", "id": "Siput." },
  { "en": "Beking?", "id": "Pelindung." },
  { "en": "Bekisar?", "id": "Ayam hutan." },
  { "en": "Bekleding?", "id": "Pelapis (Belanda)." },
  { "en": "Bekles?", "id": "Lari terbirit-birit." },
  { "en": "Beku?", "id": "Padat (karena dingin)." },
  { "en": "Bekuk?", "id": "Tangkap." },
  { "en": "Belabas?", "id": "Sutra (arkais)." },
  { "en": "Belacan?", "id": "Terasi." },
  { "en": "Belacu?", "id": "Kain kasar." },
  { "en": "Beladau?", "id": "Pisau." },
  { "en": "Belah?", "id": "Pecah." },
  { "en": "Belai?", "id": "Elus." },
  { "en": "Belajar?", "id": "Menuntut ilmu." },
  { "en": "Belak?", "id": "Belang." },
  { "en": "Belaka?", "id": "Semata-mata." },
  { "en": "Belakang?", "id": "Punggung." },
  { "en": "Belakin?", "id": "Bergetah." },
  { "en": "Belalah?", "id": "Rakus." },
  { "en": "Belalak?", "id": "Terbuka lebar (mata)." },
  { "en": "Belalang?", "id": "Serangga." },
  { "en": "Belan?", "id": "Bersinar (arkais)." },
  { "en": "Belanda?", "id": "Negeri kincir angin." },
  { "en": "Belandar?", "id": "Balok penyangga." },
  { "en": "Belandong?", "id": "Penebang kayu." },
  { "en": "Belang?", "id": "Loreng." },
  { "en": "Belanga?", "id": "Kuali tanah." },
  { "en": "Belangah?", "id": "Ternganga." },
  { "en": "Belangkas?", "id": "Kepiting tapal kuda." },
  { "en": "Belanja?", "id": "Beli." },
  { "en": "Belantai?", "id": "Pukul (dengan rotan)." },
  { "en": "Belantan?", "id": "Pentungan." },
  { "en": "Belantara?", "id": "Hutan lebat." },
  { "en": "Belantik?", "id": "Pedagang perantara." },
  { "en": "Belas?", "id": "Kasihan." },
  { "en": "Belasan?", "id": "Sebelas hingga sembilan belas." },
  { "en": "Belat?", "id": "Alat penahan." },
  { "en": "Belati?", "id": "Pisau tajam." },
  { "en": "Belatuk?", "id": "Burung pelatuk." },
  { "en": "Belebas?", "id": "Mistari." },
  { "en": "Belebat?", "id": "Tongkat." },
  { "en": "Beleda?", "id": "Kain beludru." },
  { "en": "Beleganjur?", "id": "Musik Bali." },
  { "en": "Belek?", "id": "Kaleng." },
  { "en": "Belekan?", "id": "Sakit mata." },
  { "en": "Belelang?", "id": "Lelang (arkais)." },
  { "en": "Belel?", "id": "Kumuh." },
  { "en": "Belembang?", "id": "Kotor." },
  { "en": "Beleng?", "id": "Putar (arkais)." },
  { "en": "Belenggu?", "id": "Ikatan." },
  { "en": "Belengket?", "id": "Lengket." },
  { "en": "Belenting?", "id": "Lenting." },
  { "en": "Belerang?", "id": "Sulfur." },
  { "en": "Belerong?", "id": "Balai (arkais)." },
  { "en": "Belester?", "id": "Plester." },
  { "en": "Belet?", "id": "Bodoh (Belanda)." },
  { "en": "Beli?", "id": "Tukar dengan uang." },
  { "en": "Belia?", "id": "Muda." },
  { "en": "Beliak?", "id": "Belalak." },
  { "en": "Belian?", "id": "Dukun." },
  { "en": "Beliau?", "id": "Dia (hormat)." },
  { "en": "Belibas?", "id": "Pedang." },
  { "en": "Belibat?", "id": "Tipu muslihat." },
  { "en": "Belibis?", "id": "Itik liar." },
  { "en": "Belida?", "id": "Ikan." },
  { "en": "Belikat?", "id": "Tulang bahu." },
  { "en": "Beligo?", "id": "Labu." },
  { "en": "Belikas?", "id": "Alat pintal." },
  { "en": "Belimbing?", "id": "Buah." },
  { "en": "Beling?", "id": "Kaca." },
  { "en": "Belingsat?", "id": "Gelisah." },
  { "en": "Belinjo?", "id": "Melinjo." },
  { "en": "Belintang?", "id": "Melintang." },
  { "en": "Belis?", "id": "Iblis (arkais)." },
  { "en": "Belit?", "id": "Lilit." },
  { "en": "Belitung?", "id": "Siput." },
  { "en": "Beliut?", "id": "Berkelok." },
  { "en": "Belok?", "id": "Tikung." },
  { "en": "Belolang?", "id": "Kain sutra." },
  { "en": "Belolok?", "id": "Buah enau muda." },
  { "en": "Belon?", "id": "Balon (arkais)." },
  { "en": "Belonggok?", "id": "Tumpuk." },
  { "en": "Belongkang?", "id": "Perahu." },
  { "en": "Belontang?", "id": "Melotot." },
  { "en": "Belu-belai?", "id": "Bujuk rayu." },
  { "en": "Beluam?", "id": "Kenyang (arkais)." },
  { "en": "Beluas?", "id": "Luas." },
  { "en": "Belubu?", "id": "Lubuk." },
  { "en": "Beludak?", "id": "Ular berbisa." },
  { "en": "Beludur?", "id": "Beludru." },
  { "en": "Belukap?", "id": "Bakau." },
  { "en": "Belukar?", "id": "Semak." },
  { "en": "Belulang?", "id": "Kulit kering." },
  { "en": "Belum?", "id": "Tidak lagi." },
  { "en": "Belungkang?", "id": "Pelangi." },
  { "en": "Belungkur?", "id": "Ikan." },
  { "en": "Beluntas?", "id": "Tanaman." },
  { "en": "Beluru?", "id": "Pohon." },
  { "en": "Belus?", "id": "Belut." },
  { "en": "Belut?", "id": "Ikan." },
  { "en": "Bemban?", "id": "Tanaman." },
  { "en": "Bemo?", "id": "Kendaraan." },
  { "en": "Bena?", "id": "Patut." },
  { "en": "Benak?", "id": "Otak." },
  { "en": "Benalu?", "id": "Parasit." },
  { "en": "Benam?", "id": "Tenggelam." },
  { "en": "Benang?", "id": "Utas." },
  { "en": "Benar?", "id": "Betul." },
  { "en": "Bencah?", "id": "Rawa." },
  { "en": "Bencana?", "id": "Musibah." },
  { "en": "Benci?", "id": "Tidak suka." },
  { "en": "Benda?", "id": "Barang." },
  { "en": "Bendahara?", "id": "Penyimpan uang." },
  { "en": "Bendahari?", "id": "Bendahara (arkais)." },
  { "en": "Bendel?", "id": "Berkas." },
  { "en": "Bendera?", "id": "Panji." },
  { "en": "Benderang?", "id": "Terang." },
  { "en": "Bendi?", "id": "Delman." },
  { "en": "Bendir?", "id": "Gong kecil." },
  { "en": "Bendo?", "id": "Golok." },
  { "en": "Bendu?", "id": "Sahabat (arkais)." },
  { "en": "Benduan?", "id": "Orang hukuman." },
  { "en": "Bendul?", "id": "Ambang." },
  { "en": "Bengah?", "id": "Sombong (arkais)." },
  { "en": "Bengak?", "id": "Tuli." },
  { "en": "Bengang?", "id": "Terbuka." },
  { "en": "Bengap?", "id": "Sesak napas." },
  { "en": "Bengawan?", "id": "Sungai besar." },
  { "en": "Bengkak?", "id": "Bengkak." },
  { "en": "Bengkal?", "id": "Tersangkut." },
  { "en": "Bengkalai?", "id": "Terbengkalai." },
  { "en": "Bengkang?", "id": "Kue." },
  { "en": "Bengkar?", "id": "Mekar." },
  { "en": "Bengkarak?", "id": "Kerangka." },
  { "en": "Bengkarung?", "id": "Kadal." },
  { "en": "Bengkel?", "id": "Tempat reparasi." },
  { "en": "Bengkelai?", "id": "Bertengkar." },
  { "en": "Bengkerap?", "id": "Bangkrut (arkais)." },
  { "en": "Bengkil?", "id": "Sompek." },
  { "en": "Bengkok?", "id": "Lengkung." },
  { "en": "Bengkol?", "id": "Kelok." },
  { "en": "Bengkong?", "id": "Sipilis (arkais)." },
  { "en": "Bengkuang?", "id": "Buah." },
  { "en": "Bengkuk?", "id": "Bongkok." },
  { "en": "Bengkung?", "id": "Kain ikat pinggang." },
  { "en": "Bengong?", "id": "Termenung." },
  { "en": "Bengot?", "id": "Herot." },
  { "en": "Bengu?", "id": "Bau tak sedap." },
  { "en": "Benguk?", "id": "Muram." },
  { "en": "Benian?", "id": "Peti." },
  { "en": "Benih?", "id": "Bibit." },
  { "en": "Bening?", "id": "Jernih." },
  { "en": "Benitan?", "id": "Pukulan." },
  { "en": "Benjamin?", "id": "Kemenyan." },
  { "en": "Benjol?", "id": "Bengkak." },
  { "en": "Bensin?", "id": "Bahan bakar." },
  { "en": "Bentala?", "id": "Bumi." },
  { "en": "Bentan?", "id": "Kambuh." },
  { "en": "Bentang?", "id": "Hampar." },
  { "en": "Bentangan?", "id": "Hamparan." },
  { "en": "Bentara?", "id": "Ajudan." },
  { "en": "Bentaus?", "id": "Tembus." },
  { "en": "Benteng?", "id": "Kubu." },
  { "en": "Bentik?", "id": "Lekuk." },
  { "en": "Bentoh?", "id": "Perut." },
  { "en": "Bentok?", "id": "Senggol." },
  { "en": "Bentol?", "id": "Benjol." },
  { "en": "Bentonit?", "id": "Lempung." },
  { "en": "Bentos?", "id": "Dasar laut." },
  { "en": "Bentrok?", "id": "Tabrakan." },
  { "en": "Bentuk?", "id": "Rupa." },
  { "en": "Bentulu?", "id": "Penjara (arkais)." },
  { "en": "Bentur?", "id": "Tabrak." },
  { "en": "Benturung?", "id": "Binturung." },
  { "en": "Benua?", "id": "Daratan luas." },
  { "en": "Benum?", "id": "Padat." },
  { "en": "Benur?", "id": "Bibit udang." },
  { "en": "Beo?", "id": "Burung peniru." },
  { "en": "Berabe?", "id": "Sulit." },
  { "en": "Berahi?", "id": "Nafsu." },
  { "en": "Berak?", "id": "Buang air besar." },
  { "en": "Berakah?", "id": "Sombong." },
  { "en": "Beranda?", "id": "Teras." },
  { "en": "Berandal?", "id": "Penjahat." },
  { "en": "Berandang?", "id": "Mencolok." },
  { "en": "Berang?", "id": "Marah." },
  { "en": "Berangan?", "id": "Kastanye." },
  { "en": "Berangsang?", "id": "Marah." },
  { "en": "Berangta?", "id": "Nafsu (arkais)." },
  { "en": "Berani?", "id": "Gagah." },
  { "en": "Beranta?", "id": "Perahu." },
  { "en": "Berantas?", "id": "Basmi." },
  { "en": "Berapa?", "id": "Jumlah." },
  { "en": "Beras?", "id": "Padi giling." },
  { "en": "Berat?", "id": "Bobot." },
  { "en": "Berdikari?", "id": "Mandiri." },
  { "en": "Berdus?", "id": "Bertinju." },
  { "en": "Berembang?", "id": "Pohon." },
  { "en": "Bereng-bereng?", "id": "Keranjang." },
  { "en": "Berengau?", "id": "Ulat." },
  { "en": "Berenggil?", "id": "Tidak rata." },
  { "en": "Berengsek?", "id": "Kurang ajar." },
  { "en": "Berentang?", "id": "Merentang." },
  { "en": "Bereo?", "id": "Parau." },
  { "en": "Beres?", "id": "Selesai." },
  { "en": "Beresok?", "id": "Keesokan hari." },
  { "en": "Beret?", "id": "Pangkat (militer)." },
  { "en": "Bergajul?", "id": "Nakal." },
  { "en": "Bergaya?", "id": "Modis." },
  { "en": "Bergedel?", "id": "Perkedel." },
  { "en": "Bergidik?", "id": "Merinding." },
  { "en": "Bergola?", "id": "Pintu gerbang." },
  { "en": "Bergulir?", "id": "Berputar." },
  { "en": "Berhala?", "id": "Arca sembahan." },
  { "en": "Beri?", "id": "Kasih." },
  { "en": "Beria?", "id": "Penyakit." },
  { "en": "Beringas?", "id": "Ganas." },
  { "en": "Beringin?", "id": "Pohon." },
  { "en": "Berisik?", "id": "Gaduh." },
  { "en": "Berita?", "id": "Kabar." },
  { "en": "Berkah?", "id": "Rahmat." },
  { "en": "Berkas?", "id": "Bundel." },
  { "en": "Berkat?", "id": "Karena." },
  { "en": "Berkelah?", "id": "Bertengkar (arkais)." },
  { "en": "Berkemul?", "id": "Berselimut." },
  { "en": "Berlauh?", "id": "Asrama." },
  { "en": "Berlendir?", "id": "Berliur." },
  { "en": "Berlian?", "id": "Intan." },
  { "en": "Berma?", "id": "Dewa (arkais)." },
  { "en": "Bermi?", "id": "Cacing." },
  { "en": "Bernas?", "id": "Berisi." },
  { "en": "Beroga?", "id": "Gajah (arkais)." },
  { "en": "Berok?", "id": "Monyet." },
  { "en": "Berokat?", "id": "Brokat." },
  { "en": "Berondok?", "id": "Sembunyi." },
  { "en": "Berong?", "id": "Lubang." },
  { "en": "Berongsang?", "id": "Marah-marah." },
  { "en": "Beronok?", "id": "Teripang." },
  { "en": "Berontak?", "id": "Melawan." },
  { "en": "Beroti?", "id": "Balok kayu." },
  { "en": "Bersih?", "id": "Suci." },
  { "en": "Bersil?", "id": "Menonjol." },
  { "en": "Bersin?", "id": "Wahing." },
  { "en": "Bersit?", "id": "Muncul tiba-tiba." },
  { "en": "Bersut?", "id": "Cemberut." },
  { "en": "Bertam?", "id": "Palma." },
  { "en": "Bertih?", "id": "Beras goreng." },
  { "en": "Beruang?", "id": "Hewan mamalia." },
  { "en": "Beruas?", "id": "Berbuku-buku." },
  { "en": "Berubuh?", "id": "Runtuh." },
  { "en": "Berudu?", "id": "Kecebong." },
  { "en": "Berui?", "id": "Penjara (arkais)." },
  { "en": "Beruju?", "id": "Menuju." },
  { "en": "Beruk?", "id": "Kera." },
  { "en": "Berumbun?", "id": "Tumpuk." },
  { "en": "Berungut?", "id": "Menggerutu." },
  { "en": "Beruntun?", "id": "Berturut-turut." },
  { "en": "Beruri?", "id": "Pidato (arkais)." },
  { "en": "Berus?", "id": "Sikat." },
  { "en": "Besalen?", "id": "Tempat kerja pandai besi." },
  { "en": "Besan?", "id": "Ipar." },
  { "en": "Besar?", "id": "Agung." },
  { "en": "Besek?", "id": "Wadah anyaman bambu." },
  { "en": "Besel?", "id": "Uang suap." },
  { "en": "Besengek?", "id": "Masakan." },
  { "en": "Beser?", "id": "Sering kencing." },
  { "en": "Beset?", "id": "Kupas kulit." },
  { "en": "Besi?", "id": "Logam." },
  { "en": "Besing?", "id": "Bising." },
  { "en": "Besit?", "id": "Sayat." },
  { "en": "Beskal?", "id": "Jaksa (arkais)." },
  { "en": "Beskap?", "id": "Jas Jawa." },
  { "en": "Beslah?", "id": "Sita." },
  { "en": "Beslit?", "id": "Surat keputusan." },
  { "en": "Besok?", "id": "Lusa (jika dari kemarin)." },
  { "en": "Bestari?", "id": "Cerdas." },
  { "en": "Bestek?", "id": "Rancangan bangunan." },
  { "en": "Bestel?", "id": "Pesanan." },
  { "en": "Bestialitas?", "id": "Hubungan dengan hewan." },
  { "en": "Bestir?", "id": "Pengurus (Belanda)." },
  { "en": "Besusu?", "id": "Kura-kura." },
  { "en": "Besut?", "id": "Halus." },
  { "en": "Beta?", "id": "Saya (Indonesia Timur)." },
  { "en": "Betah?", "id": "Tahan." },
  { "en": "Betapa?", "id": "Alangkah." },
  { "en": "Betara?", "id": "Dewa." },
  { "en": "Betas?", "id": "Retas." },
  { "en": "Betatas?", "id": "Ubi jalar." },
  { "en": "Betau?", "id": "Sapa." },
  { "en": "Betek?", "id": "Jengkel." },
  { "en": "Betet?", "id": "Burung." },
  { "en": "Beti?", "id": "Bukti (arkais)." },
  { "en": "Betik?", "id": "Pepaya." },
  { "en": "Betina?", "id": "Perempuan (hewan)." },
  { "en": "Beting?", "id": "Gundukan pasir." },
  { "en": "Betis?", "id": "Bagian kaki." },
  { "en": "Betok?", "id": "Ikan." },
  { "en": "Beton?", "id": "Coran semen." },
  { "en": "Betot?", "id": "Cabut paksa." },
  { "en": "Betres?", "id": "Kain (arkais)." },
  { "en": "Betul?", "id": "Benar." },
  { "en": "Betung?", "id": "Bambu besar." },
  { "en": "Betutu?", "id": "Masakan ayam." },
  { "en": "Biadab?", "id": "Barbar." },
  { "en": "Biadat?", "id": "Kurang ajar." },
  { "en": "Biak?", "id": "Berkembang biak." },
  { "en": "Biang?", "id": "Induk." },
  { "en": "Bianglala?", "id": "Pelangi." },
  { "en": "Biaperi?", "id": "Pedagang." },
  { "en": "Biar?", "id": "Supaya." },
  { "en": "Bias?", "id": "Belokan cahaya." },
  { "en": "Biasa?", "id": "Lumrah." },
  { "en": "Biat?", "id": "Biara." },
  { "en": "Biawak?", "id": "Reptil." },
  { "en": "Biaya?", "id": "Ongkos." },
  { "en": "Bibel?", "id": "Alkitab." },
  { "en": "Bibi?", "id": "Tante." },
  { "en": "Bibir?", "id": "Bagian mulut." },
  { "en": "Bibit?", "id": "Benih." },
  { "en": "Bicara?", "id": "Tutur." },
  { "en": "Bicu?", "id": "Dongkrak." },
  { "en": "Bida?", "id": "Halang (arkais)." },
  { "en": "Bidadari?", "id": "Peri." },
  { "en": "Bidal?", "id": "Pepatah." },
  { "en": "Bidam?", "id": "Candu (arkais)." },
  { "en": "Bidan?", "id": "Penolong persalinan." },
  { "en": "Bidang?", "id": "Area." },
  { "en": "Bidar?", "id": "Perahu." },
  { "en": "Bidas?", "id": "Lontar." },
  { "en": "Bidik?", "id": "Arahkan." },
  { "en": "Biduan?", "id": "Penyanyi pria." },
  { "en": "Biduanita?", "id": "Penyanyi wanita." },
  { "en": "Biduk?", "id": "Perahu kecil." },
  { "en": "Bidur?", "id": "Timah." },
  { "en": "Cabai?", "id": "Lada." },
  { "en": "Cabak?", "id": "Burung." },
  { "en": "Cabang?", "id": "Ranting." },
  { "en": "Cabar?", "id": "Tawar hati." },
  { "en": "Cabau?", "id": "Kain kasar (arkais)." },
  { "en": "Cabe?", "id": "Cabai." },
  { "en": "Cabik?", "id": "Sobek." },
  { "en": "Cabo?", "id": "Germo (arkais)." },
  { "en": "Cabuh?", "id": "Kacau." },
  { "en": "Cabuk?", "id": "Pecut." },
  { "en": "Cabul?", "id": "Tidak senonoh." },
  { "en": "Cabur?", "id": "Cebur." },
  { "en": "Cabut?", "id": "Tarik." },
  { "en": "Caca?", "id": "Cacat (arkais)." },
  { "en": "Cacah?", "id": "Jumlah." },
  { "en": "Cacah jiwa?", "id": "Sensus penduduk." },
  { "en": "Cacak?", "id": "Tegak." },
  { "en": "Cacar?", "id": "Penyakit kulit." },
  { "en": "Cacat?", "id": "Rusak." },
  { "en": "Cacau?", "id": "Kacau." },
  { "en": "Caci?", "id": "Maki." },
  { "en": "Cacing?", "id": "Hewan tanah." },
  { "en": "Cadai?", "id": "Gurauan (arkais)." },
  { "en": "Cadang?", "id": "Sedia." },
  { "en": "Cadar?", "id": "Penutup muka." },
  { "en": "Cadas?", "id": "Batu keras." },
  { "en": "Cadel?", "id": "Pelat." },
  { "en": "Cadik?", "id": "Keseimbangan perahu." },
  { "en": "Cadok?", "id": "Rabun dekat." },
  { "en": "Caduk?", "id": "Dungu (arkais)." },
  { "en": "Caem?", "id": "Cantik (prokem)." },
  { "en": "Cagak?", "id": "Tiang penyangga." },
  { "en": "Cagar?", "id": "Panjar." },
  { "en": "Cager?", "id": "Sehat (Sunda)." },
  { "en": "Cagil?", "id": "Nakal (arkais)." },
  { "en": "Caguh?", "id": "Angkuh (arkais)." },
  { "en": "Cagut?", "id": "Patuk." },
  { "en": "Cah?", "id": "Air (arkais)." },
  { "en": "Cahang?", "id": "Sombong (Minang)." },
  { "en": "Cahari?", "id": "Cari (arkais)." },
  { "en": "Cahaya?", "id": "Sinar." },
  { "en": "Cailah?", "id": "Seruan." },
  { "en": "Cair?", "id": "Encer." },
  { "en": "Cak?", "id": "Kakak (laki-laki)." },
  { "en": "Cakap?", "id": "Pandai." },
  { "en": "Cakar?", "id": "Kuku tajam." },
  { "en": "Cakawari?", "id": "Roda (arkais)." },
  { "en": "Caki?", "id": "Kakek buyut (arkais)." },
  { "en": "Cakil?", "id": "Tokoh wayang." },
  { "en": "Cako?", "id": "Baju jas." },
  { "en": "Cakra?", "id": "Roda." },
  { "en": "Cakrabuana?", "id": "Jagad raya." },
  { "en": "Cakram?", "id": "Piringan." },
  { "en": "Cakrawala?", "id": "Ufuk." },
  { "en": "Cakruk?", "id": "Gardu ronda." },
  { "en": "Caku?", "id": "Tumbuk (arkais)." },
  { "en": "Cakup?", "id": "Rangkum." },
  { "en": "Calak?", "id": "Cerdas (Minang)." },
  { "en": "Calang?", "id": "Perkakas (arkais)." },
  { "en": "Calar?", "id": "Gores." },
  { "en": "Calecer?", "id": "Bor." },
  { "en": "Caliak?", "id": "Lihat (Minang)." },
  { "en": "Calir?", "id": "Berair (arkais)." },
  { "en": "Calit?", "id": "Oles." },
  { "en": "Calo?", "id": "Perantara." },
  { "en": "Calon?", "id": "Kandidat." },
  { "en": "Calui?", "id": "Licin (arkais)." },
  { "en": "Caluk?", "id": "Alat gali." },
  { "en": "Calung?", "id": "Alat musik." },
  { "en": "Cam?", "id": "Perhatikan." },
  { "en": "Camang?", "id": "Anak buah (arkais)." },
  { "en": "Camar?", "id": "Burung laut." },
  { "en": "Camat?", "id": "Kepala kecamatan." },
  { "en": "Cambahan?", "id": "Tauge." },
  { "en": "Cambang?", "id": "Bauk." },
  { "en": "Cambuk?", "id": "Pecut." },
  { "en": "Cambul?", "id": "Menonjol." },
  { "en": "Camik?", "id": "Lezat (arkais)." },
  { "en": "Campa?", "id": "Kerajaan kuno." },
  { "en": "Campah?", "id": "Hambar." },
  { "en": "Campak?", "id": "Lempar." },
  { "en": "Campang?", "id": "Dayung (arkais)." },
  { "en": "Campin?", "id": "Ahli." },
  { "en": "Camping?", "id": "Robek." },
  { "en": "Camplungan?", "id": "Nyemplung." },
  { "en": "Campuh?", "id": "Lumat." },
  { "en": "Campur?", "id": "Aduk." },
  { "en": "Camur?", "id": "Kacau (arkais)." },
  { "en": "Canai?", "id": "Asah." },
  { "en": "Canang?", "id": "Gong kecil." },
  { "en": "Canda?", "id": "Kelakar." },
  { "en": "Candala?", "id": "Hina (Sanskerta)." },
  { "en": "Candat?", "id": "Kait." },
  { "en": "Candi?", "id": "Bangunan suci." },
  { "en": "Candik?", "id": "Dayang (arkais)." },
  { "en": "Candit?", "id": "Kait perahu." },
  { "en": "Candra?", "id": "Bulan (sastra)." },
  { "en": "Candradimuka?", "id": "Kawah (mitologi)." },
  { "en": "Candramawa?", "id": "Hitam (bulu kucing)." },
  { "en": "Candrasa?", "id": "Pedang (sastra)." },
  { "en": "Candu?", "id": "Narkotika." },
  { "en": "Candung?", "id": "Parang." },
  { "en": "Cang?", "id": "Cerek." },
  { "en": "Canggah?", "id": "Ranting." },
  { "en": "Canggai?", "id": "Kuku panjang." },
  { "en": "Canggal?", "id": "Kasar (arkais)." },
  { "en": "Canggung?", "id": "Kaku." },
  { "en": "Canggih?", "id": "Modern." },
  { "en": "Cangkang?", "id": "Kulit keras." },
  { "en": "Cangkel?", "id": "Ikat (arkais)." },
  { "en": "Cangkih?", "id": "Tidak seimbang (arkais)." },
  { "en": "Cangkir?", "id": "Gelas bertangkai." },
  { "en": "Cangking?", "id": "Jinjing." },
  { "en": "Cangkok?", "id": "Okulasi." },
  { "en": "Cangkol?", "id": "Kait." },
  { "en": "Cangkriman?", "id": "Tebakan (Jawa)." },
  { "en": "Cangkring?", "id": "Pohon." },
  { "en": "Cangkuk?", "id": "Alat pengait." },
  { "en": "Cangkul?", "id": "Alat gali." },
  { "en": "Cangkum?", "id": "Rangkum (arkais)." },
  { "en": "Cangkung?", "id": "Jongkok." },
  { "en": "Cantel?", "id": "Kait." },
  { "en": "Canteng?", "id": "Bisul kecil." },
  { "en": "Cantik?", "id": "Elok." },
  { "en": "Canting?", "id": "Alat membatik." },
  { "en": "Cantol?", "id": "Gantung." },
  { "en": "Cantrik?", "id": "Murid padepokan." },
  { "en": "Cantum?", "id": "Sertakan." },
  { "en": "Cap?", "id": "Stempel." },
  { "en": "Capa?", "id": "Permainan (arkais)." },
  { "en": "Capah?", "id": "Nampan (arkais)." },
  { "en": "Capai?", "id": "Raih." },
  { "en": "Capak?", "id": "Remeh (arkais)." },
  { "en": "Capal?", "id": "Sandal kulit." },
  { "en": "Capang?", "id": "Menjulur (kumis)." },
  { "en": "Capek?", "id": "Lelah." },
  { "en": "Capelin?", "id": "Ikan." },
  { "en": "Capgome?", "id": "Perayaan Imlek." },
  { "en": "Capiau?", "id": "Topi (arkais)." },
  { "en": "Capik?", "id": "Pincang (arkais)." },
  { "en": "Caping?", "id": "Topi petani." },
  { "en": "Caplak?", "id": "Kutu hewan." },
  { "en": "Caplok?", "id": "Telan." },
  { "en": "Capuk?", "id": "Bopeng." },
  { "en": "Cara?", "id": "Metode." },
  { "en": "Carak?", "id": "Tempat sirih." },
  { "en": "Caraka?", "id": "Utusan." },
  { "en": "Caran?", "id": "Pipa rokok." },
  { "en": "Carang?", "id": "Jarang (gigi)." },
  { "en": "Cari?", "id": "Temukan." },
  { "en": "Carik?", "id": "Sekretaris desa." },
  { "en": "Carter?", "id": "Sewa." },
  { "en": "Caruk?", "id": "Sayat." },
  { "en": "Carut?", "id": "Keji." },
  { "en": "Cas?", "id": "Isi daya." },
  { "en": "Cat?", "id": "Pewarna." },
  { "en": "Catat?", "id": "Tulis." },
  { "en": "Cate?", "id": "Permainan." },
  { "en": "Catu?", "id": "Jatah." },
  { "en": "Catuk?", "id": "Patuk." },
  { "en": "Catur?", "id": "Permainan papan." },
  { "en": "Catut?", "id": "Tang." },
  { "en": "Caung?", "id": "Pucat (arkais)." },
  { "en": "Cawak?", "id": "Lesung pipi." },
  { "en": "Cawan?", "id": "Mangkuk kecil." },
  { "en": "Cawat?", "id": "Kain penutup kemaluan." },
  { "en": "Cawis?", "id": "Sedia (Jawa)." },
  { "en": "Ce?", "id": "Kakak perempuan (Tionghoa)." },
  { "en": "Cebak?", "id": "Gali (tambang)." },
  { "en": "Cebar-cebur?", "id": "Bunyi air." },
  { "en": "Cebik?", "id": "Cibir." },
  { "en": "Cebil?", "id": "Kerdil (arkais)." },
  { "en": "Cebok?", "id": "Bersihkan (setelah buang air)." },
  { "en": "Cebol?", "id": "Kerdil." },
  { "en": "Cebur?", "id": "Masuk air." },
  { "en": "Cecah?", "id": "Sayat halus." },
  { "en": "Cecak?", "id": "Cicak." },
  { "en": "Cecap?", "id": "Kecap (rasa)." },
  { "en": "Cece?", "id": "Kakak perempuan (Peranakan)." },
  { "en": "Ceceh?", "id": "Payudara (kasar)." },
  { "en": "Cecer?", "id": "Tumpah." },
  { "en": "Cecunguk?", "id": "Penyusup." },
  { "en": "Cedal?", "id": "Sumbing." },
  { "en": "Cedayam?", "id": "Luka (arkais)." },
  { "en": "Ceder?", "id": "Luka." },
  { "en": "Cedera?", "id": "Luka." },
  { "en": "Ceding?", "id": "Kurus." },
  { "en": "Cedok?", "id": "Sendok besar." },
  { "en": "Ceflon?", "id": "Kain (arkais)." },
  { "en": "Cegah?", "id": "Halang." },
  { "en": "Cegak?", "id": "Kuat (arkais)." },
  { "en": "Cegar?", "id": "Kasar (tanah)." },
  { "en": "Cegat?", "id": "Hadang." },
  { "en": "Cek?", "id": "Periksa." },
  { "en": "Cekah?", "id": "Retak." },
  { "en": "Cekak?", "id": "Pendek." },
  { "en": "Cekal?", "id": "Tahan." },
  { "en": "Cekam?", "id": "Cengkeram." },
  { "en": "Cekatan?", "id": "Tangkas." },
  { "en": "Cekau?", "id": "Tangkap." },
  { "en": "Cekdam?", "id": "Bendungan kecil." },
  { "en": "Cekel?", "id": "Pelit (Jawa)." },
  { "en": "Ceker?", "id": "Kaki ayam." },
  { "en": "Ceki?", "id": "Permainan kartu." },
  { "en": "Cekibar?", "id": "Kibar (arkais)." },
  { "en": "Cekik?", "id": "Penggal." },
  { "en": "Cekit?", "id": "Sedikit." },
  { "en": "Ceklek?", "id": "Bunyi sakelar." },
  { "en": "Cekok?", "id": "Minum paksa." },
  { "en": "Cekos?", "id": "Kain pembersih." },
  { "en": "Ceku?", "id": "Tekan (arkais)." },
  { "en": "Cekuh?", "id": "Ambil sedikit." },
  { "en": "Cekung?", "id": "Lekuk ke dalam." },
  { "en": "Cekup?", "id": "Tangkap dengan tangan." },
  { "en": "Cekur?", "id": "Kencur." },
  { "en": "Cekut?", "id": "Ambil cepat." },
  { "en": "Cela?", "id": "Cacat." },
  { "en": "Celaga?", "id": "Tingkah (arkais)." },
  { "en": "Celah?", "id": "Rongga." },
  { "en": "Celak?", "id": "Pewarna alis." },
  { "en": "Celaka?", "id": "Nahas." },
  { "en": "Celam-celum?", "id": "Keluar masuk." },
  { "en": "Celana?", "id": "Pantalon." },
  { "en": "Celang?", "id": "Terbuka (mata)." },
  { "en": "Celangak?", "id": "Ternganga." },
  { "en": "Celapak?", "id": "Duduk bersila." },
  { "en": "Celopar?", "id": "Cerewet." },
  { "en": "Celar?", "id": "Loreng (harimau)." },
  { "en": "Celari?", "id": "Seledri (arkais)." },
  { "en": "Celaru?", "id": "Kacau." },
  { "en": "Celat?", "id": "Lompat." },
  { "en": "Celathu?", "id": "Bicara." },
  { "en": "Cele?", "id": "Kain." },
  { "en": "Celebuk?", "id": "Bunyi jatuh." },
  { "en": "Celekeh?", "id": "Kotor." },
  { "en": "Celek?", "id": "Mata (arkais)." },
  { "en": "Celemek?", "id": "Apron." },
  { "en": "Celempong?", "id": "Alat musik." },
  { "en": "Celen?", "id": "Celeng." },
  { "en": "Celengan?", "id": "Tabungan." },
  { "en": "Celengkang-celengkok?", "id": "Berliku-liku." },
  { "en": "Celentang?", "id": "Terlentang." },
  { "en": "Celepak?", "id": "Bunyi tamparan." },
  { "en": "Celepuk?", "id": "Burung hantu." },
  { "en": "Celer?", "id": "Kilau." },
  { "en": "Celeri?", "id": "Seledri." },
  { "en": "Celes?", "id": "Tusuk." },
  { "en": "Celetuk?", "id": "Ucapan tiba-tiba." },
  { "en": "Celi?", "id": "Awas." },
  { "en": "Celinguk?", "id": "Menoleh." },
  { "en": "Celis?", "id": "Gores." },
  { "en": "Celok?", "id": "Lekuk (arkais)." },
  { "en": "Celomes?", "id": "Cerewet." },
  { "en": "Celomok?", "id": "Kotor (wajah)." },
  { "en": "Celonok?", "id": "Menonjol (mata)." },
  { "en": "Celopar?", "id": "Banyak mulut." },
  { "en": "Celos?", "id": "Lepas." },
  { "en": "Celoteh?", "id": "Omong kosong." },
  { "en": "Celuk?", "id": "Masukkan tangan." },
  { "en": "Celum?", "id": "Masuk (air)." },
  { "en": "Celung?", "id": "Dalam (cekung)." },
  { "en": "Celup?", "id": "Benam." },
  { "en": "Celur?", "id": "Rebus sebentar." },
  { "en": "Celus?", "id": "Masuk." },
  { "en": "Celutak?", "id": "Suka mencuri." },
  { "en": "Cema?", "id": "Tuduhan." },
  { "en": "Cemani?", "id": "Hitam legam." },
  { "en": "Cemar?", "id": "Kotor." },
  { "en": "Cemara?", "id": "Pohon." },
  { "en": "Cemas?", "id": "Khawatir." },
  { "en": "Cemat?", "id": "Cambuk (arkais)." },
  { "en": "Cembeng?", "id": "Muka masam." },
  { "en": "Cemberut?", "id": "Muram." },
  { "en": "Cembul?", "id": "Wadah kecil." },
  { "en": "Cembung?", "id": "Menonjol keluar." },
  { "en": "Cemburu?", "id": "Iri hati." },
  { "en": "Cemeeh?", "id": "Ejek." },
  { "en": "Cemek?", "id": "Picing (mata)." },
  { "en": "Cemer?", "id": "Buta sebelah (arkais)." },
  { "en": "Cemerlang?", "id": "Gemilang." },
  { "en": "Cemeti?", "id": "Cambuk." },
  { "en": "Cemomot?", "id": "Kotor (muka)." },
  { "en": "Cempaka?", "id": "Bunga." },
  { "en": "Cempal?", "id": "Pukul (arkais)." },
  { "en": "Cempala?", "id": "Nakal." },
  { "en": "Cempe?", "id": "Anak kambing." },
  { "en": "Cempedak?", "id": "Buah." },
  { "en": "Cempek?", "id": "Pincang (arkais)." },
  { "en": "Cempelai?", "id": "Musang." },
  { "en": "Cempelik?", "id": "Ambil sedikit." },
  { "en": "Cemplang?", "id": "Hambar." },
  { "en": "Cemplung?", "id": "Masuk (ke air)." },
  { "en": "Cempo?", "id": "Kain (arkais)." },
  { "en": "Cempurit?", "id": "Gagang wayang." },
  { "en": "Cemuk?", "id": "Cium (arkais)." },
  { "en": "Cemuru?", "id": "Pohon." },
  { "en": "Cenak?", "id": "Enak (arkais)." },
  { "en": "Cenanga?", "id": "Kenanga." },
  { "en": "Cenangga?", "id": "Cacat tubuh." },
  { "en": "Cencala?", "id": "Burung." },
  { "en": "Cencaluk?", "id": "Makanan fermentasi udang." },
  { "en": "Cencang?", "id": "Cincang." },
  { "en": "Cencaru?", "id": "Ikan." },
  { "en": "Cenda?", "id": "Lelap (tidur)." },
  { "en": "Cendala?", "id": "Nista." },
  { "en": "Cendana?", "id": "Kayu wangi." },
  { "en": "Cendang?", "id": "Juling." },
  { "en": "Cendawan?", "id": "Jamur." },
  { "en": "Cendekia?", "id": "Cerdas." },
  { "en": "Cendera?", "id": "Bulan (sastra)." },
  { "en": "Cenderai?", "id": "Nyanyi (arkais)." },
  { "en": "Cenderasa?", "id": "Kenangan." },
  { "en": "Cenderung?", "id": "Condong." },
  { "en": "Cendok?", "id": "Sendok." },
  { "en": "Cenduai?", "id": "Guna-guna." },
  { "en": "Cenela?", "id": "Sandal." },
  { "en": "Ceneng?", "id": "Pusing (arkais)." },
  { "en": "Ceng?", "id": "Bau (khas)." },
  { "en": "Cengal?", "id": "Pohon." },
  { "en": "Cengam?", "id": "Gigit." },
  { "en": "Cengang?", "id": "Heran." },
  { "en": "Cengar-cengir?", "id": "Tersenyum-senyum." },
  { "en": "Cengbeng?", "id": "Ziarah kubur (Tionghoa)." },
  { "en": "Cengcong?", "id": "Banyak cakap." },
  { "en": "Cengeh?", "id": "Genit (arkais)." },
  { "en": "Cengek?", "id": "Cabai rawit." },
  { "en": "Cengeng?", "id": "Mudah menangis." },
  { "en": "Cengir?", "id": "Senyum." },
  { "en": "Cengis?", "id": "Bau menyengat." },
  { "en": "Cengkal?", "id": "Ukuran panjang." },
  { "en": "Cengkam?", "id": "Genggam." },
  { "en": "Cengkar?", "id": "Gersang." },
  { "en": "Cengkau?", "id": "Perantara." },
  { "en": "Cengkedi?", "id": "Sial." },
  { "en": "Cengkeh?", "id": "Rempah." },
  { "en": "Cengki?", "id": "Untung (arkais)." },
  { "en": "Cengkiak?", "id": "Bertengkar." },
  { "en": "Cengkih?", "id": "Cengkeh." },
  { "en": "Cengking?", "id": "Melengking." },
  { "en": "Cengkir?", "id": "Kelapa muda." },
  { "en": "Cengkok?", "id": "Liku lagu." },
  { "en": "Cengkol?", "id": "Lekuk (arkais)." },
  { "en": "Cengkong?", "id": "Bengkok (tangan)." },
  { "en": "Cengkung?", "id": "Cekung (pipi)." },
  { "en": "Cengkurai?", "id": "Kain sutra." },
  { "en": "Cenong?", "id": "Dahi menonjol." },
  { "en": "Centang?", "id": "Tanda periksa." },
  { "en": "Centangan?", "id": "Gambar (arkais)." },
  { "en": "Centeng?", "id": "Penjaga." },
  { "en": "Centet?", "id": "Kerdil." },
  { "en": "Centil?", "id": "Genit." },
  { "en": "Centong?", "id": "Sendok nasi." },
  { "en": "Centung?", "id": "Jambul." },
  { "en": "Cepak?", "id": "Potongan rambut pendek." },
  { "en": "Cepaka?", "id": "Cempaka." },
  { "en": "Cepat?", "id": "Lekas." },
  { "en": "Cepeng?", "id": "Uang (arkais)." },
  { "en": "Ceper?", "id": "Datar." },
  { "en": "Cepiau?", "id": "Topi." },
  { "en": "Cepit?", "id": "Jepit." },
  { "en": "Ceplek?", "id": "Patah (arkais)." },
  { "en": "Cepoh?", "id": "Lumpuh sebagian." },
  { "en": "Cepol?", "id": "Sanggul." },
  { "en": "Cepu?", "id": "Wadah bedak." },
  { "en": "Cerabah?", "id": "Tidak senonoh." },
  { "en": "Cerah?", "id": "Terang." },
  { "en": "Cerai?", "id": "Pisah." },
  { "en": "Cerakin?", "id": "Obat pencahar." },
  { "en": "Ceramah?", "id": "Pidato." },
  { "en": "Cerana?", "id": "Tempat sirih." },
  { "en": "Cerang?", "id": "Gudang (arkais)." },
  { "en": "Cerat?", "id": "Corong." },
  { "en": "Ceratai?", "id": "Obrolan." },
  { "en": "Cerau?", "id": "Bunyi gemerisik." },
  { "en": "Cerawat?", "id": "Roket." },
  { "en": "Cercah?", "id": "Sedikit." },
  { "en": "Cercap?", "id": "Cepat tanggap." },
  { "en": "Cerceh?", "id": "Celopar." },
  { "en": "Cercel?", "id": "Cecer." },
  { "en": "Cerdik?", "id": "Pandai." },
  { "en": "Cere?", "id": "Kain (arkais)." },
  { "en": "Cerecek?", "id": "Burung." },
  { "en": "Cereh?", "id": "Cerewet." },
  { "en": "Cerek?", "id": "Ketel." },
  { "en": "Ceremai?", "id": "Buah." },
  { "en": "Cerempung?", "id": "Alat musik." },
  { "en": "Cerewet?", "id": "Banyak bicara." },
  { "en": "Cergas?", "id": "Tangkas." },
  { "en": "Ceri?", "id": "Buah." },
  { "en": "Ceria?", "id": "Riang." },
  { "en": "Cerih?", "id": "Kotoran (arkais)." },
  { "en": "Cerita?", "id": "Kisah." },
  { "en": "Cerkas?", "id": "Kertas (arkais)." },
  { "en": "Cerkau?", "id": "Terjang." },
  { "en": "Cerlang?", "id": "Cemerlang." },
  { "en": "Cerlih?", "id": "Licin." },
  { "en": "Cermat?", "id": "Teliti." },
  { "en": "Cermin?", "id": "Kaca." },
  { "en": "Cerna?", "id": "Hancurkan (makanan)." },
  { "en": "Ceroboh?", "id": "Lalai." },
  { "en": "Cerobong?", "id": "Saluran asap." },
  { "en": "Cerocok?", "id": "Pancang." },
  { "en": "Cerompong?", "id": "Berlubang." },
  { "en": "Cerpen?", "id": "Cerita pendek." },
  { "en": "Cerpu?", "id": "Sandal kayu." },
  { "en": "Cerucuh?", "id": "Paruh bengkok." },
  { "en": "Cerucup?", "id": "Runcing." },
  { "en": "Ceruh?", "id": "Warna merah." },
  { "en": "Ceruk?", "id": "Lekuk." },
  { "en": "Cerun?", "id": "Lereng curam." },
  { "en": "Cerutu?", "id": "Rokok besar." },
  { "en": "Ces?", "id": "Bunyi mendesis." },
  { "en": "Cetai?", "id": "Robek-robek." },
  { "en": "Cetak?", "id": "Cetak." },
  { "en": "Cetar?", "id": "Bunyi keras." },
  { "en": "Cetek?", "id": "Dangkal." },
  { "en": "Ceter?", "id": "Perisai (arkais)." },
  { "en": "Ceteri?", "id": "Payung kebesaran." },
  { "en": "Cetus?", "id": "Ucapan." },
  { "en": "Ciak?", "id": "Bunyi anak ayam." },
  { "en": "Cialat?", "id": "Sial." },
  { "en": "Ciang?", "id": "Panggilan (Tionghoa)." },
  { "en": "Cibir?", "id": "Ejek." },
  { "en": "Cibit?", "id": "Ambil sedikit (arkais)." },
  { "en": "Cicik?", "id": "Jijik." },
  { "en": "Cicil?", "id": "Angsur." },
  { "en": "Cicip?", "id": "Rasai." },
  { "en": "Cidomo?", "id": "Kereta kuda (Lombok)." },
  { "en": "Ciduk?", "id": "Gayung." },
  { "en": "Cih?", "id": "Seruan jijik." },
  { "en": "Cik?", "id": "Panggilan wanita muda." },
  { "en": "Cika?", "id": "Sakit perut." },
  { "en": "Cikal?", "id": "Tunas kelapa." },
  { "en": "Cikar?", "id": "Gerobak." },
  { "en": "Cikrak?", "id": "Pengki." },
  { "en": "Ciku?", "id": "Sawo." },
  { "en": "Cikut?", "id": "Ambil (arkais)." },
  { "en": "Cilap?", "id": "Kilau." },
  { "en": "Cilat?", "id": "Jilat." },
  { "en": "Cili?", "id": "Cabai (arkais)." },
  { "en": "Cilik?", "id": "Kecil." },
  { "en": "Ciling?", "id": "Celeng (arkais)." },
  { "en": "Cilok?", "id": "Makanan." },
  { "en": "Cilukba?", "id": "Permainan anak." },
  { "en": "Cima?", "id": "Kain (arkais)." },
  { "en": "Cimeng?", "id": "Ganja (slang)." },
  { "en": "Cimplong?", "id": "Kue." },
  { "en": "Cimput?", "id": "Kecil (arkais)." },
  { "en": "Cina?", "id": "Tiongkok (sebutan lama)." },
  { "en": "Cincai?", "id": "Mudah (bahasa pasar)." },
  { "en": "Cincang?", "id": "Potong halus." },
  { "en": "Cincau?", "id": "Minuman." },
  { "en": "Cincin?", "id": "Perhiasan jari." },
  { "en": "Cinda?", "id": "Mata (arkais)." },
  { "en": "Cindai?", "id": "Kain sutra." },
  { "en": "Cindaku?", "id": "Harimau jadi-jadian." },
  { "en": "Cinde?", "id": "Kain." },
  { "en": "Cing?", "id": "Bunyi logam." },
  { "en": "Cingam?", "id": "Kotoran mata." },
  { "en": "Cingangah?", "id": "Ternganga." },
  { "en": "Cinging?", "id": "Melengking." },
  { "en": "Cingkeh?", "id": "Rokok cengkih." },
  { "en": "Cingkrang?", "id": "Terlalu pendek (celana)." },
  { "en": "Cinta?", "id": "Kasih." },
  { "en": "Cintamani?", "id": "Permata sakti." },
  { "en": "Cinteng?", "id": "Sentil." },
  { "en": "Cintrong?", "id": "Suka (slang)." },
  { "en": "Cip?", "id": "Anak ayam (bunyi)." },
  { "en": "Cipai?", "id": "Permainan kartu." },
  { "en": "Cipir?", "id": "Kecipir." },
  { "en": "Ciprat?", "id": "Percik." },
  { "en": "Cipta?", "id": "Buat." },
  { "en": "Cir?", "id": "Bunyi." },
  { "en": "Ciri?", "id": "Tanda." },
  { "en": "Cirit?", "id": "Kotoran cair." },
  { "en": "Cirpuk?", "id": "Sandal." },
  { "en": "Cit?", "id": "Decit." },
  { "en": "Cita?", "id": "Keinginan." },
  { "en": "Citra?", "id": "Gambaran." },
  { "en": "Ciut?", "id": "Sempit." },
  { "en": "Cium?", "id": "Kecup." },
  { "en": "Coba?", "id": "Uji." },
  { "en": "Coban?", "id": "Air terjun (Jawa)." },
  { "en": "Cobar-cabir?", "id": "Robek-robek." },
  { "en": "Cobek?", "id": "Alat ulekan." },
  { "en": "Cocok?", "id": "Sesuai." },
  { "en": "Cocol?", "id": "Celup." },
  { "en": "Codet?", "id": "Bekas luka." },
  { "en": "Cogah?", "id": "Mewah (arkais)." },
  { "en": "Cogan?", "id": "Panji-panji." },
  { "en": "Cok?", "id": "Tidak (Minang)." },
  { "en": "Cokek?", "id": "Tarian." },
  { "en": "Cokelat?", "id": "Warna." },
  { "en": "Coko?", "id": "Duduk (Jawa)." },
  { "en": "Cokok?", "id": "Tangkap (hewan)." },
  { "en": "Cokol?", "id": "Pijak." },
  { "en": "Colak-caling?", "id": "Tidak karuan." },
  { "en": "Colek?", "id": "Sentuh." },
  { "en": "Coleng?", "id": "Curi (arkais)." },
  { "en": "Colok?", "id": "Sumbu api." },
  { "en": "Comberan?", "id": "Selokan." },
  { "en": "Comblang?", "id": "Perantara jodoh." },
  { "en": "Comek?", "id": "Kecil (arkais)." },
  { "en": "Comel?", "id": "Menggerutu." },
  { "en": "Common?", "id": "Biasa (Inggris)." },
  { "en": "Comot?", "id": "Ambil." },
  { "en": "Compang-camping?", "id": "Koyak-moyak." },
  { "en": "Condong?", "id": "Miring." },
  { "en": "Congak?", "id": "Hitung di luar kepala." },
  { "en": "Congeh?", "id": "Menonjol (gigi)." },
  { "en": "Congkak?", "id": "Sombong." },
  { "en": "Congkel?", "id": "Cungkil." },
  { "en": "Congklak?", "id": "Dakon." },
  { "en": "Congok?", "id": "Julur (arkais)." },
  { "en": "Contoh?", "id": "Teladan." },
  { "en": "Contong?", "id": "Wadah kerucut." },
  { "en": "Copet?", "id": "Pencuri." },
  { "en": "Copot?", "id": "Lepas." },
  { "en": "Cor?", "id": "Tuang (logam)." },
  { "en": "Corak?", "id": "Pola." },
  { "en": "Corek?", "id": "Gores." },
  { "en": "Coret?", "id": "Garis." },
  { "en": "Coro?", "id": "Kecoa." },
  { "en": "Corob?", "id": "Ceroboh (arkais)." },
  { "en": "Corong?", "id": "Alat tuang." },
  { "en": "Cota?", "id": "Gada." },
  { "en": "Cotet?", "id": "Sedikit (arkais)." },
  { "en": "Cowok?", "id": "Laki-laki (prokem)." },
  { "en": "Cuaca?", "id": "Keadaan udara." },
  { "en": "Cuai?", "id": "Remeh." },
  { "en": "Cuak?", "id": "Takut (arkais)." },
  { "en": "Cuan?", "id": "Untung (Tionghoa)." },
  { "en": "Cuang?", "id": "Tiang layar." },
  { "en": "Cubit?", "id": "Jepit." },
  { "en": "Cucakrawa?", "id": "Burung." },
  { "en": "Cuci?", "id": "Basuh." },
  { "en": "Cucu?", "id": "Keturunan." },
  { "en": "Cucuh?", "id": "Sumbu." },
  { "en": "Cucuk?", "id": "Paruh." },
  { "en": "Cucun?", "id": "Cucu (arkais)." },
  { "en": "Cucung?", "id": "Anak (arkais)." },
  { "en": "Cucurut?", "id": "Tikus." },
  { "en": "Cucut?", "id": "Ikan hiu." },
  { "en": "Cuek?", "id": "Tidak peduli (prokem)." },
  { "en": "Cuh?", "id": "Seruan." },
  { "en": "Cuit?", "id": "Siul." },
  { "en": "Cuka?", "id": "Cairan asam." },
  { "en": "Cukai?", "id": "Pajak barang." },
  { "en": "Cuki?", "id": "Permainan." },
  { "en": "Cukil?", "id": "Ambil sedikit." },
  { "en": "Cukin?", "id": "Kain." },
  { "en": "Cukong?", "id": "Pemodal besar." },
  { "en": "Cukup?", "id": "Memadai." },
  { "en": "Cukur?", "id": "Pangkas rambut." },
  { "en": "Cula?", "id": "Tanduk badak." },
  { "en": "Culas?", "id": "Curang." },
  { "en": "Culiah?", "id": "Panggil (arkais)." },
  { "en": "Culika?", "id": "Licik (arkais)." },
  { "en": "Cum?", "id": "Dengan (Latin)." },
  { "en": "Cuma?", "id": "Hanya." },
  { "en": "Cumbu?", "id": "Rayu." },
  { "en": "Cumengkling?", "id": "Nyaring." },
  { "en": "Cumepak?", "id": "Siap (Jawa)." },
  { "en": "Cumik?", "id": "Kecil (arkais)." },
  { "en": "Cumi-cumi?", "id": "Hewan laut." },
  { "en": "Cumil?", "id": "Kecil (arkais)." },
  { "en": "Cunam?", "id": "Tang." },
  { "en": "Cundang?", "id": "Hasutan (arkais)." },
  { "en": "Cundrik?", "id": "Keris kecil." },
  { "en": "Cunduk?", "id": "Tusuk konde." },
  { "en": "Cung?", "id": "Panggilan (anak)." },
  { "en": "Cungap?", "id": "Tersengal." },
  { "en": "Cungkil?", "id": "Ambil paksa." },
  { "en": "Cungkup?", "id": "Bangunan makam." },
  { "en": "Cungo?", "id": "Banci (arkais)." },
  { "en": "Cungur?", "id": "Moncong." },
  { "en": "Cup?", "id": "Gelas (Inggris)." },
  { "en": "Cupai?", "id": "Lalai." },
  { "en": "Cupak?", "id": "Takaran." },
  { "en": "Cupang?", "id": "Ikan adu." },
  { "en": "Cupar?", "id": "Banyak mulut." },
  { "en": "Cupet?", "id": "Sempit (pikiran)." },
  { "en": "Cuping?", "id": "Daun telinga." },
  { "en": "Cupit?", "id": "Sempit (arkais)." },
  { "en": "Cuplik?", "id": "Ambil sebagian." },
  { "en": "Cupu?", "id": "Wadah kecil." },
  { "en": "Curah?", "id": "Tuang." },
  { "en": "Curam?", "id": "Terjal." },
  { "en": "Curang?", "id": "Tidak jujur." },
  { "en": "Curat?", "id": "Pancuran." },
  { "en": "Cureng?", "id": "Tajam (pandangan)." },
  { "en": "Curi?", "id": "Ambil tanpa izin." },
  { "en": "Curiah?", "id": "Cerewet (arkais)." },
  { "en": "Curiga?", "id": "Sangka." },
  { "en": "Cus?", "id": "Seruan berangkat." },
  { "en": "Cut?", "id": "Panggilan wanita (Aceh)." },
  { "en": "Cuti?", "id": "Libur kerja." },
  { "en": "Dab?", "id": "Sentuhan ringan." },
  { "en": "Daba?", "id": "Dahak (arkais)." },
  { "en": "Dabih?", "id": "Sembelih (arkais)." },
  { "en": "Dablek?", "id": "Keras kepala (Jawa)." },
  { "en": "Dabung?", "id": "Gigi palsu." },
  { "en": "Dabus?", "id": "Kesenian." },
  { "en": "Dacin?", "id": "Timbangan." },
  { "en": "Dada?", "id": "Bagian tubuh." },
  { "en": "Dadah?", "id": "Narkoba." },
  { "en": "Dadak?", "id": "Tiba-tiba." },
  { "en": "Dadap?", "id": "Pohon." },
  { "en": "Dadar?", "id": "Telur goreng." },
  { "en": "Dadi?", "id": "Jadi (Jawa)." },
  { "en": "Dadih?", "id": "Susu fermentasi." },
  { "en": "Dading?", "id": "Pedang (arkais)." },
  { "en": "Dadu?", "id": "Alat judi." },
  { "en": "Daeng?", "id": "Panggilan (Sulawesi)." },
  { "en": "Daerah?", "id": "Wilayah." },
  { "en": "Dafinah?", "id": "Harta terpendam (Arab)." },
  { "en": "Daftar?", "id": "Senarai." },
  { "en": "Daga?", "id": "Haus (arkais)." },
  { "en": "Dagang?", "id": "Jual beli." },
  { "en": "Dage?", "id": "Makanan (ampas tahu)." },
  { "en": "Dagel?", "id": "Lawak." },
  { "en": "Dagi?", "id": "Daki (arkais)." },
  { "en": "Daging?", "id": "Otot hewan." },
  { "en": "Dagu?", "id": "Bagian wajah." },
  { "en": "Dah?", "id": "Sudah (singkatan)." },
  { "en": "Dahaga?", "id": "Haus." },
  { "en": "Dahak?", "id": "Lendir." },
  { "en": "Daham?", "id": "Deham." },
  { "en": "Dahan?", "id": "Cabang pohon." },
  { "en": "Dahi?", "id": "Kening." },
  { "en": "Dahlia?", "id": "Bunga." },
  { "en": "Dahriah?", "id": "Ateis (Arab)." },
  { "en": "Dahsyat?", "id": "Hebat." },
  { "en": "Dahu?", "id": "Pohon (arkais)." },
  { "en": "Dahulu?", "id": "Dulu." },
  { "en": "Dai?", "id": "Pendakwah." },
  { "en": "Daidan?", "id": "Pasukan (Jepang)." },
  { "en": "Daim?", "id": "Kekal (Arab)." },
  { "en": "Daitia?", "id": "Raksasa (Sanskerta)." },
  { "en": "Dajal?", "id": "Penipu besar." },
  { "en": "Daka?", "id": "Pelayan (arkais)." },
  { "en": "Dakap?", "id": "Peluk." },
  { "en": "Dakar?", "id": "Penis (kasar)." },
  { "en": "Daki?", "id": "Kotoran kulit." },
  { "en": "Dakik?", "id": "Halus (Arab)." },
  { "en": "Dakocan?", "id": "Boneka." },
  { "en": "Dakon?", "id": "Congklak." },
  { "en": "Daksina?", "id": "Selatan (Sanskerta)." },
  { "en": "Dakwaan?", "id": "Tuduhan." }


        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 5000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
