<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
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
        <h1>Pengetahuan Umum Dasar</h1>
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


  { "en": "Apa Itu EVPN (Ethernet VPN)?", "id": "Solusi VPN (Virtual Private Network) Lapis 2 Modern." },
  { "en": "Apa Kepanjangan Dari EVPN (Ethernet VPN)?", "id": "Ethernet VPN." },
  { "en": "Control Plane Apa Digunakan EVPN (Ethernet VPN)?", "id": "MP-BGP (Multiprotocol BGP)." },
  { "en": "Data Plane Apa Digunakan EVPN (Ethernet VPN)?", "id": "VXLAN (Virtual Extensible LAN) MPLS." },
  { "en": "Apa Itu OTV (Overlay Transport Virtualization)?", "id": "Teknologi Interkoneksi Data Center (L2)." },
  { "en": "Apa Kepanjangan Dari OTV (Overlay Transport Virtualization)?", "id": "Overlay Transport Virtualization." },
  { "en": "Apa Itu Cisco SD-Access (Software-Defined Access)?", "id": "Solusi Jaringan Kampus SDN (Software-Defined Networking)." },
  { "en": "Apa Itu Cisco ACI (Application Centric Infrastructure)?", "id": "Solusi Data Center SDN (Software-Defined Networking)." },
  { "en": "Apa Itu Endpoint Group (EPG)?", "id": "Grup Endpoint (Kebijakan ACI)." },
  { "en": "Apa Itu Contract (ACI)?", "id": "Kebijakan Keamanan Antar EPG (Endpoint Group)." },
  { "en": "Apa Itu Top-of-Rack (ToR) Switch?", "id": "Switch Di Atas Rak (Leaf)." },
  { "en": "Topologi Apa Optimal (East-West)?", "id": "Spine-Leaf." },
  { "en": "Apa Itu Virtual Port Channel (vPC)?", "id": "Port Channel (Antar Dua Switch Nexus)." },
  { "en": "Apa Itu Cisco Catalyst?", "id": "Seri Switch Cisco Enterprise Kampus." },
  { "en": "Apa Itu FabricPath?", "id": "Teknologi Lapis 2 (Pengganti STP, Cisco)." },
  { "en": "Apa Itu TRILL (Transparent Interconnection of Lots of Links)?", "id": "Standar IETF (Internet Engineering Task Force) (Mirip FabricPath)." },
  { "en": "Apa Kepanjangan Dari TRILL (Transparent Interconnection of Lots of Links)?", "id": "Transparent Interconnection of Lots of Links." },
  { "en": "Apa Itu Shortest Path Bridging (SPB)?", "id": "Standar IEEE (Institute of Electrical Electronics Engineers) (802.1aq)." },
  { "en": "Apa Kepanjangan Dari SPB (Shortest Path Bridging)?", "id": "Shortest Path Bridging." },
  { "en": "Apa Itu Virtual Extensible LAN (VXLAN)?", "id": "Protokol Overlay Lapis 2." },
  { "en": "Apa Itu Ethernet VPN (EVPN)?", "id": "Control Plane (MP-BGP) Untuk VXLAN." },
  { "en": "Apa Itu Multicast?", "id": "Transmisi Satu Ke Banyak (Grup)." },
  { "en": "Apa Itu Alamat IP (Internet Protocol) Multicast?", "id": "Alamat Kelas D (224.0.0.0 /4)." },
  { "en": "Apa Itu Internet Group Management Protocol (IGMP)?", "id": "Protokol Klien (Bergabung Grup Multicast)." },
  { "en": "Apa Itu IGMP (Internet Group Management Protocol) Snooping?", "id": "Fitur Switch (Memfilter Multicast L2)." },
  { "en": "Apa Itu Protocol Independent Multicast (PIM)?", "id": "Protokol Routing Multicast (Lapis 3)." },
  { "en": "Apa Itu PIM (Protocol Independent Multicast) Sparse Mode?", "id": "Mode PIM (Join Eksplisit, Rendezvous Point)." },
  { "en": "Apa Itu Rendezvous Point (RP)?", "id": "Titik Pertemuan (PIM Sparse Mode)." },
  { "en": "Apa Kepanjangan Dari RP (Rendezvous Point)?", "id": "Rendezvous Point." },
  { "en": "Apa Itu Anycast RP (Rendezvous Point)?", "id": "Redundansi RP (Rendezvous Point) (PIM Sparse Mode)." },
  { "en": "Apa Itu Multicast Source Discovery Protocol (MSDP)?", "id": "Protokol (Berbagi Sumber Antar Domain)." },
  { "en": "Apa Itu Reverse Path Forwarding (RPF)?", "id": "Pengecekan Jalur Balik (Mencegah Loop Multicast)." },
  { "en": "Apa Kepanjangan Dari RPF (Reverse Path Forwarding)?", "id": "Reverse Path Forwarding." },
  { "en": "Apa Itu IP (Internet Protocol) Television (IPTV)?", "id": "Layanan TV Melalui Jaringan IP." },
  { "en": "Protokol Apa Digunakan IPTV (IP Television)?", "id": "Multicast (Untuk Siaran Langsung)." },
  { "en": "Apa Itu Video on Demand (VOD)?", "id": "Layanan Video (Streaming Unicast)." },
  { "en": "Apa Itu Set-Top Box (STB)?", "id": "Perangkat Penerima Sinyal IPTV." },
  { "en": "Apa Itu Differentiated Services (DiffServ)?", "id": "Model QoS (Quality of Service) (Berbasis Kelas)." },
  { "en": "Apa Itu DSCP (Differentiated Services Code Point)?", "id": "Nilai Penanda Prioritas QoS (Lapis 3)." },
  { "en": "Apa Itu Antrian (Queuing)?", "id": "Manajemen Paket Saat Jaringan Padat." },
  { "en": "Apa Itu Low Latency Queuing (LLQ)?", "id": "Antrian Prioritas (Untuk Suara Video)." },
  { "en": "Apa Itu Weighted Fair Queuing (WFQ)?", "id": "Antrian Adil (Berbasis Bobot Aliran)." },
  { "en": "Apa Itu Class-Based WFQ (CBWFQ)?", "id": "WFQ (Weighted Fair Queuing) Berbasis Kelas." },
  { "en": "Apa Itu Congestion Avoidance?", "id": "Mekanisme Menghindari Kepadatan." },
  { "en": "Apa Itu Network Address Translation (NAT)?", "id": "Translasi Alamat IP (Internet Protocol) Privat." },
  { "en": "Apa Itu Port Address Translation (PAT)?", "id": "NAT (Network Address Translation) Overload (Port)." },
  { "en": "Apa Itu Static NAT (Network Address Translation)?", "id": "Pemetaan IP (Internet Protocol) Satu-Banding-Satu." },
  { "en": "Apa Itu Dynamic NAT (Network Address Translation)?", "id": "Pemetaan IP (Internet Protocol) (Pool Alamat)." },
  { "en": "Apa Itu NAT64 (Network Address Translation 64)?", "id": "Translasi Alamat (IPv6 Ke IPv4)." },
  { "en": "Apa Itu DNS64 (Domain Name System 64)?", "id": "DNS (Domain Name System) Sintesis (IPv6 Ke IPv4)." },
  { "en": "Apa Itu Standard ACL (Access Control List)?", "id": "ACL (Access Control List) (Filter IP Sumber)." },
  { "en": "Apa Itu Extended ACL (Access Control List)?", "id": "ACL (Filter IP, Port, Protokol)." },
  { "en": "Apa Itu Extended ACL (Access Control List)?", "id": "ACL (Access Control List) (Filter IP, Port, Protokol)." },
  { "en": "Apa Itu Named ACL (Access Control List)?", "id": "ACL (Access Control List) Menggunakan Nama." },
  { "en": "Apa Itu Wildcard Mask?", "id": "Mask (Mencocokkan Bit Alamat IP)." },
  { "en": "Apa Wildcard Mask Untuk Semua Alamat?", "id": "255.255.255.255." },
  { "en": "Apa Itu Firewall Stateless?", "id": "Firewall Memeriksa Paket Individual." },
  { "en": "Apa Itu Intrusion Detection System (IDS)?", "id": "Sistem Pendeteksi Serangan (Peringatan)." },
  { "en": "Apa Beda IDS (Intrusion Detection System) Dan IPS (Intrusion Prevention System)?", "id": "IDS (Intrusion Detection System) Pasif, IPS Aktif." },
  { "en": "Apa Itu Heuristic-Based Detection?", "id": "Deteksi Berbasis Aturan Heuristik." },
  { "en": "Apa Itu Posture Assessment (NAC)?", "id": "Pemeriksaan Kepatuhan Keamanan Perangkat." },
  { "en": "Apa Komponen Utama 802.1X?", "id": "Supplicant, Authenticator, Authentication Server." },
  { "en": "Apa Itu Authentication Server (802.1X)?", "id": "Server Autentikasi (Contoh: RADIUS)." },
  { "en": "Apa Itu EAP-TLS (EAP-Transport Layer Security)?", "id": "Metode EAP (Extensible Authentication Protocol) (Sertifikat)." },
  { "en": "Apa Itu PEAP (Protected EAP)?", "id": "Metode EAP (Extensible Authentication Protocol) (Tunneling)." },
  { "en": "Apa Itu Site-to-Site VPN (Virtual Private Network)?", "id": "VPN (Virtual Private Network) Antar Jaringan." },
  { "en": "Apa Itu Remote Access VPN (Virtual Private Network)?", "id": "VPN (Virtual Private Network) Pengguna Individu." },
  { "en": "Apa Itu IPsec (Internet Protocol Security)?", "id": "Protokol Keamanan VPN (Lapis 3)." },
  { "en": "Apa Itu Authentication Header (AH)?", "id": "Bagian IPsec (Internet Protocol Security) (Autentikasi)." },
  { "en": "Apa Itu Encapsulating Security Payload (ESP)?", "id": "Bagian IPsec (Internet Protocol Security) (Enkripsi)." },
  { "en": "Apa Itu Tunnel Mode (IPsec)?", "id": "Mode (Enkripsi Seluruh Paket IP)." },
  { "en": "Apa Itu Transport Mode (IPsec)?", "id": "Mode (Enkripsi Payload Saja)." },
  { "en": "Apa Itu Security Association (SA)?", "id": "Kesepakatan Parameter Keamanan IPsec." },
  { "en": "Apa Itu SSL/TLS (Secure Sockets Layer/Transport Layer Security) VPN?", "id": "VPN (Virtual Private Network) (Port 443)." },
  { "en": "Apa Itu Enkripsi Simetris?", "id": "Enkripsi Menggunakan Satu Kunci." },
  { "en": "Apa Itu Enkripsi Asimetris?", "id": "Enkripsi Menggunakan Dua Kunci." },
  { "en": "Apa Itu Kunci Publik (Public Key)?", "id": "Kunci (Untuk Enkripsi, Dibagikan)." },
  { "en": "Apa Itu Kunci Privat (Private Key)?", "id": "Kunci (Untuk Dekripsi, Rahasia)." },
  { "en": "Apa Itu Hashing?", "id": "Fungsi Satu Arah (Integritas Data)." },
  { "en": "Apa Itu Message Digest 5 (MD5)?", "id": "Algoritma Hash (Lama, Tidak Aman)." },
  { "en": "Apa Itu Secure Hash Algorithm (SHA)?", "id": "Algoritma Hash (Lebih Aman)." },
  { "en": "Apa Itu SHA-1 (Secure Hash Algorithm 1)?", "id": "Hash (160-bit, Dianggap Lemah)." },
  { "en": "Apa Itu SHA-256 (Secure Hash Algorithm 256)?", "id": "Hash (256-bit, Aman)." },
  { "en": "Bagaimana Tanda Tangan Digital Bekerja?", "id": "Hash Dienkripsi Kunci Privat." },
  { "en": "Apa Itu Public Key Infrastructure (PKI)?", "id": "Infrastruktur Pengelola Kunci Sertifikat." },
  { "en": "Apa Itu Certificate Revocation List (CRL)?", "id": "Daftar Sertifikat Yang Dicabut." },
  { "en": "Apa Itu Online Certificate Status Protocol (OCSP)?", "id": "Pengecekan Status Sertifikat (Real-time)." },
  { "en": "Apa Itu Pretty Good Privacy (PGP)?", "id": "Program Enkripsi Email (Web of Trust)." },
  { "en": "Apa Kepanjangan Dari PGP (Pretty Good Privacy)?", "id": "Pretty Good Privacy." },
  { "en": "Apa Itu GPG (GNU Privacy Guard)?", "id": "Implementasi Open-Source PGP (Pretty Good Privacy)." },
  { "en": "Apa Kepanjangan Dari GPG (GNU Privacy Guard)?", "id": "GNU Privacy Guard." },
  { "en": "Apa Itu Key Escrow?", "id": "Penyimpanan Kunci Privat (Pihak Ketiga)." },
  { "en": "Apa Itu Steganografi?", "id": "Seni Menyembunyikan Pesan (Dalam Media)." },
  { "en": "Apa Beda Kriptografi Dan Steganografi?", "id": "Kriptografi Mengacak, Steganografi Menyembunyikan." },
  { "en": "Apa Itu Order of Volatility?", "id": "Urutan Pengumpulan Data (Paling Volatil)." },
  { "en": "Apa Itu Data Volatil?", "id": "Data Hilang (Listrik Mati, RAM)." },
  { "en": "Apa Itu Data Non-Volatil?", "id": "Data Tersimpan (Listrik Mati, Disk)." },
  { "en": "Apa Itu Slack Space?", "id": "Sisa Ruang Cluster (Data Tersembunyi)." },
  { "en": "Apa Itu Kebijakan Keamanan (Security Policy)?", "id": "Dokumen Aturan Keamanan Organisasi." },
  { "en": "Apa Itu Acceptable Use Policy (AUP)?", "id": "Kebijakan Penggunaan Aset Teknologi." },
  { "en": "Apa Itu Password Policy (Kebijakan Password)?", "id": "Aturan Pembuatan Penggunaan Password." },
  { "en": "Apa Itu Prinsip Least Privilege?", "id": "Memberikan Hak Akses Minimal (Diperlukan)." },
  { "en": "Apa Itu Separation of Duties (Pemisahan Tugas)?", "id": "Membagi Tugas Kritis (Mencegah Penipuan)." },
  { "en": "Apa Itu Job Rotation (Rotasi Pekerjaan)?", "id": "Rotasi Tugas Karyawan (Keamanan)." },
  { "en": "Apa Itu Mandatory Vacations (Cuti Wajib)?", "id": "Kebijakan Cuti Wajib (Deteksi Penipuan)." },
  { "en": "Apa Itu Onboarding (Karyawan)?", "id": "Proses Menerima Karyawan Baru (Akses)." },
  { "en": "Apa Itu Offboarding (Karyawan)?", "id": "Proses Karyawan Keluar (Mencabut Akses)." },
  { "en": "Apa Itu Data Classification (Klasifikasi Data)?", "id": "Mengelompokkan Data Berdasarkan Sensitivitas." },
  { "en": "Contoh Level Klasifikasi Data?", "id": "Publik, Internal, Rahasia, Sangat Rahasia." },
  { "en": "Apa Itu Data Owner (Pemilik Data)?", "id": "Pihak Bertanggung Jawab Atas Data." },
  { "en": "Apa Itu Data Custodian (Penjaga Data)?", "id": "Pihak Teknis Pengelola Keamanan Data." },
  { "en": "Apa Itu Data Steward (Pengurus Data)?", "id": "Pihak Pengelola Kualitas Data." },
  { "en": "Apa Itu Data Privacy (Privasi Data)?", "id": "Perlindungan Data Pribadi Individu." },
  { "en": "Apa Itu Personally Identifiable Information (PII)?", "id": "Informasi Identifikasi Pribadi (Nama, KTP)." },
  { "en": "Apa Itu Protected Health Information (PHI)?", "id": "Informasi Kesehatan Pribadi (Dilindungi)." },
  { "en": "Apa Itu General Data Protection Regulation (GDPR)?", "id": "Regulasi Privasi Data Uni Eropa." },
  { "en": "Apa Itu Payment Card Industry Data Security Standard (PCI DSS)?", "id": "Standar Keamanan Data Kartu Pembayaran." },
  { "en": "Apa Itu Health Insurance Portability and Accountability Act (HIPAA)?", "id": "Hukum Privasi Data Kesehatan (AS)." },
  { "en": "Apa Itu Data Masking (Penyamaran Data)?", "id": "Menyembunyikan Data Sensitif (Menjadi XXXXX)." },
  { "en": "Apa Itu Tokenization (Tokenisasi)?", "id": "Mengganti Data Sensitif Dengan Token." },
  { "en": "Apa Itu Data Anonymization (Anonimisasi Data)?", "id": "Menghapus Informasi Identifikasi Pribadi (PII)." },
  { "en": "Apa Itu Data Minimization (Minimisasi Data)?", "id": "Mengumpulkan Data Minimal Yang Diperlukan." },
  { "en": "Apa Itu Data Retention Policy (Kebijakan Retensi)?", "id": "Kebijakan Berapa Lama Data Disimpan." },
  { "en": "Apa Itu Data Destruction (Penghancuran Data)?", "id": "Proses Menghapus Data Secara Permanen." },
  { "en": "Apa Itu Degaussing (Demagnetisasi)?", "id": "Menghapus Data (Media Magnetik)." },
  { "en": "Apa Itu Shredding (Penghancuran Fisik)?", "id": "Menghancurkan Media Penyimpanan Secara Fisik." },
  { "en": "Apa Itu Wiping (Pembersihan Data)?", "id": "Menimpa Data (Dengan Pola Acak)." },
  { "en": "Apa Itu Cryptography (Kriptografi)?", "id": "Ilmu Pengamanan Pesan (Enkripsi)." },
  { "en": "Apa Itu Plaintext (Teks Biasa)?", "id": "Data Asli Sebelum Dienkripsi." },
  { "en": "Apa Itu Ciphertext (Teks Sandi)?", "id": "Data Yang Telah Dienkripsi." },
  { "en": "Apa Itu Cipher (Sandi)?", "id": "Algoritma Untuk Enkripsi Dekripsi." },
  { "en": "Apa Itu Key (Kunci Kriptografi)?", "id": "Informasi Rahasia (Untuk Algoritma)." },
  { "en": "Apa Itu Symmetric Encryption (Enkripsi Simetris)?", "id": "Enkripsi (Menggunakan Satu Kunci Sama)." },
  { "en": "Apa Itu Asymmetric Encryption (Enkripsi Asimetris)?", "id": "Enkripsi (Menggunakan Dua Kunci Berbeda)." },
  { "en": "Contoh Algoritma Asimetris?", "id": "RSA (Rivest–Shamir–Adleman), ECC, Diffie-Hellman." },
  { "en": "Apa Itu Public Key (Kunci Publik)?", "id": "Kunci (Untuk Enkripsi, Boleh Dibagi)." },
  { "en": "Apa Itu Private Key (Kunci Privat)?", "id": "Kunci (Untuk Dekripsi, Harus Rahasia)." },
  { "en": "Apa Itu Hybrid Encryption (Enkripsi Hibrida)?", "id": "Gabungan (Enkripsi Simetris Asimetris)." },
  { "en": "Bagaimana Hybrid Encryption Bekerja?", "id": "Asimetris (Kunci Sesi), Simetris (Data)." },
  { "en": "Apa Itu Session Key (Kunci Sesi)?", "id": "Kunci Simetris (Temporer, Satu Sesi)." },
  { "en": "Apa Itu Diffie-Hellman (DH)?", "id": "Protokol Pertukaran Kunci Aman." },
  { "en": "Apa Itu Perfect Forward Secrecy (PFS)?", "id": "Keamanan (Kunci Sesi Tetap Aman)." },
  { "en": "Apa Itu Elliptic Curve Cryptography (ECC)?", "id": "Kriptografi Asimetris (Kurva Eliptik)." },
  { "en": "Keuntungan ECC (Elliptic Curve Cryptography) Dari RSA (Rivest–Shamir–Adleman)?", "id": "Kunci Lebih Pendek (Kekuatan Sama)." },
  { "en": "Apa Itu Hash Function (Fungsi Hash)?", "id": "Fungsi Satu Arah (Membuat Hash)." },
  { "en": "Apa Itu Hash (Nilai Hash)?", "id": "Sidik Jari Digital (Ukuran Tetap)." },
  { "en": "Apa Sifat Fungsi Hash?", "id": "Satu Arah, Tahan Kolisi." },
  { "en": "Apa Itu Collision (Kolisi Hash)?", "id": "Dua Input Berbeda (Hash Sama)." },
  { "en": "Apa Itu MD5 (Message Digest 5)?", "id": "Algoritma Hash (128-bit, Tidak Aman)." },
  { "en": "Apa Itu SHA-1 (Secure Hash Algorithm 1)?", "id": "Algoritma Hash (160-bit, Lemah)." },
  { "en": "Apa Itu SHA-2 (Secure Hash Algorithm 2)?", "id": "Keluarga Hash (SHA-256, SHA-512)." },
  { "en": "Apa Itu SHA-3 (Secure Hash Algorithm 3)?", "id": "Standar Hash Terbaru (Keccak)." },
  { "en": "Apa Itu Message Authentication Code (MAC)?", "id": "Kode Autentikasi Pesan (Integritas)." },
  { "en": "Apa Kepanjangan Dari MAC (Message Authentication Code)?", "id": "Message Authentication Code." },
  { "en": "Apa Itu HMAC (Hash-based MAC)?", "id": "MAC (Message Authentication Code) (Hash Kunci)." },
  { "en": "Apa Kepanjangan Dari HMAC (Hash-based MAC)?", "id": "Hash-based Message Authentication Code." },
  { "en": "Apa Itu Digital Signature (Tanda Tangan Digital)?", "id": "Verifikasi (Autentikasi, Integritas, Non-Repudiation)." },
  { "en": "Apa Itu Non-Repudiation (Anti-Penyangkalan)?", "id": "Prinsip (Tidak Dapat Menyangkal Pengiriman)." },
  { "en": "Apa Itu Certificate Authority (CA)?", "id": "Otoritas Penerbit Sertifikat Digital." },
  { "en": "Apa Itu Registration Authority (RA)?", "id": "Otoritas Verifikasi Identitas (PKI)." },
  { "en": "Apa Kepanjangan Dari RA (Registration Authority)?", "id": "Registration Authority." },
  { "en": "Apa Itu Sertifikat Digital?", "id": "Dokumen Elektronik (Mengikat Kunci Publik)." },
  { "en": "Apa Standar Format Sertifikat Digital?", "id": "X.509." },
  { "en": "Apa Itu Root CA (Certificate Authority)?", "id": "CA (Certificate Authority) Paling Terpercaya." },
  { "en": "Apa Itu Intermediate CA (Certificate Authority)?", "id": "CA (Certificate Authority) Perantara (Bawahan Root)." },
  { "en": "Apa Itu Chain of Trust (Rantai Kepercayaan)?", "id": "Hierarki Validasi Sertifikat (Ke Root)." },
  { "en": "Apa Itu Certificate Revocation List (CRL)?", "id": "Daftar Sertifikat Yang Dicabut (Tidak Valid)." },
  { "en": "Apa Itu OCSP (Online Certificate Status Protocol) Stapling?", "id": "Optimalisasi OCSP (Online Certificate Status Protocol)." },
  { "en": "Apa Itu Key Management (Manajemen Kunci)?", "id": "Siklus Hidup Pengelolaan Kunci Kriptografi." },
  { "en": "Apa Itu Key Generation (Pembuatan Kunci)?", "id": "Proses Membuat Kunci Kriptografi." },
  { "en": "Apa Itu Key Distribution (Distribusi Kunci)?", "id": "Proses Membagikan Kunci Secara Aman." },
  { "en": "Apa Itu Key Storage (Penyimpanan Kunci)?", "id": "Proses Menyimpan Kunci Secara Aman." },
  { "en": "Apa Itu Key Escrow (Penyimpanan Kunci)?", "id": "Penyimpanan Kunci (Oleh Pihak Ketiga)." },
  { "en": "Apa Itu Key Recovery (Pemulihan Kunci)?", "id": "Proses Memulihkan Kunci (Hilang Rusak)." },
  { "en": "Apa Itu Key Revocation (Pencabutan Kunci)?", "id": "Proses Membatalkan Kunci (Terkompromi)." },
  { "en": "Apa Itu Hardware Security Module (HSM)?", "id": "Perangkat Keras (Keamanan Kunci Kriptografi)." },
  { "en": "Apa Itu Trusted Platform Module (TPM)?", "id": "Chip Kriptografi (Di Motherboard)." },
  { "en": "Apa Itu Steganografi?", "id": "Seni Menyembunyikan Data (Dalam Media Lain)." },
  { "en": "Mengapa Chain of Custody Penting?", "id": "Menjaga Integritas Bukti (Hukum)." },
  { "en": "Apa Itu Order of Volatility?", "id": "Urutan Pengumpulan Bukti (Paling Volatil)." },
  { "en": "Apa Data Paling Volatil?", "id": "Register CPU (Central Processing Unit), Cache." },
  { "en": "Apa Data Non-Volatil?", "id": "Data Di Hard Disk (HDD)." },
  { "en": "Apa Itu Akuisisi Data?", "id": "Proses Pengumpulan Data Bukti Digital." },
  { "en": "Mengapa Menggunakan Image Forensik?", "id": "Analisis Tanpa Mengubah Bukti Asli." },
  { "en": "Apa Fungsi Hashing (Forensik)?", "id": "Memvalidasi Integritas Data Bukti." },
  { "en": "Algoritma Hash Apa Digunakan (Forensik)?", "id": "MD5 (Message Digest 5), SHA-1 (Secure Hash Algorithm 1)." },
  { "en": "Apa Itu Analisis Slack Space?", "id": "Menganalisis Sisa Ruang Cluster File." },
  { "en": "Apa Itu Analisis Unallocated Space?", "id": "Menganalisis Ruang Disk (Data Terhapus)." },
  { "en": "Apa Itu File Carving?", "id": "Proses Pemulihan File Dihapus (Header)." },
  { "en": "Apa Itu Metadata?", "id": "Data Tentang Data (Informasi File)." },
  { "en": "Apa Itu Analisis Timeline?", "id": "Merekonstruksi Urutan Waktu Kejadian." },
  { "en": "Apa Itu Forensik Jaringan (Network Forensics)?", "id": "Investigasi Bukti (Lalu Lintas Jaringan)." },
  { "en": "Apa Itu Packet Capture (Pcap)?", "id": "Rekaman Lalu Lintas Jaringan." },
  { "en": "Apa Kepanjangan Dari Pcap (Packet Capture)?", "id": "Packet Capture." },
  { "en": "Alat Apa Digunakan (Packet Capture)?", "id": "Wireshark, Tcpdump." },
  { "en": "Apa Itu Analisis Log?", "id": "Memeriksa File Log (Sistem, Aplikasi)." },
  { "en": "Apa Itu Forensik Memori (Memory Forensics)?", "id": "Investigasi Bukti Di RAM (Random Access Memory)." },
  { "en": "Apa Itu Memory Dump?", "id": "Salinan Isi RAM (Random Access Memory)." },
  { "en": "Alat Apa Digunakan (Memory Forensics)?", "id": "Volatility." },
  { "en": "Apa Itu Forensik Seluler (Mobile Forensics)?", "id": "Investigasi Bukti (Perangkat Seluler)." },
  { "en": "Apa Itu Forensik Cloud?", "id": "Investigasi Bukti (Layanan Cloud)." },
  { "en": "Apa Itu Anti-Forensik?", "id": "Teknik Menghalangi Investigasi Forensik." },
  { "en": "Contoh Anti-Forensik?", "id": "Enkripsi, Penghapusan Data, Steganografi." },
  { "en": "Apa Itu Steganografi?", "id": "Menyembunyikan Data Dalam File Lain." },
  { "en": "Apa Itu Laporan Forensik?", "id": "Dokumen Hasil Temuan Investigasi." },
  { "en": "Apa Itu Saksi Ahli (Expert Witness)?", "id": "Spesialis (Memberi Kesaksian Forensik)." },
  { "en": "Apa Itu Etika Digital (Digital Ethics)?", "id": "Prinsip Moral (Penggunaan Teknologi Digital)." },
  { "en": "Apa Itu Privasi (Privacy)?", "id": "Hak Individu (Mengontrol Informasi Pribadi)." },
  { "en": "Apa Itu Anonimitas (Anonymity)?", "id": "Keadaan (Identitas Tidak Diketahui)." },
  { "en": "Apa Itu Personally Identifiable Information (PII)?", "id": "Informasi Identifikasi Pribadi." },
  { "en": "Contoh PII (Personally Identifiable Information)?", "id": "Nama, Nomor KTP, Alamat." },
  { "en": "Apa Itu Data Breach (Pelanggaran Data)?", "id": "Insiden (Akses Data Tidak Sah)." },
  { "en": "Apa Itu Identity Theft (Pencurian Identitas)?", "id": "Pencurian (Penggunaan Identitas Orang Lain)." },
  { "en": "Apa Itu Surveillance (Pengawasan)?", "id": "Pemantauan Aktivitas Perilaku." },
  { "en": "Apa Itu Hak Cipta (Copyright)?", "id": "Hak Legal (Perlindungan Karya Cipta)." },
  { "en": "Apa Itu Fair Use (Penggunaan Wajar)?", "id": "Pengecualian Hak Cipta (Terbatas)." },
  { "en": "Apa Itu Paten (Patent)?", "id": "Hak Eksklusif (Penemuan Baru)." },
  { "en": "Apa Itu Merek Dagang (Trademark)?", "id": "Simbol (Identifikasi Merek Barang)." },
  { "en": "Apa Itu Rahasia Dagang (Trade Secret)?", "id": "Informasi Bisnis Rahasia (Formula)." },
  { "en": "Apa Itu Digital Millennium Copyright Act (DMCA)?", "id": "Hukum Hak Cipta Digital (AS)." },
  { "en": "Apa Kepanjangan Dari DMCA (Digital Millennium Copyright Act)?", "id": "Digital Millennium Copyright Act." },
  { "en": "Apa Itu Plagiarisme?", "id": "Mengambil Karya Orang (Mengakui Milik Sendiri)." },
  { "en": "Apa Itu Cyberbullying (Perundungan Siber)?", "id": "Perundungan (Menggunakan Teknologi Digital)." },
  { "en": "Apa Itu Sexting?", "id": "Mengirim Pesan Gambar Seksual." },
  { "en": "Apa Itu Grooming (Online)?", "id": "Predator (Membangun Hubungan Anak)." },
  { "en": "Apa Itu Catfishing?", "id": "Membuat Identitas Palsu (Menipu Online)." },
  { "en": "Apa Itu Hoax (Berita Bohong)?", "id": "Informasi Palsu (Sengaja Disebarkan)." },
  { "en": "Apa Itu Misinformation?", "id": "Informasi Salah (Tidak Sengaja)." },
  { "en": "Apa Itu Disinformation?", "id": "Informasi Salah (Sengaja Menipu)." },
  { "en": "Apa Itu Fake News (Berita Palsu)?", "id": "Informasi Salah (Menyerupai Berita)." },
  { "en": "Apa Itu Deepfake?", "id": "Video Audio Palsu (AI)." },
  { "en": "Apa Itu Filter Bubble (Gelembung Filter)?", "id": "Isolasi Informasi (Akibat Algoritma)." },
  { "en": "Apa Itu Echo Chamber (Ruang Gema)?", "id": "Informasi (Memperkuat Keyakinan Sendiri)." },
  { "en": "Apa Itu Confirmation Bias (Bias Konfirmasi)?", "id": "Mencari Informasi (Pembenaran Keyakinan)." },
  { "en": "Apa Itu Literasi Digital?", "id": "Kemampuan (Menggunakan Teknologi Digital)." },
  { "en": "Apa Itu Jejak Digital (Digital Footprint)?", "id": "Jejak Data (Aktivitas Online)." },
  { "en": "Apa Itu Jejak Digital Pasif?", "id": "Jejak (Ditinggalkan Tanpa Sadar, IP)." },
  { "en": "Apa Itu Jejak Digital Aktif?", "id": "Jejak (Ditinggalkan Sengaja, Posting)." },
  { "en": "Apa Itu Kecanduan Internet?", "id": "Penggunaan Internet Berlebihan (Kompulsif)." },
  { "en": "Apa Itu Nomophobia?", "id": "Ketakutan (Tanpa Ponsel)." },
  { "en": "Apa Itu Cyberstalking?", "id": "Penguntitan (Menggunakan Media Elektronik)." },
  { "en": "Apa Itu Doxing?", "id": "Mempublikasikan Informasi Pribadi (Online)." },
  { "en": "Apa Itu Swatting?", "id": "Laporan Palsu (Mengirim Polisi SWAT)." },
  { "en": "Apa Itu Kesenjangan Digital (Digital Divide)?", "id": "Kesenjangan Akses Teknologi Digital." },
  { "en": "Apa Itu E-Waste (Limbah Elektronik)?", "id": "Limbah Peralatan Elektronik Bekas." },
  { "en": "Apa Itu Green Computing?", "id": "Komputasi Ramah Lingkungan (Efisien Energi)." },
  { "en": "Apa Itu Tata Kelola Internet (Internet Governance)?", "id": "Pengembangan Aturan Pengelolaan Internet." },
  { "en": "Apa Itu Internet Society (ISOC)?", "id": "Organisasi Nirlaba (Pengembangan Internet)." },
  { "en": "Apa Kepanjangan Dari ISOC (Internet Society)?", "id": "Internet Society." },
  { "en": "Apa Itu World Wide Web Consortium (W3C)?", "id": "Organisasi Standar Web (HTML, CSS)." },
  { "en": "Apa Kepanjangan Dari W3C (World Wide Web Consortium)?", "id": "World Wide Web Consortium." },
  { "en": "Apa Itu Internet Engineering Task Force (IETF)?", "id": "Organisasi Standar Protokol Internet." },
  { "en": "Apa Itu Internet Research Task Force (IRTF)?", "id": "Organisasi Riset Jangka Panjang Internet." },
  { "en": "Apa Kepanjangan Dari IRTF (Internet Research Task Force)?", "id": "Internet Research Task Force." },
  { "en": "Apa Itu Internet Architecture Board (IAB)?", "id": "Dewan Pengawas Arsitektur Internet." },
  { "en": "Apa Kepanjangan Dari IAB (Internet Architecture Board)?", "id": "Internet Architecture Board." },
  { "en": "Apa Itu ICANN (Internet Corporation for Assigned Names and Numbers)?", "id": "Pengelola Nama Domain IP (Internet Protocol)." },
  { "en": "Apa Itu Regional Internet Registry (RIR)?", "id": "Organisasi Regional Alokasi IP (Internet Protocol)." },
  { "en": "Apa Itu ARIN (American Registry for Internet Numbers)?", "id": "RIR (Regional Internet Registry) Amerika Utara." },
  { "en": "Apa Itu RIPE NCC (Réseaux IP Européens Network Coordination Centre)?", "id": "RIR (Regional Internet Registry) Eropa." },
  { "en": "Apa Itu Net Neutrality (Netralitas Jaringan)?", "id": "Prinsip Perlakuan Setara Trafik Internet." },
  { "en": "Apa Itu Splinternet?", "id": "Internet Yang Terfragmentasi (Sensor)." },
  { "en": "Apa Itu Kedaulatan Siber (Cyber Sovereignty)?", "id": "Kedaulatan Negara Di Ruang Siber." },
  { "en": "Apa Itu Perang Siber (Cyberwarfare)?", "id": "Serangan Siber (Antar Negara)." },
  { "en": "Apa Itu Terorisme Siber (Cyberterrorism)?", "id": "Serangan Siber (Motif Terorisme)." },
  { "en": "Apa Itu Artificial Intelligence (AI)?", "id": "Kecerdasan Yang Ditiru Oleh Mesin." },
  { "en": "Apa Itu Machine Learning (ML)?", "id": "Cabang AI (Artificial Intelligence) (Belajar)." },
  { "en": "Apa Itu Deep Learning (DL)?", "id": "Cabang ML (Machine Learning) (Jaringan Saraf)." },
  { "en": "Apa Kepanjangan Dari DL (Deep Learning)?", "id": "Deep Learning." },
  { "en": "Apa Itu Supervised Learning?", "id": "ML (Machine Learning) (Belajar Data Berlabel)." },
  { "en": "Apa Itu Unsupervised Learning?", "id": "ML (Machine Learning) (Belajar Data Tak Berlabel)." },
  { "en": "Apa Itu Reinforcement Learning?", "id": "ML (Machine Learning) (Belajar (Reward Punishment))." },
  { "en": "Sebutkan Tiga V (Big Data)?", "id": "Volume, Velocity, Variety." },
  { "en": "Apa Itu Variety (Big Data)?", "id": "Keberagaman Jenis Data." },
  { "en": "Apa Itu Veracity (Big Data)?", "id": "Keakuratan Kebenaran Data." },
  { "en": "Apa Itu Value (Big Data)?", "id": "Nilai Manfaat Dari Data." },
  { "en": "Apa Itu Apache Hadoop?", "id": "Kerangka Kerja (Pemrosesan Big Data)." },
  { "en": "Apa Itu MapReduce?", "id": "Model Pemrograman (Pemrosesan Paralel)." },
  { "en": "Apa Itu Apache Spark?", "id": "Mesin Pemrosesan Data (Cepat, In-Memory)." },
  { "en": "Apa Itu Data Warehouse (Gudang Data)?", "id": "Sistem Penyimpanan Data (Analisis)." },
  { "en": "Apa Itu ETL (Extract, Transform, Load)?", "id": "Proses Integrasi Data." },
  { "en": "Apa Itu Business Intelligence (BI)?", "id": "Analisis Data (Keputusan Bisnis)." },
  { "en": "Apa Itu Database Relasional?", "id": "Database (Berbasis Tabel Relasi)." },
  { "en": "Apa Itu Database Dokumen?", "id": "Database NoSQL (Not Only SQL) (Data JSON)." },
  { "en": "Apa Itu Database Key-Value?", "id": "Database NoSQL (Not Only SQL) (Pasangan Kunci-Nilai)." },
  { "en": "Apa Itu Database Kolom (Columnar)?", "id": "Database NoSQL (Not Only SQL) (Penyimpanan Kolom)." },
  { "en": "Apa Itu Database Graf (Graph)?", "id": "Database NoSQL (Not Only SQL) (Relasi Node)." },
  { "en": "Apa Itu Message Queue (Antrian Pesan)?", "id": "Sistem Komunikasi Asinkron." },
  { "en": "Contoh Message Queue?", "id": "RabbitMQ, ActiveMQ." },
  { "en": "Apa Itu MQTT (Message Queuing Telemetry Transport)?", "id": "Protokol Pesan (Ringan, IoT)." },
  { "en": "Apa Itu Model Publish/Subscribe?", "id": "Model Komunikasi (Broker, Topik)." },
  { "en": "Apa Itu Broker (Pialang Pesan)?", "id": "Server Pusat (Manajemen Pesan)." },
  { "en": "Apa Itu Topik (Pesan)?", "id": "Label Kategori Pesan (Pub/Sub)." },
  { "en": "Fungsi API (Application Programming Interface) Gateway?", "id": "Routing, Keamanan, Monitoring API (Application Programming Interface)." },
  { "en": "Apa Itu Autentikasi API (Application Programming Interface)?", "id": "Verifikasi Identitas Pengguna API (Application Programming Interface)." },
  { "en": "Apa Itu Otorisasi API (Application Programming Interface)?", "id": "Pemberian Izin Akses API (Application Programming Interface)." },
  { "en": "Apa Itu API (Application Programming Interface) Key?", "id": "Kunci Token (Autentikasi Sederhana)." },
  { "en": "Apa Itu HTTP (Hypertext Transfer Protocol) Strict Transport Security (HSTS)?", "id": "Kebijakan Keamanan (Paksa HTTPS)." },
  { "en": "Apa Kepanjangan Dari HSTS (HTTP Strict Transport Security)?", "id": "HTTP Strict Transport Security." },
  { "en": "Apa Itu Content Security Policy (CSP)?", "id": "Kebijakan Keamanan (Mencegah XSS)." },
  { "en": "Apa Kepanjangan Dari CSP (Content Security Policy)?", "id": "Content Security Policy." },
  { "en": "Apa Itu Cross-Origin Resource Sharing (CORS)?", "id": "Mekanisme Izin Permintaan Lintas Domain." },
  { "en": "Apa Itu Subresource Integrity (SRI)?", "id": "Keamanan (Memverifikasi Integritas Skrip)." },
  { "en": "Apa Kepanjangan Dari SRI (Subresource Integrity)?", "id": "Subresource Integrity." },
  { "en": "Apa Itu Public Key Pinning (PKP)?", "id": "Keamanan (Membatasi Kunci Publik, Usang)." },
  { "en": "Apa Kepanjangan Dari PKP (Public Key Pinning)?", "id": "Public Key Pinning." },
  { "en": "Apa Itu Certificate Transparency (CT)?", "id": "Sistem Audit Log Sertifikat Publik." },
  { "en": "Apa Kepanjangan Dari CT (Certificate Transparency)?", "id": "Certificate Transparency." },
  { "en": "Apa Itu Domain Controller (DC) (Windows)?", "id": "Server (Manajemen Autentikasi Domain)." },
  { "en": "Apa Itu Kerberos?", "id": "Protokol Autentikasi Jaringan (Tiket)." },
  { "en": "Apa Itu Group Policy Object (GPO)?", "id": "Kumpulan Kebijakan (Active Directory)." },
  { "en": "Apa Itu Organizational Unit (OU)?", "id": "Kontainer Objek (Active Directory)." },
  { "en": "Apa Itu Domain (Active Directory)?", "id": "Batas Administrasi (Active Directory)." },
  { "en": "Apa Itu Tree (Active Directory)?", "id": "Hierarki Domain (Namespace Sama)." },
  { "en": "Apa Itu Trust (Active Directory)?", "id": "Hubungan Kepercayaan Antar Domain." },
  { "en": "Apa Itu FSMO (Flexible Single Master Operations) Roles?", "id": "Peran Khusus Domain Controller." },
  { "en": "Apa Kepanjangan Dari FSMO (Flexible Single Master Operations)?", "id": "Flexible Single Master Operations." },
  { "en": "Apa Itu Global Catalog (GC)?", "id": "Indeks Sebagian Objek Forest." },
  { "en": "Apa Kepanjangan Dari GC (Global Catalog)?", "id": "Global Catalog." },
  { "en": "Apa Itu DNS (Domain Name System) Integration (AD)?", "id": "Integrasi Zona DNS (Active Directory)." },
  { "en": "Apa Itu SYSVOL?", "id": "Folder Replikasi (Skrip, GPO)." },
  { "en": "Apa Itu Azure Active Directory (Azure AD)?", "id": "Layanan Identitas Cloud (Microsoft)." },
  { "en": "Apa Itu Hybrid Azure AD (Active Directory) Join?", "id": "Menggabungkan AD (Active Directory) Lokal Azure." },
  { "en": "Apa Itu Active Directory Federation Services (AD FS)?", "id": "Layanan Federasi Identitas (SSO)." },
  { "en": "Apa Kepanjangan Dari AD FS (Active Directory Federation Services)?", "id": "Active Directory Federation Services." },
  { "en": "Apa Itu Ping?", "id": "Utilitas Jaringan (Menguji Konektivitas)." },
  { "en": "Protokol Apa Digunakan Ping?", "id": "ICMP (Internet Control Message Protocol)." },
  { "en": "Apa Itu Traceroute (Tracert)?", "id": "Utilitas Jaringan (Melacak Rute)." },
  { "en": "Bagaimana Traceroute Bekerja?", "id": "Menggunakan Pesan ICMP (Time Exceeded)." },
  { "en": "Apa Itu Nslookup?", "id": "Utilitas Kueri DNS (Domain Name System)." },
  { "en": "Apa Itu Dig (Domain Information Groper)?", "id": "Utilitas Kueri DNS (Domain Name System) (Unix)." },
  { "en": "Apa Itu Ipconfig?", "id": "Utilitas Konfigurasi IP (Windows)." },
  { "en": "Apa Itu Ifconfig?", "id": "Utilitas Konfigurasi IP (Unix, Lama)." },
  { "en": "Apa Itu Perintah 'Ip' (Linux)?", "id": "Utilitas Konfigurasi IP (Modern)." },
  { "en": "Apa Itu Netstat?", "id": "Utilitas (Menampilkan Koneksi Jaringan)." },
  { "en": "Apa Itu Arp (Perintah)?", "id": "Utilitas (Menampilkan Cache ARP)." },
  { "en": "Apa Itu Perintah 'Route'?", "id": "Utilitas (Menampilkan Mengubah Tabel Routing)." },
  { "en": "Apa Itu Tcpdump?", "id": "Alat Sniffer Paket (Command-Line)." },
  { "en": "Apa Itu Nmap (Network Mapper)?", "id": "Alat Pemindai Port Jaringan." },
  { "en": "Apa Itu Port Scanning (Pemindaian Port)?", "id": "Teknik (Mencari Port Terbuka)." },
  { "en": "Apa Itu TCP (Transmission Control Protocol) Connect Scan?", "id": "Pemindaian (Melakukan Three-Way Handshake)." },
  { "en": "Apa Itu TCP (Transmission Control Protocol) SYN (Synchronize) Scan?", "id": "Pemindaian (Setengah Terbuka, Stealthy)." },
  { "en": "Apa Itu UDP (User Datagram Protocol) Scan?", "id": "Pemindaian Port UDP (User Datagram Protocol)." },
  { "en": "Apa Itu Banner Grabbing?", "id": "Teknik (Mendapatkan Informasi Layanan Server)." },
  { "en": "Apa Itu Troubleshooting Model OSI (Open Systems Interconnection)?", "id": "Pendekatan Lapis Demi Lapis." },
  { "en": "Apa Itu Troubleshooting Bottom-Up?", "id": "Memulai Dari Lapis Fisik (1)." },
  { "en": "Apa Itu Troubleshooting Top-Down?", "id": "Memulai Dari Lapis Aplikasi (7)." },
  { "en": "Apa Itu Troubleshooting Divide-and-Conquer?", "id": "Memulai Dari Lapis Tengah (3)." },
  { "en": "Masalah Apa Di Lapis Data Link?", "id": "MAC (Media Access Control) Error, VLAN (Virtual Local Area Network)." },
  { "en": "Masalah Apa Di Lapis Jaringan?", "id": "Routing Salah, IP (Internet Protocol) Salah." },
  { "en": "Masalah Apa Di Lapis Transport?", "id": "Port Diblokir Firewall, TCP (Transmission Control Protocol) Error." },
  { "en": "Masalah Apa Di Lapis Aplikasi?", "id": "DNS (Domain Name System) Salah, Layanan Mati." },
  { "en": "Apa Itu Kabel Konsol (Console Cable)?", "id": "Kabel Akses Manajemen Perangkat." },
  { "en": "Apa Itu Out-of-Band (OOB) Management?", "id": "Manajemen (Lewat Jaringan Terpisah)." },
  { "en": "Apa Itu In-Band Management?", "id": "Manajemen (Lewat Jaringan Data)." },
  { "en": "Apa Itu Kabel Straight-Through?", "id": "Kabel (Urutan Pin Sama)." },
  { "en": "Apa Itu Kabel Crossover?", "id": "Kabel (Urutan Pin Silang)." },
  { "en": "Apa Itu Kabel Rollover?", "id": "Kabel (Urutan Pin Terbalik)." },
  { "en": "Kapan Kabel Rollover Digunakan?", "id": "Koneksi Port Konsol (Console)." },
  { "en": "Apa Itu Auto-MDIX (Medium-Dependent Interface Crossover)?", "id": "Fitur (Deteksi Kabel Otomatis)." },
  { "en": "Apa Kepanjangan Dari MDIX (Medium-Dependent Interface Crossover)?", "id": "Medium-Dependent Interface Crossover." },
  { "en": "Apa Itu T568A?", "id": "Standar Urutan Warna Kabel UTP (Unshielded Twisted Pair)." },
  { "en": "Apa Itu T568B?", "id": "Standar Urutan Warna Kabel (Paling Umum)." },
  { "en": "Apa Itu Cable Tester?", "id": "Alat Uji Konektivitas Kabel." },
  { "en": "Apa Itu Tone Generator And Probe?", "id": "Alat Pelacak Kabel (Fox Hound)." },
  { "en": "Apa Itu Crimper?", "id": "Alat Pemasang Konektor RJ45." },
  { "en": "Apa Itu Punch-Down Tool?", "id": "Alat Terminasi Kabel (Patch Panel)." },
  { "en": "Apa Itu Gelang Anti-Statis?", "id": "Alat Pencegah ESD (Electrostatic Discharge)." },
  { "en": "Apa Itu Self-Grounding?", "id": "Menyentuh Logam (Membuang ESD)." },
  { "en": "Apa Itu Horizontal Cabling?", "id": "Pengkabelan (IDF Ke Outlet Dinding)." },
  { "en": "Apa Itu Vertical Cabling (Backbone)?", "id": "Pengkabelan (Antar MDF IDF)." },
  { "en": "Apa Itu Smart Jack?", "id": "Demarc (Demarcation Point) (Diagnostik Provider)." },
  { "en": "Apa Itu Loopback Adapter?", "id": "Alat Tes Port (Loopback)." },
  { "en": "Apa Itu Maintenance Window (Jendela Pemeliharaan)?", "id": "Waktu Terjadwal Untuk Pemeliharaan." },
  { "en": "Apa Itu Network Documentation?", "id": "Catatan Detail Konfigurasi Jaringan." },
  { "en": "Apa Itu Network Diagram (Diagram Jaringan)?", "id": "Gambar Visual Topologi Jaringan." },
  { "en": "Apa Itu Diagram Fisik?", "id": "Gambar (Lokasi Fisik Perangkat Kabel)." },
  { "en": "Apa Itu Diagram Logis?", "id": "Gambar (Aliran Data, IP, VLAN)." },
  { "en": "Apa Itu Rack Diagram?", "id": "Gambar (Tata Letak Perangkat Rak)." },
  { "en": "Apa Itu Project Scope (Lingkup Proyek)?", "id": "Batasan Pekerjaan Yang Dilakukan." },
  { "en": "Bahasa Skrip Populer?", "id": "Python, Bash, PowerShell." },
  { "en": "Apa Itu JSON (JavaScript Object Notation)?", "id": "Format Data Ringan (Populer API)." },
  { "en": "Apa Itu Tipe 1 Hypervisor?", "id": "Hypervisor (Berjalan Langsung Di Hardware)." },
  { "en": "Apa Itu Tipe 2 Hypervisor?", "id": "Hypervisor (Berjalan Di Atas OS)." },
  { "en": "Apa Itu Pod (Kubernetes)?", "id": "Unit Terkecil (Berisi Kontainer)." },
  { "en": "Apa Itu Cluster (Kubernetes)?", "id": "Kumpulan Master Dan Node." },
  { "en": "Apa Itu Kube-Proxy?", "id": "Proxy Jaringan (Di Setiap Node)." },
  { "en": "Apa Itu Etcd (Kubernetes)?", "id": "Database Key-Value (Status Cluster)." },
  { "en": "Apa Itu Kubectl?", "id": "Alat Perintah (CLI) Kubernetes." },
  { "en": "Apa Itu Namespace (Kubernetes)?", "id": "Ruang Lingkup Virtual (Isolasi Sumber Daya)." },
  { "en": "Apa Itu ConfigMap (Kubernetes)?", "id": "Menyimpan Konfigurasi Non-Rahasia." },
  { "en": "Apa Itu Secret (Kubernetes)?", "id": "Menyimpan Informasi Sensitif (Password)." },
  { "en": "Apa Itu Ingress (Kubernetes)?", "id": "Mengelola Akses Eksternal (HTTP/HTTPS)." },
  { "en": "Apa Itu Ingress Controller?", "id": "Implementasi (Menerapkan Aturan Ingress)." },
  { "en": "Apa Itu Persistent Volume (PV)?", "id": "Penyimpanan Jaringan (Siklus Hidup Pod)." },
  { "en": "Apa Kepanjangan Dari PV (Persistent Volume)?", "id": "Persistent Volume." },
  { "en": "Apa Itu Persistent Volume Claim (PVC)?", "id": "Permintaan Penyimpanan Oleh Pengguna." },
  { "en": "Apa Kepanjangan Dari PVC (Persistent Volume Claim)?", "id": "Persistent Volume Claim." },
  { "en": "Apa Itu StatefulSet (Kubernetes)?", "id": "Mengelola Aplikasi Stateful (Database)." },
  { "en": "Apa Itu DaemonSet (Kubernetes)?", "id": "Memastikan Pod Berjalan (Setiap Node)." },
  { "en": "Apa Itu Helm Chart?", "id": "Paket Aplikasi (Templat Helm)." },
  { "en": "Apa Itu CNI (Container Network Interface)?", "id": "Standar Jaringan Kontainer Kubernetes." },
  { "en": "Contoh CNI (Container Network Interface) Plugin?", "id": "Calico, Flannel, Weave Net." },
  { "en": "Apa Itu Flannel (CNI)?", "id": "Plugin CNI (Container Network Interface) (Overlay Sederhana)." },
  { "en": "Apa Itu Sidecar Proxy?", "id": "Proxy (Ditempelkan Di Samping Kontainer)." },
  { "en": "Apa Itu Envoy (Proxy)?", "id": "Proxy Sidecar (Digunakan Istio)." },
  { "en": "Apa Itu Control Plane (Istio)?", "id": "Bagian Manajemen (Konfigurasi Proxy)." },
  { "en": "Apa Itu Data Plane (Istio)?", "id": "Bagian Data (Proxy Envoy)." },
  { "en": "Apa Itu API (Application Programming Interface) Gateway?", "id": "Gerbang Titik Masuk (Manajemen API)." },
  { "en": "Apa Itu Deep Learning (DL)?", "id": "Cabang Machine Learning (Jaringan Saraf)." },
  { "en": "Apa Itu Neural Network (Jaringan Saraf Tiruan)?", "id": "Model Komputasi (Tiruan Otak)." },
  { "en": "Apa Itu Supervised Learning?", "id": "Machine Learning (Belajar Data Berlabel)." },
  { "en": "Apa Itu Unsupervised Learning?", "id": "Machine Learning (Belajar Data Tak Berlabel)." },
  { "en": "Apa Itu Reinforcement Learning?", "id": "Machine Learning (Belajar (Reward Punishment))." },
  { "en": "Apa Itu Clustering (Unsupervised)?", "id": "Proses Pengelompokan Data (Tanpa Label)." },
  { "en": "Apa Itu Classification (Supervised)?", "id": "Proses Pemetaan Data Ke Kategori." },
  { "en": "Apa Itu Regression (Supervised)?", "id": "Proses Memprediksi Nilai Kontinu." },
  { "en": "Apa Itu Natural Language Processing (NLP)?", "id": "Pemrosesan Bahasa Manusia (AI)." },
  { "en": "Apa Kepanjangan Dari NLP (Natural Language Processing)?", "id": "Natural Language Processing." },
  { "en": "Contoh NLP (Natural Language Processing) Di Telekomunikasi?", "id": "Chatbot, Analisis Sentimen Pelanggan." },
  { "en": "Apa Itu Computer Vision (CV)?", "id": "AI (Artificial Intelligence) (Pemahaman Gambar Video)." },
  { "en": "Apa Kepanjangan Dari CV (Computer Vision)?", "id": "Computer Vision." },
  { "en": "Apa Itu Anomaly Detection (Deteksi Anomali)?", "id": "Proses Menemukan Pola Menyimpang." },
  { "en": "Penerapan Anomaly Detection (Telekomunikasi)?", "id": "Deteksi Penipuan (Fraud), Keamanan Jaringan." },
  { "en": "Apa Itu Predictive Maintenance?", "id": "Memprediksi Kapan Peralatan Akan Gagal." },
  { "en": "Manfaat Predictive Maintenance (Telekomunikasi)?", "id": "Mengurangi Downtime Jaringan (BTS)." },
  { "en": "Apa Itu Churn Prediction?", "id": "Memprediksi Pelanggan Yang Akan Berhenti." },
  { "en": "Apa Itu Customer Segmentation?", "id": "Mengelompokkan Pelanggan (Perilaku Serupa)." },
  { "en": "Apa Itu Recommender System?", "id": "Sistem (Merekomendasikan Produk Layanan)." },
  { "en": "Apa Itu Data Science (Ilmu Data)?", "id": "Ilmu (Ekstraksi Wawasan Dari Data)." },
  { "en": "Apa Itu Data Analyst (Analis Data)?", "id": "Menganalisis Data (Menjawab Pertanyaan Bisnis)." },
  { "en": "Apa Itu Data Scientist (Ilmuwan Data)?", "id": "Membangun Model Prediktif (Data)." },
  { "en": "Apa Itu Data Engineer (Insinyur Data)?", "id": "Membangun Infrastruktur (Alur Data)." },
  { "en": "Apa Itu Data Visualization (Visualisasi Data)?", "id": "Representasi Grafis Dari Data (Grafik)." },
  { "en": "Alat Apa Untuk Data Visualization?", "id": "Tableau, Power BI, Grafana." },
  { "en": "Apa Itu Business Intelligence (BI)?", "id": "Teknologi (Analisis Data Keputusan Bisnis)." },
  { "en": "Apa Itu Dashboard (BI)?", "id": "Tampilan Visual (Metrik Kinerja Utama)." },
  { "en": "Apa Itu Key Performance Indicator (KPI)?", "id": "Indikator Kunci (Pengukuran Kinerja)." },
  { "en": "Apa Kepanjangan Dari KPI (Key Performance Indicator)?", "id": "Key Performance Indicator." },
  { "en": "Apa Itu Data Warehouse (Gudang Data)?", "id": "Database (Penyimpanan Data Analisis)." },
  { "en": "Apa Itu ETL (Extract, Transform, Load)?", "id": "Proses Memasukkan Data Ke Warehouse." },
  { "en": "Apa Itu Jaringan Sosial (Social Network)?", "id": "Platform Online (Interaksi Sosial)." },
  { "en": "Apa Itu Analisis Jaringan Sosial (SNA)?", "id": "Analisis Struktur Hubungan Sosial." },
  { "en": "Apa Kepanjangan Dari SNA (Social Network Analysis)?", "id": "Social Network Analysis." },
  { "en": "Apa Itu Node (SNA)?", "id": "Entitas (Orang, Akun) Dalam Jaringan." },
  { "en": "Apa Itu Edge (SNA)?", "id": "Hubungan (Koneksi) Antar Node." },
  { "en": "Apa Itu Sentralitas (SNA)?", "id": "Ukuran Kepentingan Node (Jaringan)." },
  { "en": "Apa Itu Analisis Sentimen?", "id": "Proses (Menentukan Opini Positif/Negatif)." },
  { "en": "Penerapan Analisis Sentimen (Telekomunikasi)?", "id": "Menganalisis Umpan Balik Pelanggan." },
  { "en": "Apa Itu Self-Organizing Network (SON)?", "id": "Jaringan Seluler (Optimasi Otomatis)." },
  { "en": "Apa Kepanjangan Dari SON (Self-Organizing Network)?", "id": "Self-Organizing Network." },
  { "en": "Apa Fungsi SON (Self-Organizing Network)?", "id": "Self-Configuration, Self-Optimization, Self-Healing." },
  { "en": "Apa Itu Self-Configuration (SON)?", "id": "Instalasi Konfigurasi Otomatis (Sel Baru)." },
  { "en": "Apa Itu Self-Optimization (SON)?", "id": "Optimasi Kinerja Jaringan (Otomatis)." },
  { "en": "Apa Itu Self-Healing (SON)?", "id": "Deteksi Perbaikan Kegagalan (Otomatis)." },
  { "en": "Apa Itu Mobility Load Balancing (SON)?", "id": "Menyeimbangkan Beban Trafik Antar Sel." },
  { "en": "Apa Tujuan Network Slicing?", "id": "Menyediakan Layanan Berbeda (Satu Fisik)." },
  { "en": "Contoh Slice (Network Slicing)?", "id": "Slice eMBB (Enhanced Mobile Broadband)." },
  { "en": "Contoh Lain Slice (Network Slicing)?", "id": "Slice URLLC (Ultra-Reliable Low-Latency Communication)." },
  { "en": "Apa Itu Network Function (NF)?", "id": "Fungsi Jaringan (Virtual Atau Fisik)." },
  { "en": "Apa Kepanjangan Dari NF (Network Function)?", "id": "Network Function." },
  { "en": "Apa Itu Network Slice Instance (NSI)?", "id": "Instansi Jaringan Virtual (Slice)." },
  { "en": "Apa Kepanjangan Dari NSI (Network Slice Instance)?", "id": "Network Slice Instance." },
  { "en": "Apa Itu Network Slice Subnet Instance (NSSI)?", "id": "Sub-Jaringan Dalam Satu Slice." },
  { "en": "Apa Kepanjangan Dari NSSI (Network Slice Subnet Instance)?", "id": "Network Slice Subnet Instance." },
  { "en": "Apa Itu Network Slice Management Function (NSMF)?", "id": "Fungsi Manajemen Network Slice." },
  { "en": "Apa Kepanjangan Dari NSMF (Network Slice Management Function)?", "id": "Network Slice Management Function." },
  { "en": "Apa Itu Network Slice Selection Function (NSSF)?", "id": "Fungsi Pemilihan Slice (Untuk Perangkat)." },
  { "en": "Apa Kepanjangan Dari NSSF (Network Slice Selection Function)?", "id": "Network Slice Selection Function." },
  { "en": "Apa Itu Core Network (5GC)?", "id": "Jaringan Inti 5G (Berbasis Layanan)." },
  { "en": "Apa Itu Service-Based Architecture (SBA)?", "id": "Arsitektur 5G Core (Fungsi Terpisah)." },
  { "en": "Apa Itu Access and Mobility Management Function (AMF)?", "id": "Fungsi 5G Core (Mobilitas Akses)." },
  { "en": "Apa Kepanjangan Dari AMF (Access and Mobility Management Function)?", "id": "Access and Mobility Management Function." },
  { "en": "Apa Itu Session Management Function (SMF)?", "id": "Fungsi 5G Core (Manajemen Sesi)." },
  { "en": "Apa Itu User Plane Function (UPF)?", "id": "Fungsi 5G Core (Penerusan Data)." },
  { "en": "Apa Itu Unified Data Management (UDM)?", "id": "Fungsi 5G Core (Manajemen Data Pengguna)." },
  { "en": "Apa Kepanjangan Dari UDM (Unified Data Management)?", "id": "Unified Data Management." },
  { "en": "Apa Itu Policy Control Function (PCF)?", "id": "Fungsi 5G Core (Kebijakan QoS)." },
  { "en": "Apa Kepanjangan Dari PCF (Policy Control Function)?", "id": "Policy Control Function." },
  { "en": "Apa Itu Application Function (AF)?", "id": "Fungsi 5G Core (Interaksi Aplikasi)." },
  { "en": "Apa Kepanjangan Dari AF (Application Function)?", "id": "Application Function." },
  { "en": "Apa Itu Network Repository Function (NRF)?", "id": "Fungsi 5G Core (Service Discovery)." },
  { "en": "Apa Kepanjangan Dari NRF (Network Repository Function)?", "id": "Network Repository Function." },
  { "en": "Apa Itu Network Exposure Function (NEF)?", "id": "Fungsi 5G Core (Ekspos Layanan)." },
  { "en": "Apa Kepanjangan Dari NEF (Network Exposure Function)?", "id": "Network Exposure Function." },
  { "en": "Apa Itu Authentication Server Function (AUSF)?", "id": "Fungsi 5G Core (Autentikasi)." },
  { "en": "Apa Kepanjangan Dari AUSF (Authentication Server Function)?", "id": "Authentication Server Function." },
  { "en": "Apa Itu Control and User Plane Separation (CUPS)?", "id": "Pemisahan Control Plane User Plane." },
  { "en": "Apa Kepanjangan Dari CUPS (Control and User Plane Separation)?", "id": "Control and User Plane Separation." },
  { "en": "Dimana CUPS (Control and User Plane Separation) Diterapkan?", "id": "Jaringan 4G (EPC) 5G (5GC)." },
  { "en": "Apa Keuntungan CUPS (Control and User Plane Separation)?", "id": "Skalabilitas Fleksibilitas Penempatan UPF." },
  { "en": "Apa Itu Data Center Interconnect (DCI)?", "id": "Koneksi Jaringan Antar Data Center." },
  { "en": "Apa Kepanjangan Dari DCI (Data Center Interconnect)?", "id": "Data Center Interconnect." },
  { "en": "Teknologi Apa Digunakan DCI (Data Center Interconnect)?", "id": "DWDM (Dense WDM), OTV (Overlay Transport Virtualization)." },
  { "en": "Apa Itu OTV (Overlay Transport Virtualization)?", "id": "Teknologi DCI (Data Center Interconnect) Lapis 2." },
  { "en": "Apa Itu EVPN-VXLAN (Ethernet VPN-Virtual Extensible LAN)?", "id": "Teknologi Overlay (DCI Data Center)." },
  { "en": "Apa Itu Submarine Cable (Kabel Bawah Laut)?", "id": "Kabel Komunikasi (Optik) Di Laut." },
  { "en": "Apa Itu Cable Landing Station (CLS)?", "id": "Stasiun Pendaratan Kabel Bawah Laut." },
  { "en": "Apa Kepanjangan Dari CLS (Cable Landing Station)?", "id": "Cable Landing Station." },
  { "en": "Apa Itu Repeater (Kabel Bawah Laut)?", "id": "Penguat Sinyal Optik (Bawah Laut)." },
  { "en": "Sumber Daya Repeater Bawah Laut?", "id": "Listrik Tegangan Tinggi Dari CLS (Cable Landing Station)." },
  { "en": "Apa Itu Kapal Kabel (Cable Ship)?", "id": "Kapal Khusus (Instalasi Perbaikan Kabel)." },
    { "en": "Apa Itu Malware?", "id": "Perangkat Lunak Berbahaya (Merusak Sistem)." },
  { "en": "Apa Itu Virus Komputer?", "id": "Malware (Memerlukan Inang File Menyebar)." },
  { "en": "Apa Itu Worm (Cacing Komputer)?", "id": "Malware (Menyebar Sendiri Lewat Jaringan)." },
  { "en": "Apa Itu Trojan (Kuda Troya)?", "id": "Malware (Menyamar Sebagai Program Legal)." },
  { "en": "Apa Itu Ransomware?", "id": "Malware (Mengenkripsi Data Meminta Tebusan)." },
  { "en": "Apa Itu Spyware?", "id": "Malware (Memata-matai Aktivitas Pengguna)." },
  { "en": "Apa Itu Adware?", "id": "Malware (Menampilkan Iklan Tidak Diinginkan)." },
  { "en": "Apa Itu Rootkit?", "id": "Malware (Menyembunyikan Akses (Level Root))." },
  { "en": "Apa Itu Keylogger?", "id": "Malware (Merekam Ketukan Tombol Keyboard)." },
  { "en": "Apa Itu Backdoor (Pintu Belakang)?", "id": "Akses Rahasia (Membypass Keamanan)." },
  { "en": "Apa Itu Botnet?", "id": "Jaringan Komputer Zombie (Terkendali)." },
  { "en": "Apa Itu Serangan DDoS (Distributed Denial of Service)?", "id": "Serangan (Melumpuhkan Layanan, Banyak Sumber)." },
  { "en": "Apa Itu Serangan DoS (Denial of Service)?", "id": "Serangan (Melumpuhkan Layanan, Satu Sumber)." },
  { "en": "Apa Itu Phishing?", "id": "Penipuan (Mendapatkan Informasi Sensitif, Email)." },
  { "en": "Apa Itu Spear Phishing?", "id": "Phishing (Tertarget Spesifik Individu)." },
  { "en": "Apa Itu Whaling (Serangan Phishing)?", "id": "Spear Phishing (Menargetkan Eksekutif Tinggi)." },
  { "en": "Apa Itu Vishing?", "id": "Phishing (Menggunakan Panggilan Suara)." },
  { "en": "Apa Itu Smishing?", "id": "Phishing (Menggunakan Pesan Teks SMS)." },
  { "en": "Apa Itu Pharming?", "id": "Pengalihan Otomatis Ke Situs Web Palsu." },
  { "en": "Apa Itu Social Engineering?", "id": "Manipulasi Psikologis (Mendapatkan Informasi)." },
  { "en": "Apa Itu Man-in-the-Middle (MITM) Attack?", "id": "Serangan (Pencegatan Komunikasi Dua Pihak)." },
  { "en": "Apa Itu ARP (Address Resolution Protocol) Poisoning?", "id": "Serangan (Memalsukan Cache ARP, MITM)." },
  { "en": "Apa Itu DNS (Domain Name System) Poisoning?", "id": "Serangan (Meracuni Cache DNS, Pengalihan)." },
  { "en": "Apa Itu SQL (Structured Query Language) Injection?", "id": "Serangan (Menyisipkan Perintah SQL Jahat)." },
  { "en": "Apa Itu Cross-Site Scripting (XSS)?", "id": "Serangan (Menyisipkan Skrip Jahat Website)." },
  { "en": "Apa Itu Cross-Site Request Forgery (CSRF)?", "id": "Serangan (Memaksa Pengguna Melakukan Aksi)." },
  { "en": "Apa Itu Serangan Brute Force?", "id": "Serangan (Menebak Password, Semua Kombinasi)." },
  { "en": "Apa Itu Serangan Dictionary?", "id": "Serangan (Menebak Password, Daftar Kata)." },
  { "en": "Apa Itu Serangan Rainbow Table?", "id": "Serangan (Membalikkan Hash, Tabel Hash)." },
  { "en": "Apa Itu Salting (Password)?", "id": "Menambahkan Data Acak (Sebelum Hashing)." },
  { "en": "Apa Itu Key Stretching?", "id": "Memperlambat Proses Hashing (Iterasi)." },
  { "en": "Apa Itu Serangan Zero-Day?", "id": "Serangan (Memanfaatkan Celah Belum Diketahui)." },
  { "en": "Apa Itu Exploit?", "id": "Kode (Memanfaatkan Kerentanan Sistem)." },
  { "en": "Apa Itu Vulnerability (Kerentanan)?", "id": "Kelemahan Sistem (Yang Bisa Dieksploitasi)." },
  { "en": "Apa Itu Threat (Ancaman)?", "id": "Potensi Bahaya (Yang Mengeksploitasi)." },
  { "en": "Apa Itu Firewall?", "id": "Perangkat Keamanan (Penyaring Lalu Lintas)." },
  { "en": "Apa Itu Access Control List (ACL)?", "id": "Daftar Aturan (Izin Tolak Trafik)." },
  { "en": "Apa Itu Stateful Firewall?", "id": "Firewall (Melacak Status Koneksi Aktif)." },
  { "en": "Apa Itu Stateless Firewall?", "id": "Firewall (Memeriksa Paket Individual Statis)." },
  { "en": "Apa Itu Deep Packet Inspection (DPI)?", "id": "Inspeksi Mendalam (Isi Paket Data)." },
  { "en": "Apa Itu Intrusion Detection System (IDS)?", "id": "Sistem (Mendeteksi Aktivitas Mencurigakan)." },
  { "en": "Apa Itu Intrusion Prevention System (IPS)?", "id": "Sistem (Mendeteksi Dan Mencegah Intrusi)." },
  { "en": "Apa Itu NIDS (Network IDS)?", "id": "IDS (Intrusion Detection System) (Memantau Lalu Lintas Jaringan)." },
  { "en": "Apa Itu HIDS (Host-based IDS)?", "id": "IDS (Intrusion Detection System) (Memantau Aktivitas Host)." },
  { "en": "Apa Itu Signature-Based Detection (Deteksi)?", "id": "Deteksi (Berbasis Pola Serangan Dikenal)." },
  { "en": "Apa Itu Anomaly-Based Detection (Deteksi)?", "id": "Deteksi (Berbasis Penyimpangan Perilaku Normal)." },
  { "en": "Apa Itu False Positive (Keamanan)?", "id": "Peringatan Palsu (Normal Dianggap Ancaman)." },
  { "en": "Apa Itu False Negative (Keamanan)?", "id": "Kegagalan Deteksi (Ancaman Dianggap Normal)." },
  { "en": "Apa Itu Honeypot?", "id": "Sistem Umpan (Menjebak Penyerang)." },
  { "en": "Apa Itu 802.1X?", "id": "Standar Autentikasi Jaringan (Port-Based)." },
  { "en": "Apa Itu RADIUS (Remote Authentication Dial-In User Service)?", "id": "Protokol Server Autentikasi (802.1X)." },
  { "en": "Apa Itu Tanda Tangan Digital?", "id": "Verifikasi (Autentikasi, Integritas, Non-Repudiation)." },
  { "en": "Apa Itu Public Key Infrastructure (PKI)?", "id": "Infrastruktur Pengelola Sertifikat Digital." },
  { "en": "Apa Itu Hashing (Forensik)?", "id": "Memvalidasi Integritas Data Bukti." },
  { "en": "Apa Itu Steganografi?", "id": "Seni Menyembunyikan Data (Dalam Media)." },
  { "en": "Apa Itu Artificial Intelligence (AI) Di Telekomunikasi?", "id": "AI (Artificial Intelligence) Mengoptimalkan Jaringan." },
  { "en": "Apa Itu Machine Learning (ML) Di Telekomunikasi?", "id": "ML (Machine Learning) Memprediksi Kinerja Jaringan." },
  { "en": "Apa Itu Deep Learning (DL) Di Telekomunikasi?", "id": "DL (Deep Learning) Analisis Trafik Kompleks." },
  { "en": "Contoh AI (Artificial Intelligence) Di Jaringan Seluler?", "id": "Self-Organizing Networks (SON)." },
  { "en": "Apa Itu Self-Organizing Networks (SON)?", "id": "Jaringan Mengoptimalkan Diri (AI)." },
  { "en": "Apa Kepanjangan Dari SON (Self-Organizing Networks)?", "id": "Self-Organizing Networks." },
  { "en": "Apa Itu Predictive Maintenance (Telekomunikasi)?", "id": "Memprediksi Kegagalan Perangkat Jaringan." },
  { "en": "Apa Itu Anomaly Detection (Deteksi Anomali)?", "id": "Mendeteksi Perilaku Jaringan Tidak Normal." },
  { "en": "Bagaimana AI (Artificial Intelligence) Membantu Keamanan Jaringan?", "id": "Mendeteksi Ancaman Siber (Real-Time)." },
  { "en": "Apa Itu Virtual Assistant (Asisten Virtual)?", "id": "Program AI (Artificial Intelligence) Layanan Pelanggan." },
  { "en": "Contoh Virtual Assistant (Telekomunikasi)?", "id": "Chatbot Operator Seluler." },
  { "en": "Apa Itu Big Data (Telekomunikasi)?", "id": "Volume Data Sangat Besar (Jaringan)." },
  { "en": "Data Apa Dikumpulkan Operator Telekomunikasi?", "id": "Call Detail Records (CDR), Trafik Data." },
  { "en": "Apa Itu Call Detail Record (CDR)?", "id": "Catatan Detail Setiap Panggilan Telepon." },
  { "en": "Apa Manfaat Analisis Big Data (Telekomunikasi)?", "id": "Optimasi Jaringan, Pemasaran Tertarget." },
  { "en": "Bagaimana AI (Artificial Intelligence) Mencegah Churn?", "id": "Memberikan Penawaran Personalisasi." },
  { "en": "Apa Itu Customer Segmentation (Segmentasi Pelanggan)?", "id": "Mengelompokkan Pelanggan (Perilaku Serupa)." },
  { "en": "Apa Itu Robotic Process Automation (RPA)?", "id": "Otomatisasi Proses Bisnis (Robot Software)." },
  { "en": "Apa Kepanjangan Dari RPA (Robotic Process Automation)?", "id": "Robotic Process Automation." },
  { "en": "Contoh RPA (Robotic Process Automation) (Telekomunikasi)?", "id": "Otomatisasi Aktivasi Layanan Baru." },
  { "en": "Penerapan NLP (Natural Language Processing) (Telekomunikasi)?", "id": "Analisis Sentimen Umpan Balik Pelanggan." },
  { "en": "Apa Itu Computer Vision (CV)?", "id": "AI (Artificial Intelligence) (Analisis Gambar Video)." },
  { "en": "Penerapan CV (Computer Vision) (Telekomunikasi)?", "id": "Inspeksi Infrastruktur (Menara BTS)." },
  { "en": "Apa Itu Digital Twin (Kembaran Digital)?", "id": "Representasi Digital Objek Fisik." },
  { "en": "Fungsi Digital Twin (Telekomunikasi)?", "id": "Simulasi Optimasi Jaringan Nirkabel." },
  { "en": "Apa Itu AIOps (AI for IT Operations)?", "id": "AI (Artificial Intelligence) Operasi IT." },
  { "en": "Apa Kepanjangan Dari AIOps (AI for IT Operations)?", "id": "Artificial Intelligence for IT Operations." },
  { "en": "Apa Itu Etika AI (Artificial Intelligence)?", "id": "Prinsip Moral (Penggunaan AI)." },
  { "en": "Apa Itu Bias (AI)?", "id": "Prasangka Sistematis (Data Algoritma)." },
  { "en": "Apa Itu Explainable AI (XAI)?", "id": "AI (Artificial Intelligence) (Keputusan Dapat Dijelaskan)." },
  { "en": "Apa Kepanjangan Dari XAI (Explainable AI)?", "id": "Explainable AI." },
  { "en": "Apa Itu Model Federated Learning?", "id": "Melatih Model AI (Artificial Intelligence) Lokal." },
  { "en": "Keuntungan Federated Learning?", "id": "Menjaga Privasi Data Pengguna." },
  { "en": "Apa Itu Jaringan 6G?", "id": "Jaringan Seluler Generasi Keenam (Masa Depan)." },
  { "en": "Kapan 6G Diperkirakan Hadir?", "id": "Sekitar Tahun 2030." },
  { "en": "Visi Jaringan 6G?", "id": "Konektivitas Cerdas, Imersif, Terabit." },
  { "en": "Teknologi Kunci Potensial 6G?", "id": "AI (Artificial Intelligence), THz, RIS." },
  { "en": "Apa Itu Frekuensi Terahertz (THz)?", "id": "Frekuensi Sangat Tinggi (Di Atas EHF)." },
  { "en": "Keuntungan Frekuensi Terahertz (THz)?", "id": "Bandwidth Sangat Besar (Kecepatan Terabit)." },
  { "en": "Tantangan Frekuensi Terahertz (THz)?", "id": "Jangkauan Sangat Pendek, Atenuasi Tinggi." },
  { "en": "Apa Itu Reconfigurable Intelligent Surface (RIS)?", "id": "Permukaan Cerdas (Memantulkan Sinyal)." },
  { "en": "Apa Kepanjangan Dari RIS (Reconfigurable Intelligent Surface)?", "id": "Reconfigurable Intelligent Surface." },
  { "en": "Fungsi RIS (Reconfigurable Intelligent Surface)?", "id": "Mengoptimalkan Kanal Nirkabel (Secara Pasif)." },
  { "en": "Apa Itu Komunikasi Holografik?", "id": "Transmisi Tampilan Hologram (3D Real-Time)." },
  { "en": "Apa Itu Internet of Senses (IoS)?", "id": "Internet (Melibatkan Indra Manusia)." },
  { "en": "Apa Itu Jaringan Non-Terrestrial (NTN)?", "id": "Jaringan (Satelit, Drone)." },
  { "en": "Apa Kepanjangan Dari NTN (Non-Terrestrial Network)?", "id": "Non-Terrestrial Network." },
  { "en": "Apa Itu High-Altitude Platform Station (HAPS)?", "id": "Stasiun Platform (Di Stratosfer)." },
  { "en": "Apa Kepanjangan Dari HAPS (High-Altitude Platform Station)?", "id": "High-Altitude Platform Station." },
  { "en": "Contoh HAPS (High-Altitude Platform Station)?", "id": "Balon Udara, Pesawat Tenaga Surya." },
  { "en": "Apa Itu Komunikasi Kuantum (Quantum)?", "id": "Komunikasi Berbasis Mekanika Kuantum." },
  { "en": "Apa Itu Quantum Key Distribution (QKD)?", "id": "Distribusi Kunci Kriptografi (Kuantum)." },
  { "en": "Apa Itu Quantum Cryptography?", "id": "Kriptografi Menggunakan Prinsip Kuantum." },
  { "en": "Apa Itu Komputasi Kuantum (Quantum Computing)?", "id": "Komputasi (Menggunakan Qubit)." },
  { "en": "Apa Itu Qubit (Quantum Bit)?", "id": "Unit Dasar Komputasi Kuantum." },
  { "en": "Apa Itu Superposisi (Kuantum)?", "id": "Qubit (Bisa Bernilai 0 Dan 1)." },
  { "en": "Apa Itu Entanglement (Kuantum)?", "id": "Keterikatan Kuantum (Antar Qubit)." },
  { "en": "Apa Ancaman Komputasi Kuantum?", "id": "Dapat Memecahkan Enkripsi Asimetris (RSA)." },
  { "en": "Apa Itu Jaringan Data Center (DCN)?", "id": "Jaringan Internal Data Center." },
  { "en": "Apa Kepanjangan Dari DCN (Data Center Network)?", "id": "Data Center Network." },
  { "en": "Apa Itu Arsitektur Tiga Tingkat (Tradisional)?", "id": "Core, Agregasi (Distribusi), Akses." },
  { "en": "Apa Kelemahan Arsitektur Tiga Tingkat?", "id": "Kurang Optimal (Trafik East-West)." },
  { "en": "Apa Itu Arsitektur Spine-Leaf (Clos)?", "id": "Arsitektur Jaringan Data Center Modern." },
  { "en": "Apa Itu Leaf Switch?", "id": "Switch Akses (Terhubung Server, ToR)." },
  { "en": "Apa Itu Spine Switch?", "id": "Switch Core (Terhubung Semua Leaf)." },
  { "en": "Bagaimana Koneksi Spine-Leaf?", "id": "Setiap Leaf Terhubung Ke Setiap Spine." },
  { "en": "Apakah Leaf Terhubung Ke Leaf?", "id": "Tidak (Dalam Arsitektur Murni)." },
  { "en": "Apakah Spine Terhubung Ke Spine?", "id": "Tidak (Dalam Arsitektur Murni)." },
  { "en": "Topologi Apa Mengoptimalkan East-West Traffic?", "id": "Spine-Leaf." },
  { "en": "Apa Itu Jaringan Overlay?", "id": "Jaringan Virtual (Di Atas Fisik)." },
  { "en": "Apa Itu Jaringan Underlay?", "id": "Jaringan Fisik (Transport Dasar)." },
  { "en": "Contoh Teknologi Overlay (Data Center)?", "id": "VXLAN (Virtual Extensible LAN), NVGRE, Geneve." },
  { "en": "Berapa Banyak VNI (VXLAN Network Identifier) Didukung?", "id": "Sekitar 16 Juta." },
  { "en": "Apa Itu Control Plane (VXLAN)?", "id": "Mekanisme Pemetaan (Host Ke VTEP)." },
  { "en": "Apa Itu Multicast Control Plane (VXLAN)?", "id": "Menggunakan Multicast (Menemukan VTEP)." },
  { "en": "Apa Itu EVPN (Ethernet VPN) Control Plane?", "id": "Menggunakan BGP (Border Gateway Protocol) (Kontrol VXLAN)." },
  { "en": "Apa Keuntungan EVPN (Ethernet VPN) VXLAN?", "id": "Skalabel, Standar Terbuka." },
  { "en": "Apa Itu NVGRE (Network Virtualization using GRE)?", "id": "Teknologi Overlay (Microsoft)." },
  { "en": "Apa Kepanjangan Dari NVGRE (Network Virtualization using GRE)?", "id": "Network Virtualization using Generic Routing Encapsulation." },
  { "en": "Apa Itu Geneve (Generic Network Virtualization Encapsulation)?", "id": "Protokol Enkapsulasi Fleksibel (Overlay)." },
  { "en": "Apa Itu VMware NSX?", "id": "Platform Virtualisasi Jaringan (VMware)." },
  { "en": "Apa Itu Data Center Interconnect (DCI)?", "id": "Menghubungkan Dua Data Center." },
  { "en": "Apa Itu High-Performance Computing (HPC)?", "id": "Komputasi (Daya Komputasi Sangat Tinggi)." },
  { "en": "Jaringan Apa Digunakan HPC (High-Performance Computing)?", "id": "Infiniband, Ethernet Kecepatan Tinggi." },
  { "en": "Apa Itu Infiniband?", "id": "Standar Jaringan (Latensi Sangat Rendah)." },
  { "en": "Apa Itu Remote Direct Memory Access (RDMA)?", "id": "Akses Memori Langsung (Lewat Jaringan)." },
  { "en": "Apa Kepanjangan Dari RDMA (Remote Direct Memory Access)?", "id": "Remote Direct Memory Access." },
  { "en": "Apa Itu RDMA (Remote Direct Memory Access) Over Converged Ethernet (RoCE)?", "id": "RDMA (Remote Direct Memory Access) Di Atas Ethernet." },
  { "en": "Apa Kepanjangan Dari RoCE (RDMA Over Converged Ethernet)?", "id": "RDMA (Remote Direct Memory Access) Over Converged Ethernet." },
  { "en": "Apa Itu Internet2?", "id": "Jaringan Penelitian Pendidikan (Kecepatan Tinggi)." },
  { "en": "Apa Itu ESnet (Energy Sciences Network)?", "id": "Jaringan Departemen Energi AS (Riset)." },
  { "en": "Apa Kepanjangan Dari ESnet (Energy Sciences Network)?", "id": "Energy Sciences Network." },
  { "en": "Apa Itu Tier 1 ISP (Internet Service Provider)?", "id": "ISP (Internet Service Provider) Global (Backbone)." },
  { "en": "Apa Itu Tier 2 ISP (Internet Service Provider)?", "id": "ISP (Internet Service Provider) Regional (Peering/Transit)." },
  { "en": "Apa Itu Tier 3 ISP (Internet Service Provider)?", "id": "ISP (Internet Service Provider) Lokal (Akses Pengguna)." },
  { "en": "Apa Itu Peering (Internet)?", "id": "Pertukaran Trafik Langsung (Antar ISP)." },
  { "en": "Apa Itu Exterior Gateway Protocol (EGP)?", "id": "Protokol Routing (Antar AS)." },
  { "en": "Apa Itu Interior Gateway Protocol (IGP)?", "id": "Protokol Routing (Dalam Satu AS)." },
  { "en": "Apa Itu Point of Presence (PoP)?", "id": "Titik Akses Jaringan (Provider)." },
  { "en": "Apa Itu Colocation (Colo)?", "id": "Menyewa Ruang Rak (Data Center)." },
  { "en": "Apa Itu Meet-Me Room (MMR)?", "id": "Ruang Koneksi Antar Provider (Data Center)." },
  { "en": "Apa Kepanjangan Dari MMR (Meet-Me Room)?", "id": "Meet-Me Room." },
  { "en": "Apa Itu Cross-Connect (Koneksi Silang)?", "id": "Kabel Koneksi Langsung (Antar Rak)." },
  { "en": "Apa Itu Dark Fiber?", "id": "Serat Optik (Tidak Terpakai, Belum Aktif)." },
  { "en": "Apa Itu Lit Fiber?", "id": "Serat Optik (Sudah Aktif, Digunakan)." },
  { "en": "Apa Itu Wavelength Service (Layanan Gelombang)?", "id": "Layanan (Menyewa Panjang Gelombang DWDM)." },
  { "en": "Apa Itu Ethernet Private Line (EPL)?", "id": "Layanan Ethernet (Point-to-Point)." },
  { "en": "Apa Kepanjangan Dari EPL (Ethernet Private Line)?", "id": "Ethernet Private Line." },
  { "en": "Apa Itu Ethernet Virtual Private Line (EVPL)?", "id": "Layanan Ethernet (Multiplexing VLAN)." },
  { "en": "Apa Kepanjangan Dari EVPL (Ethernet Virtual Private Line)?", "id": "Ethernet Virtual Private Line." },
  { "en": "Apa Itu E-LAN (Ethernet LAN)?", "id": "Layanan Ethernet (Multipoint-to-Multipoint)." },
  { "en": "Apa Itu E-Tree (Ethernet Tree)?", "id": "Layanan Ethernet (Rooted-Multipoint)." },
  { "en": "Standar Layanan Ethernet (MEF)?", "id": "Metro Ethernet Forum (MEF)." },
  { "en": "Apa Kepanjangan Dari MEF (Metro Ethernet Forum)?", "id": "Metro Ethernet Forum." },
  { "en": "Apa Itu Customer Premises Equipment (CPE)?", "id": "Perangkat Di Lokasi Pelanggan." },
  { "en": "Apa Kepanjangan Dari CPE (Customer Premises Equipment)?", "id": "Customer Premises Equipment." },
  { "en": "Contoh CPE (Customer Premises Equipment)?", "id": "Modem, Router (Pelanggan)." },
  { "en": "Apa Itu Local Loop?", "id": "Koneksi (Demarc Ke Central Office)." },
  { "en": "Apa Itu Central Office (CO)?", "id": "Kantor Sentral Provider Telekomunikasi." },
  { "en": "Apa Itu Digital Subscriber Line (DSL)?", "id": "Internet (Lewat Saluran Telepon)." },
  { "en": "Apa Itu Asymmetric DSL (ADSL)?", "id": "DSL (Digital Subscriber Line) (Unduh Unggah Beda)." },
  { "en": "Apa Itu DSLAM (DSL Access Multiplexer)?", "id": "Perangkat DSL (Digital Subscriber Line) Provider." },
  { "en": "Apa Itu Cable Modem?", "id": "Modem (Internet Lewat Jaringan TV Kabel)." },
  { "en": "Apa Itu CMTS (Cable Modem Termination System)?", "id": "Perangkat Kabel Modem (Provider)." },
  { "en": "Apa Itu DOCSIS (Data Over Cable Service Interface Specification)?", "id": "Standar Internet Kabel Modem." },
  { "en": "Apa Itu Optical Network Terminal (ONT)?", "id": "Perangkat Pelanggan Jaringan PON." },
  { "en": "Apa Itu Optical Splitter?", "id": "Pembagi Sinyal Optik Pasif." },
  { "en": "Apa Itu GPON (Gigabit PON)?", "id": "Standar PON (Passive Optical Network) (Gigabit)." },
  { "en": "Apa Itu EPON (Ethernet PON)?", "id": "Standar PON (Passive Optical Network) (Ethernet)." },
  { "en": "Apa Itu T-Carrier?", "id": "Sistem Transmisi Digital (T1, T3)." },
  { "en": "Apa Itu Saluran T1?", "id": "Transmisi Digital (1.544 Mbps)." },
  { "en": "Apa Itu E-Carrier?", "id": "Sistem Transmisi Digital (E1, E3)." },
  { "en": "Apa Itu Saluran E1?", "id": "Transmisi Digital (2.048 Mbps)." },
  { "en": "Apa Itu DS0 (Digital Signal 0)?", "id": "Satu Saluran Suara (64 Kbps)." },
  { "en": "Berapa DS0 (Digital Signal 0) Dalam T1?", "id": "24 (Dua Puluh Empat)." },
  { "en": "Berapa DS0 (Digital Signal 0) Dalam E1?", "id": "32 (Tiga Puluh Dua)." },
  { "en": "Apa Itu Channelized T1/E1?", "id": "Saluran (Dibagi Menjadi DS0)." },
  { "en": "Apa Itu Unchannelized T1/E1?", "id": "Saluran (Satu Link Data Besar)." },
  { "en": "Apa Itu CSU/DSU (Channel Service Unit/Data Service Unit)?", "id": "Perangkat Terminasi (T1/E1)." },
  { "en": "Apa Itu Frame Relay?", "id": "Teknologi WAN (Wide Area Network) (Packet-Switching)." },
  { "en": "Apa Itu Permanent Virtual Circuit (PVC)?", "id": "Sirkuit Virtual Tetap (Frame Relay)." },
  { "en": "Apa Itu Switched Virtual Circuit (SVC)?", "id": "Sirkuit Virtual (Dibuat Saat Dibutuhkan)." },
  { "en": "Apa Itu Data Link Connection Identifier (DLCI)?", "id": "Identifikasi Sirkuit (Frame Relay)." },
  { "en": "Apa Itu Asynchronous Transfer Mode (ATM)?", "id": "Teknologi WAN (Wide Area Network) (Sel Tetap)." },
  { "en": "Apa Itu Sel (Cell) ATM (Asynchronous Transfer Mode)?", "id": "Paket Data Ukuran Tetap (53 Byte)." },
  { "en": "Apa Itu VPI (Virtual Path Identifier)?", "id": "Identifikasi Jalur Virtual (ATM)." },
  { "en": "Apa Itu VCI (Virtual Channel Identifier)?", "id": "Identifikasi Saluran Virtual (ATM)." },
  { "en": "Apa Itu Multiprotocol Label Switching (MPLS)?", "id": "Teknik Switching (Berbasis Label)." },
  { "en": "Apa Itu Label (MPLS)?", "id": "Identifier Pendek (Header MPLS)." },
  { "en": "Apa Itu Label Switched Path (LSP)?", "id": "Jalur Paket (MPLS)." },
  { "en": "Apa Itu Label Switching Router (LSR)?", "id": "Router Dalam Jaringan MPLS." },
  { "en": "Apa Itu Label Distribution Protocol (LDP)?", "id": "Protokol Distribusi Label (MPLS)." },
  { "en": "Apa Itu MPLS (Multiprotocol Label Switching) VPN (Virtual Private Network)?", "id": "VPN (Virtual Private Network) Lapis 3." },
    { "en": "Apa Itu Internet of Things (IoT)?", "id": "Jaringan Perangkat Fisik Terhubung Internet." },
  { "en": "Apa Itu Sensor (IoT)?", "id": "Perangkat Pengumpul Data Lingkungan." },
  { "en": "Apa Itu Aktuator (IoT)?", "id": "Perangkat Penggerak (Merespon Data)." },
  { "en": "Apa Itu Gateway (IoT)?", "id": "Perantara Perangkat IoT Ke Cloud." },
  { "en": "Apa Itu MQTT (Message Queuing Telemetry Transport)?", "id": "Protokol Pesan Ringan (IoT)." },
  { "en": "Apa Itu Broker (MQTT)?", "id": "Server Pusat (Manajemen Pesan MQTT)." },
  { "en": "Contoh Teknologi LPWAN (Low-Power Wide-Area Network)?", "id": "LoRaWAN (Long Range Wide Area Network), Sigfox." },
  { "en": "Apa Itu LoRaWAN (Long Range Wide Area Network)?", "id": "Protokol Jaringan (Di Atas LoRa)." },
  { "en": "Apa Itu Sigfox?", "id": "Teknologi LPWAN (Low-Power Wide-Area Network) (Ultra-Narrowband)." },
  { "en": "Apa Itu NB-IoT (Narrowband-IoT)?", "id": "Standar LPWAN (Low-Power Wide-Area Network) Seluler (LTE)." },
  { "en": "Apa Itu LTE-M (Long-Term Evolution for Machines)?", "id": "Standar LPWAN (Low-Power Wide-Area Network) Seluler." },
  { "en": "Apa Itu 6LoWPAN?", "id": "IPv6 (Internet Protocol Version 6) Di Jaringan IoT." },
  { "en": "Apa Itu Zigbee?", "id": "Standar Nirkabel (IEEE 802.15.4, Mesh)." },
  { "en": "Apa Itu Z-Wave?", "id": "Protokol Nirkabel (Rumah Pintar, Mesh)." },
  { "en": "Apa Itu Beacon?", "id": "Pemancar Sinyal BLE (Bluetooth Low Energy) (Lokasi)." },
  { "en": "Apa Itu Edge Computing?", "id": "Pemrosesan Data (Dekat Sumber Data)." },
  { "en": "Apa Itu Fog Computing?", "id": "Lapisan Komputasi (Antara Edge Cloud)." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Otomatisasi Industri (Logika)." },
  { "en": "Apa Itu Machine-to-Machine (M2M) Communication?", "id": "Komunikasi Langsung Antar Mesin." },
  { "en": "Apa Itu Artificial Intelligence (AI)?", "id": "Kecerdasan Buatan (Tiruan Manusia)." },
  { "en": "Apa Itu Toil (SRE)?", "id": "Pekerjaan Manual Berulang (Sia-sia)." },
  { "en": "Apa Itu Blameless Postmortem?", "id": "Analisis Insiden (Tanpa Menyalahkan)." },
  { "en": "Apa Itu Kabel Modem?", "id": "Modem (Internet Lewat Jaringan TV Kabel)." },
  { "en": "Apa Itu GPON (Gigabit Passive Optical Network)?", "id": "Standar PON (Passive Optical Network) Kecepatan Gigabit." },
  { "en": "Apa Itu EPON (Ethernet Passive Optical Network)?", "id": "Standar PON (Passive Optical Network) (IEEE)." },
  { "en": "Apa Itu T-Carrier?", "id": "Sistem Transmisi Digital (Amerika Utara)." },
  { "en": "Apa Itu Saluran T1?", "id": "Saluran Transmisi Digital (1.544 Mbps)." },
  { "en": "Apa Itu E-Carrier?", "id": "Sistem Transmisi Digital (Eropa)." },
  { "en": "Apa Itu Saluran E1?", "id": "Saluran Transmisi Digital (2.048 Mbps)." },
  { "en": "Apa Itu DSCP (Differentiated Services Code Point)?", "id": "Nilai Penanda Prioritas (Lapis 3)." },
  { "en": "Apa Itu Signaling System 7 (SS7)?", "id": "Protokol Pensinyalan Jaringan Telepon." },
  { "en": "Apa Itu Signal Transfer Point (STP)?", "id": "Router Sinyal Jaringan SS7 (Signaling System 7)." },
  { "en": "Apa Kepanjangan Dari STP (Signal Transfer Point)?", "id": "Signal Transfer Point." },
  { "en": "Apa Itu Service Control Point (SCP)?", "id": "Database Jaringan SS7 (Signaling System 7)." },
  { "en": "Apa Kepanjangan Dari SCP (Service Control Point)?", "id": "Service Control Point." },
  { "en": "Apa Itu Message Transfer Part (MTP)?", "id": "Bagian Transport Sinyal SS7 (Signaling System 7)." },
  { "en": "Apa Itu ISDN (Integrated Services Digital Network) User Part (ISUP)?", "id": "Bagian Kontrol Panggilan SS7 (Signaling System 7)." },
  { "en": "Apa Kepanjangan Dari ISUP (ISDN User Part)?", "id": "ISDN (Integrated Services Digital Network) User Part." },
  { "en": "Apa Itu Tandem Office (Sentral Tandem)?", "id": "Sentral Telepon (Penghubung Antar Sentral)." },
  { "en": "Apa Itu Synchronous Transport Signal (STS)?", "id": "Sinyal Dasar Standar SONET (Synchronous Optical Networking)." },
  { "en": "Apa Kepanjangan Dari STS (Synchronous Transport Signal)?", "id": "Synchronous Transport Signal." },
  { "en": "Apa Itu STS-1 (Synchronous Transport Signal 1)?", "id": "Kecepatan Dasar SONET (Optical Carrier 1)." },
  { "en": "Apa Itu Virtual Container (VC) (SDH)?", "id": "Kontainer Pembawa Sinyal (SDH)." },
  { "en": "Apa Itu Administrative Unit (AU) (SDH)?", "id": "Unit Administrasi (Pointer VC)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Route Aggregation?", "id": "Proses Meringkas Rute BGP (Border Gateway Protocol)." },
  { "en": "Apa Itu Atribut Aggregator (BGP)?", "id": "Atribut (Menunjukkan Siapa Meringkas Rute)." },
  { "en": "Apa Itu Atribut Atomic Aggregate (BGP)?", "id": "Atribut (Menandakan Rute Agregat)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Peer Group?", "id": "Mengelompokkan Peer (Kebijakan Sama)." },
  { "en": "Apa Itu Rute Flapping (Fluktuatif)?", "id": "Rute (Sering Naik Turun)." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Network Type Broadcast?", "id": "Jaringan OSPF (Open Shortest Path First) (Default Ethernet)." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Network Type Point-to-Point?", "id": "Jaringan OSPF (Open Shortest Path First) (Dua Router)." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Network Type Non-Broadcast Multi-Access (NBMA)?", "id": "Jaringan OSPF (Open Shortest Path First) (Frame Relay)." },
  { "en": "Apa Kepanjangan Dari NBMA (Non-Broadcast Multi-Access)?", "id": "Non-Broadcast Multi-Access." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Network Type Point-to-Multipoint?", "id": "Jaringan OSPF (Open Shortest Path First) (Hub-Spoke)." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Network Type Loopback?", "id": "Jaringan OSPF (Open Shortest Path First) (Antarmuka Loopback)." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Area?", "id": "Pembagian Jaringan OSPF (Open Shortest Path First)." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Area 0 (Backbone)?", "id": "Area Inti (Semua Area Terhubung)." },
  { "en": "Apa Itu Area Border Router (ABR)?", "id": "Router (Penghubung Antar Area OSPF)." },
  { "en": "Apa Itu Autonomous System Boundary Router (ASBR)?", "id": "Router (Penghubung OSPF Protokol Lain)." },
  { "en": "Apa Itu LSA (Link-State Advertisement) Tipe 1 (Router LSA)?", "id": "Informasi Router (Dalam Satu Area)." },
  { "en": "Apa Itu LSA (Link-State Advertisement) Tipe 2 (Network LSA)?", "id": "Informasi Jaringan (Dibuat DR)." },
  { "en": "Apa Itu LSA (Link-State Advertisement) Tipe 3 (Summary LSA)?", "id": "Informasi Rute (Antar Area, ABR)." },
  { "en": "Apa Itu LSA (Link-State Advertisement) Tipe 4 (ASBR Summary LSA)?", "id": "Informasi Lokasi ASBR (Autonomous System Boundary Router)." },
  { "en": "Apa Itu LSA (Link-State Advertisement) Tipe 5 (External LSA)?", "id": "Informasi Rute Eksternal (Dari ASBR)." },
  { "en": "Apa Itu LSA (Link-State Advertisement) Tipe 7 (NSSA External LSA)?", "id": "LSA (Link-State Advertisement) Eksternal (Area NSSA)." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Stub Area?", "id": "Area (Memblokir LSA Tipe 5)." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Totally Stubby Area?", "id": "Area (Memblokir LSA 3, 4, 5)." },
  { "en": "Apa Itu Not-So-Stubby Area (NSSA)?", "id": "Area Stub (Mengizinkan Rute Eksternal)." },
  { "en": "Apa Itu EIGRP (Enhanced Interior Gateway Routing Protocol) K-Values?", "id": "Konstanta (Penghitungan Metrik EIGRP)." },
  { "en": "K-Values Apa Default EIGRP (Enhanced Interior Gateway Routing Protocol)?", "id": "K1 (Bandwidth) Dan K3 (Delay)." },
  { "en": "Apa Itu EIGRP (Enhanced Interior Gateway Routing Protocol) Variance?", "id": "Fitur (Load Balancing (Jalur Beda Metrik))." },
  { "en": "Apa Itu EIGRP (Enhanced Interior Gateway Routing Protocol) Stub Router?", "id": "Router (Tidak Meneruskan Rute Transit)." },
  { "en": "Apa Itu Cloud Access Security Broker (CASB)?", "id": "Keamanan (Perantara Pengguna Layanan Cloud)." },
  { "en": "Apa Kepanjangan Dari CASB (Cloud Access Security Broker)?", "id": "Cloud Access Security Broker." },
  { "en": "Fungsi CASB (Cloud Access Security Broker)?", "id": "Visibilitas, Kepatuhan, Keamanan Data Cloud." },
  { "en": "Apa Itu Security Information and Event Management (SIEM)?", "id": "Manajemen Informasi Peristiwa Keamanan." },
  { "en": "Apa Itu Security Orchestration, Automation, and Response (SOAR)?", "id": "Otomatisasi Respon Insiden Keamanan." },
  { "en": "Apa Kepanjangan Dari SOAR (Security Orchestration, Automation, and Response)?", "id": "Security Orchestration, Automation, Response." },
  { "en": "Apa Itu Playbook (SOAR)?", "id": "Alur Kerja Otomatis (Respon Insiden)." },
  { "en": "Apa Itu NETCONF (Network Configuration Protocol)?", "id": "Protokol Konfigurasi Jaringan (XML)." },
  { "en": "Apa Itu RESTCONF (REST Configuration Protocol)?", "id": "Protokol Konfigurasi (Berbasis REST)." },
  { "en": "Apa Itu Simultaneous Authentication of Equals (SAE)?", "id": "Metode Autentikasi (WPA3 Personal)." },
  { "en": "Apa Kepanjangan Dari SAE (Simultaneous Authentication of Equals)?", "id": "Simultaneous Authentication of Equals." },
  { "en": "Apa Itu Opportunistic Wireless Encryption (OWE)?", "id": "Enkripsi Otomatis (Jaringan Wi-Fi Terbuka)." },
  { "en": "Apa Kepanjangan Dari OWE (Opportunistic Wireless Encryption)?", "id": "Opportunistic Wireless Encryption." },
  { "en": "Apa Itu Secure Real-time Transport Protocol (SRTP)?", "id": "Protokol Enkripsi Media (VoIP)." },
  { "en": "Apa Kepanjangan Dari SRTP (Secure Real-time Transport Protocol)?", "id": "Secure Real-time Transport Protocol." },
  { "en": "Apa Itu Advanced Message Queuing Protocol (AMQP)?", "id": "Protokol Antrian Pesan (Enterprise)." },
  { "en": "Apa Itu Data Distribution Service (DDS)?", "id": "Standar Komunikasi Real-Time (Pub/Sub)." },
  { "en": "Apa Kepanjangan Dari DDS (Data Distribution Service)?", "id": "Data Distribution Service." },
  { "en": "Apa Itu Fabric Extender (FEX) Cisco?", "id": "Ekstender Port (Remote Line Card)." },
  { "en": "Apa Kepanjangan Dari FEX (Fabric Extender)?", "id": "Fabric Extender." },
  { "en": "Apa Itu C-Band (Pita C) (Optik)?", "id": "Pita Panjang Gelombang (DWDM, 1530-1565 nm)." },
  { "en": "Apa Itu L-Band (Pita L) (Optik)?", "id": "Pita Panjang Gelombang (DWDM, 1565-1625 nm)." },
  { "en": "Apa Itu Transponder (Satelit) Bent Pipe?", "id": "Transponder (Hanya Menguatkan Memancarkan)." },
  { "en": "Apa Itu Transponder (Satelit) Regenerative?", "id": "Transponder (Demodulasi, Koreksi, Modulasi Ulang)." },
  { "en": "Apa Itu Trellis Code Modulation (TCM)?", "id": "Teknik Modulasi (Menggabungkan FEC)." },
  { "en": "Apa Kepanjangan Dari TCM (Trellis Code Modulation)?", "id": "Trellis Code Modulation." },
  { "en": "Apa Itu Turbo Code?", "id": "Kode Koreksi Error (Sangat Kuat)." },
  { "en": "Apa Itu Polar Code?", "id": "Kode Koreksi Error (Baru, 5G)." },
  { "en": "Apa Itu Ancaman Komputasi Kuantum?", "id": "Dapat Memecahkan Kriptografi (Asimetris)." },
  { "en": "Apa Itu Algoritma Shor?", "id": "Algoritma Kuantum (Memfaktorkan Bilangan)." },
  { "en": "Apa Itu Algoritma Grover?", "id": "Algoritma Kuantum (Pencarian Database)." },
  { "en": "Apa Itu Lattice-Based Cryptography?", "id": "Kandidat PQC (Post-Quantum Cryptography) (Berbasis Kisi)." },
  { "en": "Apa Itu Hash-Based Cryptography?", "id": "Kandidat PQC (Post-Quantum Cryptography) (Tanda Tangan Hash)." },
  { "en": "Apa Kepanjangan Dari ZTA (Zero Trust Architecture)?", "id": "Zero Trust Architecture." },
  { "en": "Prinsip Utama Zero Trust?", "id": "Verifikasi Eksplisit, Akses Least Privilege." },
  { "en": "Apa Itu Microsegmentation (Keamanan)?", "id": "Segmentasi Jaringan (Sangat Halus)." },
  { "en": "Tujuan Microsegmentation?", "id": "Membatasi Pergerakan Lateral Penyerang." },
  { "en": "Apa Itu Software-Defined Perimeter (SDP)?", "id": "Perimeter Keamanan (Berbasis Software)." },
  { "en": "Apa Kepanjangan Dari SDP (Software-Defined Perimeter)?", "id": "Software-Defined Perimeter." },
  { "en": "Apa Itu Secure Access Service Edge (SASE)?", "id": "Arsitektur Keamanan Cloud (WAN Edge)." },
  { "en": "Apa Kepanjangan Dari SASE (Secure Access Service Edge)?", "id": "Secure Access Service Edge." },
  { "en": "Komponen SASE (Secure Access Service Edge)?", "id": "SD-WAN (Software-Defined WAN) Keamanan Cloud." },
  { "en": "Apa Itu Cloud-Delivered Security?", "id": "Keamanan (Diberikan Dari Cloud)." },
  { "en": "Contoh Keamanan Cloud (SASE)?", "id": "FWaaS (Firewall as a Service), ZTNA." },
  { "en": "Apa Itu FWaaS (Firewall as a Service)?", "id": "Firewall (Sebagai Layanan Cloud)." },
  { "en": "Apa Kepanjangan Dari FWaaS (Firewall as a Service)?", "id": "Firewall as a Service." },
  { "en": "Apa Itu Zero Trust Network Access (ZTNA)?", "id": "Akses Jaringan (Model Zero Trust)." },
  { "en": "Apa Kepanjangan Dari ZTNA (Zero Trust Network Access)?", "id": "Zero Trust Network Access." },
  { "en": "Apa Itu Cloud Access Security Broker (CASB)?", "id": "Broker Keamanan (Antara Pengguna Cloud)." },
  { "en": "Apa Itu Secure Web Gateway (SWG)?", "id": "Gateway (Filter Keamanan Lalu Lintas Web)." },
  { "en": "Apa Kepanjangan Dari SWG (Secure Web Gateway)?", "id": "Secure Web Gateway." },
  { "en": "Apa Itu Data Loss Prevention (DLP)?", "id": "Sistem (Mencegah Kebocoran Data Sensitif)." },
  { "en": "Bagaimana DLP (Data Loss Prevention) Bekerja?", "id": "Menganalisis Konten Data (Mencari Pola)." },
  { "en": "Apa Itu DLP (Data Loss Prevention) In-Motion?", "id": "Menganalisis Data (Bergerak Di Jaringan)." },
  { "en": "Apa Itu DLP (Data Loss Prevention) At-Rest?", "id": "Menganalisis Data (Tersimpan Di Penyimpanan)." },
  { "en": "Apa Itu DLP (Data Loss Prevention) In-Use?", "id": "Menganalisis Data (Digunakan Di Endpoint)." },
  { "en": "Apa Itu Endpoint Security?", "id": "Keamanan (Perangkat Akhir Pengguna)." },
  { "en": "Contoh Endpoint Security?", "id": "Antivirus, Firewall Host, HIDS." },
  { "en": "Apa Itu Antivirus (AV)?", "id": "Perangkat Lunak (Mendeteksi Virus)." },
  { "en": "Apa Itu Anti-Malware?", "id": "Perangkat Lunak (Melawan Beragam Malware)." },
  { "en": "Apa Itu Endpoint Detection and Response (EDR)?", "id": "Deteksi Respon Ancaman (Endpoint)." },
  { "en": "Apa Kepanjangan Dari EDR (Endpoint Detection and Response)?", "id": "Endpoint Detection and Response." },
  { "en": "Apa Itu Extended Detection and Response (XDR)?", "id": "EDR (Endpoint Detection and Response) (Terintegrasi)." },
  { "en": "Apa Kepanjangan Dari XDR (Extended Detection and Response)?", "id": "Extended Detection and Response." },
  { "en": "Apa Itu Host-Based Firewall?", "id": "Firewall (Berjalan Di Perangkat Host)." },
  { "en": "Apa Itu Host-Based IDS (HIDS)?", "id": "IDS (Intrusion Detection System) (Di Host)." },
  { "en": "Apa Itu Full Disk Encryption (FDE)?", "id": "Enkripsi (Seluruh Disk Penyimpanan)." },
  { "en": "Contoh FDE (Full Disk Encryption)?", "id": "BitLocker (Windows), FileVault (Mac)." },
  { "en": "Apa Itu Trusted Platform Module (TPM)?", "id": "Chip Kriptografi (Penyimpan Kunci Aman)." },
  { "en": "Apa Itu Hardware Security Module (HSM)?", "id": "Perangkat Keras (Manajemen Kunci Kripto)." },
  { "en": "Apa Beda TPM (Trusted Platform Module) Dan HSM (Hardware Security Module)?", "id": "TPM (Trusted Platform Module) Klien, HSM (Hardware Security Module) Server." },
  { "en": "Apa Itu Secure Boot?", "id": "Fitur UEFI (Mencegah Boot Malware)." },
  { "en": "Apa Itu Unified Extensible Firmware Interface (UEFI)?", "id": "Firmware Modern (Pengganti BIOS)." },
  { "en": "Apa Itu Basic Input/Output System (BIOS)?", "id": "Firmware Lama (Booting Komputer)." },
  { "en": "Apa Itu Sandboxing (Keamanan)?", "id": "Lingkungan Terisolasi (Menjalankan Program)." },
  { "en": "Tujuan Sandboxing?", "id": "Menganalisis Malware (Tanpa Merusak Sistem)." },
  { "en": "Apa Itu Application Whitelisting?", "id": "Hanya Mengizinkan Aplikasi (Terdaftar)." },
  { "en": "Apa Itu Application Blacklisting?", "id": "Memblokir Aplikasi (Terlarang)." },
  { "en": "Mana Lebih Aman, Whitelisting Atau Blacklisting?", "id": "Whitelisting (Default Deny)." },
  { "en": "Apa Itu Bring Your Own Device (BYOD)?", "id": "Kebijakan (Perangkat Pribadi Bekerja)." },
  { "en": "Apa Itu Remote Wipe (MDM)?", "id": "Menghapus Data Perangkat (Jarak Jauh)." },
  { "en": "Apa Itu Geofencing?", "id": "Batas Geografis Virtual (Aturan Lokasi)." },
  { "en": "Apa Itu Geolocation?", "id": "Pelacakan Lokasi Geografis Perangkat." },
  { "en": "Apa Itu Rooting (Android)?", "id": "Mendapatkan Akses Root (Sistem Android)." },
  { "en": "Apa Itu Sideloading (Aplikasi)?", "id": "Instalasi Aplikasi (Luar Toko Resmi)." },
  { "en": "Apa Itu Mobile Application Management (MAM)?", "id": "Pengelolaan Keamanan (Aplikasi Seluler)." },
  { "en": "Apa Beda MDM (Mobile Device Management) Dan MAM (Mobile Application Management)?", "id": "MDM (Mobile Device Management) Perangkat, MAM Aplikasi." },
  { "en": "Siapa Mengembangkan Cyber Kill Chain?", "id": "Lockheed Martin." },
  { "en": "Sebutkan Tahap Pertama Kill Chain?", "id": "Reconnaissance (Pengintaian)." },
  { "en": "Sebutkan Tahap Kedua Kill Chain?", "id": "Weaponization (Pembuatan Senjata)." },
  { "en": "Sebutkan Tahap Ketiga Kill Chain?", "id": "Delivery (Pengiriman Senjata)." },
  { "en": "Sebutkan Tahap Keempat Kill Chain?", "id": "Exploitation (Eksploitasi Kerentanan)." },
  { "en": "Sebutkan Tahap Kelima Kill Chain?", "id": "Installation (Instalasi Malware)." },
  { "en": "Sebutkan Tahap Keenam Kill Chain?", "id": "Command And Control (C2)." },
  { "en": "Sebutkan Tahap Ketujuh Kill Chain?", "id": "Actions on Objectives (Aksi Tujuan)." },
  { "en": "Apa Itu Reconnaissance (Pengintaian)?", "id": "Mengumpulkan Informasi Target (Pasif Aktif)." },
  { "en": "Apa Itu Command And Control (C2) Server?", "id": "Server Pusat (Kontrol Malware Botnet)." },
  { "en": "Apa Kepanjangan Dari C2 (Command And Control)?", "id": "Command And Control." },
  { "en": "Apa Itu MITRE ATT&CK Framework?", "id": "Database Taktik Teknik Serangan Siber." },
  { "en": "Apa Kepanjangan Dari ATT&CK (Adversarial Tactics, Techniques, and Common Knowledge)?", "id": "Adversarial Tactics, Techniques, Common Knowledge." },
  { "en": "Apa Itu Tactic (ATT&CK)?", "id": "Tujuan Taktis Penyerang (Contoh: Akses)." },
  { "en": "Apa Itu Technique (ATT&CK)?", "id": "Cara Mencapai Tujuan Taktis." },
  { "en": "Apa Itu Diamond Model (Intrusion Analysis)?", "id": "Model Analisis Insiden (Empat Poin)." },
  { "en": "Sebutkan Empat Poin Diamond Model?", "id": "Adversary, Infrastructure, Capability, Victim." },
  { "en": "Apa Itu Threat Intelligence (Intelijen Ancaman)?", "id": "Informasi Kontekstual Mengenai Ancaman." },
  { "en": "Apa Itu Indicator of Compromise (IOC)?", "id": "Indikator Bukti (Insiden Telah Terjadi)." },
  { "en": "Contoh IOC (Indicator of Compromise)?", "id": "Hash Malware, Alamat IP (Internet Protocol) C2." },
  { "en": "Apa Itu Indicator of Attack (IOA)?", "id": "Indikator (Serangan Sedang Berlangsung)." },
  { "en": "Apa Itu TTP (Tactics, Techniques, and Procedures)?", "id": "Metodologi Perilaku Penyerang." },
  { "en": "Apa Itu Threat Hunting (Perburuan Ancaman)?", "id": "Proses Proaktif (Mencari Ancaman Tersembunyi)." },
  { "en": "Fungsi Utama SIEM (Security Information and Event Management)?", "id": "Agregasi Korelasi Log Keamanan." },
  { "en": "Apa Itu Korelasi Log (SIEM)?", "id": "Menghubungkan Peristiwa Log (Mencari Pola)." },
  { "en": "Apa Itu User and Entity Behavior Analytics (UEBA)?", "id": "Analisis Perilaku (Deteksi Anomali)." },
  { "en": "Apa Kepanjangan Dari UEBA (User and Entity Behavior Analytics)?", "id": "User and Entity Behavior Analytics." },
  { "en": "Tahap Pertama IR (Incident Response)?", "id": "Preparation (Persiapan)." },
  { "en": "Tahap Kedua IR (Incident Response)?", "id": "Identification (Identifikasi)." },
  { "en": "Tahap Ketiga IR (Incident Response)?", "id": "Containment (Pembatasan)." },
  { "en": "Tahap Keempat IR (Incident Response)?", "id": "Eradication (Pemberantasan)." },
  { "en": "Tahap Kelima IR (Incident Response)?", "id": "Recovery (Pemulihan)." },
  { "en": "Tahap Keenam IR (Incident Response)?", "id": "Lessons Learned (Pembelajaran)." },
  { "en": "Apa Itu Computer Security Incident Response Team (CSIRT)?", "id": "Tim Tanggap Insiden Keamanan Komputer." },
  { "en": "Apa Kepanjangan Dari CSIRT (Computer Security Incident Response Team)?", "id": "Computer Security Incident Response Team." },
  { "en": "Apa Itu Product Security Incident Response Team (PSIRT)?", "id": "Tim (Insiden Keamanan Produk Vendor)." },
  { "en": "Apa Kepanjangan Dari PSIRT (Product Security Incident Response Team)?", "id": "Product Security Incident Response Team." },
  { "en": "Apa Kepanjangan Dari TPM (Trusted Platform Module)?", "id":"Trusted Platform Module." },
    { "en": "Apa Itu Forensik Digital?", "id": "Ilmu Investigasi Bukti Digital." },
  { "en": "Apa Itu GPON (Gigabit Passive Optical Network)?", "id": "Standar PON (Passive Optical Network) (Gigabit)." },
  { "en": "Apa Itu EPON (Ethernet Passive Optical Network)?", "id": "Standar PON (Passive Optical Network) (Ethernet)." },
  { "en": "Apa Itu Symbol Error Rate (SER)?", "id": "Rasio Perbandingan Simbol Error Simbol Total." },
  { "en": "Apa Kepanjangan Dari SER (Symbol Error Rate)?", "id": "Symbol Error Rate." },
  { "en": "Apa Hubungan BER (Bit Error Rate) Dan SNR (Signal-to-Noise Ratio)?", "id": "SNR (Signal-to-Noise Ratio) Tinggi, BER Rendah." },
  { "en": "Apa Itu Eb/N0 (Energy per bit to noise density)?", "id": "Rasio Energi Per Bit Kebisingan." },
  { "en": "Apa Kepanjangan Dari Eb/N0 (Energy per bit to noise density)?", "id": "Energy per bit to noise density." },
  { "en": "Apa Itu Phase Noise (Derau Fasa)?", "id": "Fluktuasi Fasa Sinyal Osilator." },
  { "en": "Apa Dampak Phase Noise?", "id": "Merusak Kinerja Modulasi Digital." },
  { "en": "Apa Itu Jitter (Sinyal Digital)?", "id": "Variasi Waktu Tiba Sinyal Digital." },
  { "en": "Apa Itu Diagram Mata (Eye Diagram)?", "id": "Tampilan Visual Kualitas Sinyal Digital." },
  { "en": "Apa Yang Diukur Eye Diagram?", "id": "Jitter, Noise, Distorsi Sinyal." },
  { "en": "Apa Itu Bukaan Mata (Eye Opening)?", "id": "Indikator Kualitas Sinyal (Mata Terbuka)." },
  { "en": "Apa Itu Link Budget (Anggaran Taut)?", "id": "Perhitungan Total Penguatan Kehilangan Sinyal." },
  { "en": "Tujuan Link Budget?", "id": "Memastikan Sinyal Diterima Dengan Baik." },
  { "en": "Apa Itu Effective Isotropic Radiated Power (EIRP)?", "id": "Daya Pancar Efektif Antena Isotropik." },
  { "en": "Apa Kepanjangan Dari EIRP (Effective Isotropic Radiated Power)?", "id": "Effective Isotropic Radiated Power." },
  { "en": "Apa Itu Carrier-to-Noise Ratio (C/N)?", "id": "Rasio Daya Sinyal Pembawa Noise." },
  { "en": "Apa Kepanjangan Dari C/N (Carrier-to-Noise Ratio)?", "id": "Carrier-to-Noise Ratio." },
  { "en": "Apa Itu G/T (G over T)?", "id": "Ukuran Kinerja Stasiun Bumi Penerima." },
  { "en": "Apa Kepanjangan Dari G/T (G over T)?", "id": "Gain-to-Noise-Temperature." },
  { "en": "Apa Itu Noise Temperature (Suhu Derau)?", "id": "Representasi Derau (Dalam Kelvin)." },
  { "en": "Apa Itu Low Noise Amplifier (LNA)?", "id": "Penguat (Sinyal Sangat Lemah, Noise Rendah)." },
  { "en": "Apa Kepanjangan Dari LNA (Low Noise Amplifier)?", "id": "Low Noise Amplifier." },
  { "en": "Dimana LNA (Low Noise Amplifier) Ditempatkan?", "id": "Dekat Antena Penerima (Stasiun Bumi)." },
  { "en": "Apa Itu Low Noise Block (LNB) (Satelit)?", "id": "Perangkat Penerima (Blok Konverter LNA)." },
  { "en": "Apa Kepanjangan Dari LNB (Low Noise Block)?", "id": "Low Noise Block." },
  { "en": "Apa Fungsi LNB (Low Noise Block)?", "id": "Menguatkan Mengubah Frekuensi Turun." },
  { "en": "Apa Itu Block Up-Converter (BUC) (Satelit)?", "id": "Perangkat Pemancar (Blok Konverter Naik)." },
  { "en": "Apa Kepanjangan Dari BUC (Block Up-Converter)?", "id": "Block Up-Converter." },
  { "en": "Apa Fungsi BUC (Block Up-Converter)?", "id": "Mengubah Frekuensi Naik (Sebelum HPA)." },
  { "en": "Apa Itu High Power Amplifier (HPA)?", "id": "Penguat Daya Tinggi (Pemancar Satelit)." },
  { "en": "Apa Kepanjangan Dari HPA (High Power Amplifier)?", "id": "High Power Amplifier." },
  { "en": "Apa Itu Solid State Power Amplifier (SSPA)?", "id": "HPA (High Power Amplifier) Berbasis Solid-State." },
  { "en": "Apa Kepanjangan Dari SSPA (Solid State Power Amplifier)?", "id": "Solid State Power Amplifier." },
  { "en": "Apa Itu Orbit Molniya?", "id": "Orbit Satelit Sangat Elips (Rusia)." },
  { "en": "Apa Itu Orbit Tundra?", "id": "Orbit Satelit Elips (Mirip Molniya)." },
  { "en": "Apa Itu Orbit Polar?", "id": "Orbit Satelit (Melewati Kutub Utara Selatan)." },
  { "en": "Apa Itu Orbit Sun-Synchronous (SSO)?", "id": "Orbit (Satelit Lewati Titik Sama Jam Sama)." },
  { "en": "Apa Kepanjangan Dari SSO (Sun-Synchronous Orbit)?", "id": "Sun-Synchronous Orbit." },
  { "en": "Apa Itu Antenna Efficiency (Efisiensi Antena)?", "id": "Rasio Daya Dipancarkan Daya Masukan." },
  { "en": "Apa Itu Voltage Standing Wave Ratio (VSWR)?", "id": "Rasio Gelombang Berdiri (Kecocokan Impedansi)." },
  { "en": "Apa Kepanjangan Dari VSWR (Voltage Standing Wave Ratio)?", "id": "Voltage Standing Wave Ratio." },
  { "en": "Apa Nilai VSWR (Voltage Standing Wave Ratio) Ideal?", "id": "1:1 (Satu Banding Satu)." },
  { "en": "Apa Itu Smith Chart?", "id": "Grafik (Analisis Impedansi Saluran Transmisi)." },
  { "en": "Apa Itu Near Field (Antena)?", "id": "Area Sangat Dekat Dengan Antena." },
  { "en": "Apa Itu Far Field (Antena)?", "id": "Area Jauh (Pola Radiasi Stabil)." },
  { "en": "Apa Itu Radio Horizon (Cakrawala Radio)?", "id": "Jarak Maksimal Propagasi Line-of-Sight." },
  { "en": "Apa Itu Optical Spectrum Analyzer (OSA)?", "id": "Alat Ukur (Spektrum Sinyal Optik)." },
  { "en": "Apa Kepanjangan Dari OSA (Optical Spectrum Analyzer)?", "id": "Optical Spectrum Analyzer." },
  { "en": "Apa Itu Bit Error Rate Tester (BERT)?", "id": "Alat Ukur (Pengujian BER)." },
  { "en": "Apa Kepanjangan Dari BERT (Bit Error Rate Tester)?", "id": "Bit Error Rate Tester." },
  { "en": "Apa Itu Pita O (Optical Band)?", "id": "Pita Optik (Original, 1260-1360 nm)." },
  { "en": "Apa Itu Pita E (Optical Band)?", "id": "Pita Optik (Extended, 1360-1460 nm)." },
  { "en": "Apa Itu Pita S (Optical Band)?", "id": "Pita Optik (Short, 1460-1530 nm)." },
  { "en": "Apa Itu Pita C (Optical Band)?", "id": "Pita Optik (Conventional, 1530-1565 nm)." },
  { "en": "Apa Itu Pita L (Optical Band)?", "id": "Pita Optik (Long, 1565-1625 nm)." },
  { "en": "Apa Itu Pita U (Optical Band)?", "id": "Pita Optik (Ultralong, 1625-1675 nm)." },
  { "en": "Pita Optik Apa Paling Umum (DWDM)?", "id": "Pita C (C-Band) Dan L (L-Band)." },
  { "en": "Apa Itu Chromatic Dispersion (Dispersi Kromatik)?", "id": "Pelebaran Pulsa (Beda Kecepatan Warna)." },
  { "en": "Apa Itu Polarization Mode Dispersion (PMD)?", "id": "Pelebaran Pulsa (Beda Polarisasi)." },
  { "en": "Apa Itu Dispersion Compensation Fiber (DCF)?", "id": "Serat (Mengkompensasi Dispersi Kromatik)." },
  { "en": "Apa Kepanjangan Dari DCF (Dispersion Compensation Fiber)?", "id": "Dispersion Compensation Fiber." },
  { "en": "Apa Itu Fiber Bragg Grating (FBG)?", "id": "Filter Optik (Berbasis Serat Optik)." },
  { "en": "Apa Kepanjangan Dari FBG (Fiber Bragg Grating)?", "id": "Fiber Bragg Grating." },
  { "en": "Apa Itu Wavelength Selective Switch (WSS)?", "id": "Switch Optik (Berbasis Panjang Gelombang)." },
  { "en": "Apa Kepanjangan Dari WSS (Wavelength Selective Switch)?", "id": "Wavelength Selective Switch." },
  { "en": "Apa Itu Add-Drop Multiplexer (ADM)?", "id": "Perangkat (Menambah Mengambil Saluran TDM)." },
  { "en": "Apa Kepanjangan Dari ADM (Add-Drop Multiplexer)?", "id": "Add-Drop Multiplexer." },
  { "en": "Apa Itu Optical Add-Drop Multiplexer (OADM)?", "id": "ADM (Add-Drop Multiplexer) (Jaringan Optik WDM)." },
  { "en": "Apa Itu Reconfigurable OADM (ROADM)?", "id": "OADM (Optical Add-Drop Multiplexer) (Dapat Diprogram)." },
  { "en": "Apa Itu Soliton (Optik)?", "id": "Pulsa Optik (Menjaga Bentuk Jarak Jauh)." },
  { "en": "Apa Itu Amplifier Spontaneous Emission (ASE)?", "id": "Noise (Dihasilkan Penguat Optik)." },
  { "en": "Apa Kepanjangan Dari ASE (Amplifier Spontaneous Emission)?", "id": "Amplifier Spontaneous Emission." },
  { "en": "Apa Itu Optical Signal-to-Noise Ratio (OSNR)?", "id": "SNR (Signal-to-Noise Ratio) Di Domain Optik." },
  { "en": "Apa Kepanjangan Dari OSNR (Optical Signal-to-Noise Ratio)?", "id": "Optical Signal-to-Noise Ratio." },
  { "en": "Apa Itu Four-Wave Mixing (FWM)?", "id": "Efek Non-Linear (Interferensi Optik)." },
  { "en": "Apa Kepanjangan Dari FWM (Four-Wave Mixing)?", "id": "Four-Wave Mixing." },
  { "en": "Apa Itu Self-Phase Modulation (SPM)?", "id": "Efek Non-Linear (Perubahan Fasa Sendiri)." },
  { "en": "Apa Kepanjangan Dari SPM (Self-Phase Modulation)?", "id": "Self-Phase Modulation." },
  { "en": "Apa Itu Cross-Phase Modulation (XPM)?", "id": "Efek Non-Linear (Perubahan Fasa Antar Saluran)." },
  { "en": "Apa Kepanjangan Dari XPM (Cross-Phase Modulation)?", "id": "Cross-Phase Modulation." },
  { "en": "Apa Itu Efek Non-Linear (Serat Optik)?", "id": "Distorsi Sinyal (Daya Optik Tinggi)." },
  { "en": "Apa Itu Jaringan Non-Terrestrial (NTN)?", "id": "Jaringan (Satelit, Drone, HAPS)." },
  { "en": "Apa Itu Komunikasi Kuantum?", "id": "Komunikasi Berbasis Prinsip Kuantum." },
  { "en": "Apa Itu Elastic Network Interface (ENI)?", "id": "Antarmuka Jaringan Virtual (AWS)." },
  { "en": "Apa Kepanjangan Dari ENI (Elastic Network Interface)?", "id": "Elastic Network Interface." },
  { "en": "Apa Itu Load Balancer?", "id": "Perangkat Pendistribusi Beban Trafik." },
  { "en": "Apa Itu Application Load Balancer (ALB)?", "id": "Load Balancer Lapis 7 (Aplikasi)." },
  { "en": "Apa Kepanjangan Dari ALB (Application Load Balancer)?", "id": "Application Load Balancer." },
  { "en": "Apa Itu Network Load Balancer (NLB)?", "id": "Load Balancer Lapis 4 (Jaringan)." },
  { "en": "Apa Kepanjangan Dari NLB (Network Load Balancer)?", "id": "Network Load Balancer." },
  { "en": "Apa Itu Classic Load Balancer (CLB)?", "id": "Load Balancer Lama (AWS)." },
  { "en": "Apa Kepanjangan Dari CLB (Classic Load Balancer)?", "id": "Classic Load Balancer." },
  { "en": "Apa Itu Auto Scaling?", "id": "Penyesuaian Kapasitas Sumber Daya Otomatis." },
  { "en": "Apa Itu Horizontal Scaling (Scale Out)?", "id": "Menambah Mengurangi Jumlah Instansi." },
  { "en": "Apa Itu Vertical Scaling (Scale Up)?", "id": "Menambah Mengurangi Ukuran Instansi." },
  { "en": "Apa Itu Amazon S3 (Simple Storage Service)?", "id": "Layanan Penyimpanan Objek (AWS)." },
  { "en": "Apa Kepanjangan Dari S3 (Simple Storage Service)?", "id": "Simple Storage Service." },
  { "en": "Apa Itu Object Storage?", "id": "Penyimpanan Data (Sebagai Objek)." },
  { "en": "Apa Itu Block Storage?", "id": "Penyimpanan Data (Sebagai Blok Disk)." },
  { "en": "Apa Itu File Storage?", "id": "Penyimpanan Data (Sistem File Hirarkis)." },
  { "en": "Apa Itu Amazon EBS (Elastic Block Store)?", "id": "Layanan Block Storage (AWS EC2)." },
  { "en": "Apa Kepanjangan Dari EBS (Elastic Block Store)?", "id": "Elastic Block Store." },
  { "en": "Apa Itu Amazon EFS (Elastic File System)?", "id": "Layanan File Storage (AWS NFS)." },
  { "en": "Apa Kepanjangan Dari EFS (Elastic File System)?", "id": "Elastic File System." },
  { "en": "Apa Itu Amazon RDS (Relational Database Service)?", "id": "Layanan Database Relasional (Terkelola)." },
  { "en": "Apa Kepanjangan Dari RDS (Relational Database Service)?", "id": "Relational Database Service." },
  { "en": "Apa Itu Amazon DynamoDB?", "id": "Layanan Database NoSQL (Terkelola)." },
  { "en": "Apa Itu Serverless (Tanpa Server)?", "id": "Model Cloud (Tanpa Mengelola Server)." },
  { "en": "Apa Itu AWS (Amazon Web Services) Lambda?", "id": "Layanan Komputasi Serverless (FaaS)." },
  { "en": "Apa Itu Function as a Service (FaaS)?", "id": "Model Eksekusi Kode Serverless." },
  { "en": "Apa Itu AWS (Amazon Web Services) Fargate?", "id": "Mesin Komputasi Serverless (Kontainer)." },
  { "en": "Apa Itu Amazon EC2 (Elastic Compute Cloud)?", "id": "Layanan Server Virtual (IaaS)." },
  { "en": "Apa Kepanjangan Dari EC2 (Elastic Compute Cloud)?", "id": "Elastic Compute Cloud." },
  { "en": "Apa Itu Amazon Machine Image (AMI)?", "id": "Templat (Membuat Instansi EC2)." },
  { "en": "Apa Kepanjangan Dari AMI (Amazon Machine Image)?", "id": "Amazon Machine Image." },
  { "en": "Apa Itu Instance (Instansi) (Cloud)?", "id": "Server Virtual (Yang Berjalan)." },
  { "en": "Apa Itu Microsoft Azure?", "id": "Platform Cloud Computing Microsoft." },
  { "en": "Apa Itu Azure Virtual Network (VNet)?", "id": "Jaringan Virtual (Mirip VPC AWS)." },
  { "en": "Apa Itu Azure Virtual Machine (VM)?", "id": "Server Virtual (Mirip EC2 AWS)." },
  { "en": "Apa Itu Azure Blob Storage?", "id": "Penyimpanan Objek (Mirip S3 AWS)." },
  { "en": "Apa Itu Azure SQL (Structured Query Language) Database?", "id": "Layanan Database Relasional (Azure)." },
  { "en": "Apa Itu Azure Functions?", "id": "Layanan Serverless (Mirip Lambda AWS)." },
  { "en": "Apa Itu Google Cloud Platform (GCP)?", "id": "Platform Cloud Computing Google." },
  { "en": "Apa Itu Google Compute Engine (GCE)?", "id": "Layanan Server Virtual (GCP)." },
  { "en": "Apa Kepanjangan Dari GCE (Google Compute Engine)?", "id": "Google Compute Engine." },
  { "en": "Apa Itu Google Cloud Storage?", "id": "Layanan Penyimpanan Objek (GCP)." },
  { "en": "Apa Itu Google Cloud SQL (Structured Query Language)?", "id": "Layanan Database Relasional (GCP)." },
  { "en": "Apa Itu Google Cloud Functions?", "id": "Layanan Serverless (GCP)." },
  { "en": "Apa Itu OpenStack?", "id": "Platform Perangkat Lunak (Cloud Privat)." },
  { "en": "Apa Itu Komponen Nova (OpenStack)?", "id": "Komponen Komputasi (Manajemen VM)." },
  { "en": "Apa Itu Komponen Swift (OpenStack)?", "id": "Komponen Penyimpanan Objek." },
  { "en": "Apa Itu Komponen Neutron (OpenStack)?", "id": "Komponen Jaringan (Virtualisasi Jaringan)." },
  { "en": "Apa Itu Komponen Cinder (OpenStack)?", "id": "Komponen Penyimpanan Blok." },
  { "en": "Apa Itu Komponen Keystone (OpenStack)?", "id": "Komponen Layanan Identitas (Autentikasi)." },
  { "en": "Apa Itu Komponen Glance (OpenStack)?", "id": "Komponen Layanan Image (Templat VM)." },
  { "en": "Apa Itu OpenFlow?", "id": "Protokol Komunikasi (Controller Switch SDN)." },
  { "en": "Contoh SDN (Software-Defined Networking) Controller?", "id": "OpenDaylight, ONOS." },
  { "en": "Apa Itu OpenDaylight (ODL)?", "id": "Platform Controller SDN (Open Source)." },
  { "en": "Apa Kepanjangan Dari ODL (OpenDaylight)?", "id": "OpenDaylight." },
  { "en": "Apa Itu ONOS (Open Network Operating System)?", "id": "Controller SDN (Open Source, Provider)." },
  { "en": "Apa Kepanjangan Dari ONOS (Open Network Operating System)?", "id": "Open Network Operating System." },
  { "en": "Apa Itu Management and Orchestration (MANO)?", "id": "Manajemen Orkestrasi (NFV)." },
  { "en": "Apa Kepanjangan Dari MANO (Management and Orchestration)?", "id": "Management and Orchestration." },
  { "en": "Apa Itu VNF (Virtual Network Function) Manager (VNFM)?", "id": "Manajer Siklus Hidup VNF (Virtual Network Function)." },
  { "en": "Apa Itu Virtualized Infrastructure Manager (VIM)?", "id": "Manajer Infrastruktur Virtual (OpenStack)." },
  { "en": "Apa Itu NFV (Network Functions Virtualization) Orchestrator (NFVO)?", "id": "Orkestrator Layanan Jaringan (NFV)." },
  { "en": "Apa Itu Service Chaining (Rantai Layanan)?", "id": "Mengarahkan Trafik (Melalui VNF Berurutan)." },
  { "en": "Apa Itu ETSI (European Telecommunications Standards Institute)?", "id": "Badan Standarisasi Eropa (NFV)." },
  { "en": "Apa Kepanjangan Dari ETSI (European Telecommunications Standards Institute)?", "id": "European Telecommunications Standards Institute." },
  { "en": "Apa Itu OPNFV (Open Platform for NFV)?", "id": "Proyek Open Source (Integrasi NFV)." },
  { "en": "Apa Kepanjangan Dari OPNFV (Open Platform for NFV)?", "id": "Open Platform for NFV." },
  { "en": "Apa Itu Software-Defined WAN (SD-WAN)?", "id": "Aplikasi SDN (Software-Defined Networking) WAN." },
  { "en": "Apa Itu Application-Aware Routing?", "id": "Routing (Memilih Jalur Berdasarkan Aplikasi)." },
  { "en": "Apa Itu HIDS (Host-based IDS)?", "id": "IDS (Intrusion Detection System) (Di Host)." },
    { "en": "Apa Itu Fading Cepat (Fast Fading)?", "id": "Fluktuasi Sinyal (Periode Simbol Pendek)." },
  { "en": "Apa Itu Fading Lambat (Slow Fading)?", "id": "Fluktuasi Sinyal (Periode Simbol Panjang)." },
  { "en": "Apa Itu Model Kanal (Channel Model)?", "id": "Representasi Matematis (Karakteristik Kanal)." },
  { "en": "Apa Itu Kanal Rayleigh Fading?", "id": "Model Kanal (Tanpa Sinyal Dominan LOS)." },
  { "en": "Apa Itu Kanal Rician Fading?", "id": "Model Kanal (Ada Sinyal Dominan LOS)." },
  { "en": "Apa Itu Faktor K (Rician)?", "id": "Rasio Daya Sinyal Dominan Pantulan." },
  { "en": "Apa Itu Doppler Spread?", "id": "Pelebaran Spektrum (Akibat Gerakan)." },
  { "en": "Apa Itu Coherence Time?", "id": "Durasi Waktu Kanal Dianggap Stabil." },
  { "en": "Apa Itu Coherence Bandwidth?", "id": "Rentang Frekuensi (Mengalami Fading Serupa)." },
  { "en": "Apa Itu Frequency Selective Fading?", "id": "Fading (Berbeda Di Frekuensi Berbeda)." },
  { "en": "Apa Itu Flat Fading?", "id": "Fading (Seragam Di Seluruh Frekuensi)." },
  { "en": "Apa Itu Teknik Diversity?", "id": "Teknik (Mengatasi Fading, Redundansi)." },
  { "en": "Apa Itu Space Diversity (Keanekaragaman Ruang)?", "id": "Diversity (Menggunakan Antena Terpisah)." },
  { "en": "Apa Itu Frequency Diversity (Keanekaragaman Frekuensi)?", "id": "Diversity (Menggunakan Frekuensi Berbeda)." },
  { "en": "Apa Itu Time Diversity (Keanekaragaman Waktu)?", "id": "Diversity (Mengirim Ulang Waktu Berbeda)." },
  { "en": "Apa Itu Polarization Diversity (Keanekaragaman Polarisasi)?", "id": "Diversity (Menggunakan Polarisasi Berbeda)." },
  { "en": "Apa Itu Selection Combining (SC)?", "id": "Teknik Diversity (Memilih Sinyal Terbaik)." },
  { "en": "Apa Itu Maximal-Ratio Combining (MRC)?", "id": "Teknik Diversity (Menggabungkan Sinyal Optimal)." },
  { "en": "Apa Itu Equal-Gain Combining (EGC)?", "id": "Teknik Diversity (Menggabungkan Sinyal Sama)." },
  { "en": "Apa Itu Rake Receiver?", "id": "Penerima (Menggabungkan Sinyal Multipath)." },
  { "en": "Apa Itu Inter-Symbol Interference (ISI)?", "id": "Interferensi (Antar Simbol Berurutan)." },
  { "en": "Penyebab ISI (Inter-Symbol Interference)?", "id": "Multipath Fading, Bandwidth Terbatas." },
  { "en": "Apa Itu Equalizer (Penyesuai)?", "id": "Filter (Mengurangi Distorsi Sinyal, ISI)." },
  { "en": "Apa Itu Linear Equalizer?", "id": "Equalizer (Menggunakan Filter Linear)." },
  { "en": "Apa Itu Decision Feedback Equalizer (DFE)?", "id": "Equalizer (Menggunakan Keputusan Umpan Balik)." },
  { "en": "Apa Kepanjangan Dari DFE (Decision Feedback Equalizer)?", "id": "Decision Feedback Equalizer." },
  { "en": "Apa Itu Adaptive Equalizer?", "id": "Equalizer (Dapat Menyesuaikan Diri Otomatis)." },
  { "en": "Apa Itu Orthogonality (Ortogonalitas)?", "id": "Sifat (Sinyal Tidak Saling Mengganggu)." },
  { "en": "Apa Itu Orthogonal Frequency Division Multiplexing (OFDM)?", "id": "Modulasi (Banyak Sub-carrier Orthogonal)." },
  { "en": "Dimana OFDM (Orthogonal Frequency Division Multiplexing) Digunakan?", "id": "Wi-Fi, LTE (Long Term Evolution), 5G." },
  { "en": "Apa Itu Cyclic Prefix (CP)?", "id": "Awalan Siklik (Salinan Akhir Simbol)." },
  { "en": "Apa Fungsi CP (Cyclic Prefix) (OFDM)?", "id": "Mengatasi ISI (Inter-Symbol Interference)." },
  { "en": "Masalah PAPR (Peak-to-Average Power Ratio) Tinggi (OFDM)?", "id": "Membutuhkan Amplifier Linear (Mahal)." },
  { "en": "Dimana SC-FDMA (Single-Carrier Frequency Division Multiple Access) Digunakan?", "id": "Uplink Jaringan LTE (Long Term Evolution)." },
  { "en": "Apa Itu Orthogonal Code?", "id": "Kode (Saling Ortogonal, Tidak Interferensi)." },
  { "en": "Contoh Orthogonal Code?", "id": "Kode Walsh-Hadamard." },
  { "en": "Apa Itu Spreading Factor (CDMA)?", "id": "Faktor Penebaran Sinyal (CDMA)." },
  { "en": "Apa Itu Information Theory (Teori Informasi)?", "id": "Studi Kuantifikasi Penyimpanan Komunikasi Informasi." },
  { "en": "Apa Itu Entropi (Teori Informasi)?", "id": "Ukuran Ketidakpastian Suatu Sumber Informasi." },
  { "en": "Apa Itu Mutual Information (Informasi Timbal Balik)?", "id": "Ukuran Informasi (Antara Dua Variabel)." },
  { "en": "Apa Itu Teorema Shannon-Hartley?", "id": "Rumus Kapasitas Saluran (Noise Gaussian)." },
  { "en": "Faktor Apa Membatasi Kapasitas Saluran?", "id": "Bandwidth Dan SNR (Signal-to-Noise Ratio)." },
  { "en": "Apa Itu Source Coding (Pengkodean Sumber)?", "id": "Proses Kompresi Data (Mengurangi Redundansi)." },
  { "en": "Tujuan Source Coding?", "id": "Efisiensi Transmisi Atau Penyimpanan." },
  { "en": "Contoh Source Coding?", "id": "Huffman Coding, Lempel-Ziv (LZ)." },
  { "en": "Apa Itu Huffman Coding?", "id": "Kode (Optimal, Berbasis Frekuensi Simbol)." },
  { "en": "Apa Itu Channel Coding (Pengkodean Kanal)?", "id": "Proses Menambah Redundansi (Koreksi Error)." },
  { "en": "Tujuan Channel Coding?", "id": "Melawan Noise Gangguan Saluran." },
  { "en": "Apa Itu Code Rate (Laju Kode)?", "id": "Rasio Bit Informasi Total Bit." },
  { "en": "Apa Itu Block Code (Kode Blok)?", "id": "Channel Coding (Bekerja Per Blok)." },
  { "en": "Apa Itu Convolutional Code (Kode Konvolusional)?", "id": "Channel Coding (Bekerja Berkelanjutan)." },
  { "en": "Apa Itu Parity Check Code?", "id": "Kode Deteksi Error (Bit Paritas)." },
  { "en": "Apa Itu Hamming Code?", "id": "Kode Koreksi Error (Blok Linear)." },
  { "en": "Apa Itu Viterbi Algorithm?", "id": "Algoritma Dekode Kode Konvolusional (Optimal)." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Hello Packet?", "id": "Pesan Menemukan Menjaga Tetangga." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Dead Interval?", "id": "Waktu Maksimal (Tanpa Hello)." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Database Description (DBD)?", "id": "Ringkasan Database Link-State (LSDB)." },
  { "en": "Apa Kepanjangan Dari DBD (Database Description)?", "id": "Database Description." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Link-State Request (LSR)?", "id": "Meminta Informasi LSA (Link-State Advertisement) Detail." },
  { "en": "Apa Kepanjangan Dari LSR (Link-State Request)?", "id": "Link-State Request." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Link-State Update (LSU)?", "id": "Mengirimkan LSA (Link-State Advertisement) Diminta." },
  { "en": "Apa Kepanjangan Dari LSU (Link-State Update)?", "id": "Link-State Update." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Link-State Acknowledgment (LSAck)?", "id": "Konfirmasi Penerimaan LSU (Link-State Update)." },
  { "en": "Apa Kepanjangan Dari LSAck (Link-State Acknowledgment)?", "id": "Link-State Acknowledgment." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Area?", "id": "Pengelompokan Logis Router OSPF." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Area 0?", "id": "Area Backbone Utama OSPF." },
  { "en": "Apa Itu Area Border Router (ABR)?", "id": "Router Penghubung Area 0 Area Lain." },
  { "en": "Apa Itu Autonomous System Boundary Router (ASBR)?", "id": "Router Penghubung OSPF Protokol Lain." },
  { "en": "Apa Itu LSA (Link-State Advertisement) Tipe 1?", "id": "Router LSA (Link-State Advertisement) (Internal Area)." },
  { "en": "Apa Itu LSA (Link-State Advertisement) Tipe 2?", "id": "Network LSA (Link-State Advertisement) (Oleh DR)." },
  { "en": "Apa Itu LSA (Link-State Advertisement) Tipe 3?", "id": "Summary LSA (Link-State Advertisement) (Antar Area)." },
  { "en": "Apa Itu LSA (Link-State Advertisement) Tipe 4?", "id": "Summary LSA (Link-State Advertisement) (Menuju ASBR)." },
  { "en": "Apa Itu LSA (Link-State Advertisement) Tipe 5?", "id": "External LSA (Link-State Advertisement) (Rute Eksternal)." },
  { "en": "Apa Itu Designated Router (DR)?", "id": "Router Pilihan (Jaringan Multi-Access)." },
  { "en": "Apa Itu Backup Designated Router (BDR)?", "id": "Router Cadangan DR (Designated Router)." },
  { "en": "Mengapa DR (Designated Router) BDR (Backup Designated Router) Diperlukan?", "id": "Mengurangi Adjacency OSPF (Open Shortest Path First)." },
  { "en": "Bagaimana DR (Designated Router) Dipilih?", "id": "Prioritas Tertinggi, Lalu Router ID Tertinggi." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Router ID?", "id": "Identitas Unik Router OSPF (32-bit)." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Cost?", "id": "Metrik OSPF (Open Shortest Path First) (Berbasis Bandwidth)." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Reference Bandwidth?", "id": "Bandwidth Referensi (Perhitungan Cost)." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Stub Area?", "id": "Area (Memblokir Rute Eksternal LSA 5)." },
  { "en": "Apa Itu OSPF (Open Shortest Path First) Not-So-Stubby Area (NSSA)?", "id": "Area Stub (Mengizinkan ASBR Internal)." },
  { "en": "Apa Itu LSA (Link-State Advertisement) Tipe 7?", "id": "LSA (Link-State Advertisement) Eksternal (Area NSSA)." },
  { "en": "Algoritma Apa Digunakan EIGRP (Enhanced Interior Gateway Routing Protocol)?", "id": "DUAL (Diffusing Update Algorithm)." },
  { "en": "Metrik Apa Digunakan EIGRP (Enhanced Interior Gateway Routing Protocol)?", "id": "Komposit (Bandwidth, Delay, Reliabilitas, Beban)." },
  { "en": "Metrik Default EIGRP (Enhanced Interior Gateway Routing Protocol)?", "id": "Bandwidth Dan Delay (K-Values)." },
  { "en": "Apa Itu Successor (EIGRP)?", "id": "Jalur Terbaik Menuju Tujuan (EIGRP)." },
  { "en": "Apa Itu Feasible Successor (FS)?", "id": "Jalur Cadangan Bebas Loop (EIGRP)." },
  { "en": "Apa Kepanjangan Dari FS (Feasible Successor)?", "id": "Feasible Successor." },
  { "en": "Apa Itu Feasible Distance (FD)?", "id": "Metrik Terbaik (Total) Menuju Tujuan." },
  { "en": "Apa Kepanjangan Dari FD (Feasible Distance)?", "id": "Feasible Distance." },
  { "en": "Apa Itu Advertised Distance (AD) EIGRP?", "id": "Metrik Dilaporkan Tetangga Menuju Tujuan." },
  { "en": "Apa Kepanjangan Dari AD (Advertised Distance)?", "id": "Advertised Distance." },
  { "en": "Apa Itu Feasibility Condition (EIGRP)?", "id": "Syarat FS (AD < FD Successor)." },
  { "en": "Protokol Transport Apa Digunakan EIGRP (Enhanced Interior Gateway Routing Protocol)?", "id": "RTP (Reliable Transport Protocol)." },
  { "en": "Apa Itu EIGRP (Enhanced Interior Gateway Routing Protocol) Hello Packet?", "id": "Menjaga Hubungan Tetangga (EIGRP)." },
  { "en": "Apa Itu EIGRP (Enhanced Interior Gateway Routing Protocol) Update Packet?", "id": "Mengirim Informasi Rute (EIGRP)." },
  { "en": "Apa Itu EIGRP (Enhanced Interior Gateway Routing Protocol) Query Packet?", "id": "Mencari Jalur Alternatif (Saat Successor Gagal)." },
  { "en": "Apa Itu EIGRP (Enhanced Interior Gateway Routing Protocol) Reply Packet?", "id": "Balasan Untuk EIGRP (Enhanced Interior Gateway Routing Protocol) Query." },
  { "en": "Apa Itu EIGRP (Enhanced Interior Gateway Routing Protocol) ACK (Acknowledgment) Packet?", "id": "Konfirmasi Penerimaan Paket (RTP)." },
  { "en": "Apa Itu EIGRP (Enhanced Interior Gateway Routing Protocol) Autonomous System (AS)?", "id": "Nomor Identifikasi Jaringan EIGRP." },
  { "en": "Apa Itu EIGRP (Enhanced Interior Gateway Routing Protocol) Stub Routing?", "id": "Membatasi Penyebaran Rute (Router Cabang)." },
  { "en": "Apa Itu EIGRP (Enhanced Interior Gateway Routing Protocol) Variance?", "id": "Fitur Load Balancing (Jalur Tidak Sama)." },
  { "en": "Apa Itu Exterior Gateway Protocol (EGP)?", "id": "Protokol Routing (Antar AS Berbeda)." },
  { "en": "Apa Itu Border Gateway Protocol (BGP)?", "id": "Protokol EGP (Exterior Gateway Protocol) Utama Internet." },
  { "en": "Sebutkan Atribut BGP (Border Gateway Protocol) Well-Known Mandatory?", "id": "AS-Path, Origin, Next-Hop." },
  { "en": "Sebutkan Atribut BGP (Border Gateway Protocol) Well-Known Discretionary?", "id": "Local Preference, Atomic Aggregate." },
  { "en": "Apa Itu Atribut AS-Path?", "id": "Daftar AS (Autonomous System) Yang Dilewati." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Peer?", "id": "Router Tetangga BGP (Border Gateway Protocol)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Full Mesh (iBGP)?", "id": "Semua Router iBGP (Internal BGP) Saling Terhubung." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Route Reflector (RR)?", "id": "Solusi Skalabilitas (Memantulkan Rute iBGP)." },
  { "en": "Apa Itu Address Family (BGP)?", "id": "Kategori Protokol (Contoh: IPv6, VPNv4)." },
  { "en": "Apa Itu VPNv4 (Virtual Private Network Version 4) Address Family?", "id": "Address Family (Untuk MPLS VPN)." },
  { "en": "Apa Itu Off-Hook (Telepon)?", "id": "Kondisi (Gagang Telepon Diangkat)." },
  { "en": "Apa Itu On-Hook (Telepon)?", "id": "Kondisi (Gagang Telepon Ditutup)." },
  { "en": "Apa Itu Dual-Tone Multi-Frequency (DTMF)?", "id": "Sinyal Nada Tombol Telepon." },
  { "en": "Apa Itu Pulse Dialing (Panggilan Pulsa)?", "id": "Metode Panggilan (Telepon Putar, Lama)." },
  { "en": "Apa Itu Jaringan Seluler (Cellular Network)?", "id": "Jaringan Komunikasi Nirkabel (Berbasis Sel)." },
  { "en": "Apa Itu Sel (Cell)?", "id": "Area Geografis (Dilayani Satu BTS)." },
  { "en": "Apa Itu Base Transceiver Station (BTS)?", "id": "Pemancar Penerima Sinyal (Sel)." },
  { "en": "Apa Itu Frequency Reuse (Penggunaan Ulang Frekuensi)?", "id": "Menggunakan Frekuensi Sama (Sel Jauh)." },
  { "en": "Apa Itu Mobile Switching Center (MSC)?", "id": "Pusat Penyambungan Jaringan Seluler." },
  { "en": "Apa Itu Generasi Pertama (1G)?", "id": "Jaringan Seluler Analog (Suara Saja)." },
  { "en": "Apa Itu Generasi Kedua (2G)?", "id": "Jaringan Seluler Digital (Suara, SMS)." },
  { "en": "Apa Itu Global System for Mobile Communications (GSM)?", "id": "Standar Jaringan 2G (Global)." },
  { "en": "Apa Itu Code Division Multiple Access (CDMA)?", "id": "Teknologi Jaringan 2G/3G (Kode)." },
  { "en": "Apa Itu Subscriber Identity Module (SIM)?", "id": "Kartu Identitas Pelanggan (GSM)." },
  { "en": "Apa Itu Short Message Service (SMS)?", "id": "Layanan Pesan Teks Pendek." },
  { "en": "Apa Itu Multimedia Messaging Service (MMS)?", "id": "Layanan Pesan Multimedia (Gambar, Audio)." },
  { "en": "Apa Kepanjangan Dari MMS (Multimedia Messaging Service)?", "id": "Multimedia Messaging Service." },
  { "en": "Apa Itu Generasi Ketiga (3G)?", "id": "Jaringan Seluler (Data Kecepatan Tinggi)." },
  { "en": "Apa Itu Wideband CDMA (W-CDMA)?", "id": "Teknologi Antarmuka Udara 3G (UMTS)." },
  { "en": "Apa Itu HSDPA (High-Speed Downlink Packet Access)?", "id": "HSPA (High-Speed Packet Access) (Unduh Cepat)." },
  { "en": "Apa Itu HSUPA (High-Speed Uplink Packet Access)?", "id": "HSPA (High-Speed Packet Access) (Unggah Cepat)." },
  { "en": "Apa Itu Generasi Keempat (4G)?", "id": "Jaringan Seluler (Berbasis IP, Cepat)." },
  { "en": "Apa Itu Orthogonal Frequency Division Multiple Access (OFDMA)?", "id": "Teknologi Akses (Downlink 4G/5G)." },
  { "en": "Apa Itu Single-Carrier Frequency Division Multiple Access (SC-FDMA)?", "id": "Teknologi Akses (Uplink 4G)." },
  { "en": "Apa Itu eNodeB (Evolved Node B)?", "id": "Nama BTS (Base Transceiver Station) Jaringan 4G." },
  { "en": "Apa Itu Mobility Management Entity (MME)?", "id": "Node EPC (Evolved Packet Core) (Manajemen Mobilitas)." },
  { "en": "Apa Itu Home Subscriber Server (HSS)?", "id": "Database Pelanggan 4G (Hlr+Auc)." },
  { "en": "Apa Itu Generasi Kelima (5G)?", "id": "Jaringan Seluler Generasi Kelima." },
  { "en": "Apa Itu Superposisi (Kuantum)?", "id": "Qubit (Quantum Bit) (0 Dan 1 Bersamaan)." },
  { "en": "Apa Itu Entanglement (Kuantum)?", "id": "Keterikatan Nasib Antar Qubit (Quantum Bit)." },
  { "en": "Apa Itu Algoritma Shor?", "id": "Algoritma Kuantum (Memfaktorkan Bilangan Besar)." },
  { "en": "Apa Ancaman Algoritma Shor?", "id": "Dapat Memecahkan Enkripsi RSA (Rivest–Shamir–Adleman)." },
  { "en": "Apa Itu Algoritma Grover?", "id": "Algoritma Kuantum (Pencarian Database Cepat)." },
  { "en": "Apa Itu Host ID?", "id": "Bagian Alamat IP (Internet Protocol) Identifikasi Host." },
  { "en": "Apa Itu Network ID?", "id": "Bagian Alamat IP (Internet Protocol) Identifikasi Jaringan." },
  { "en": "Apa Itu Alamat Anycast?", "id": "Alamat Untuk Penerima Terdekat (Grup)." },
  { "en": "Apa Itu Alamat IP (Internet Protocol) Loopback?", "id": "Alamat Tes Diri Sendiri (127.0.0.1)." },
  { "en": "Apa Itu APIPA (Automatic Private IP Addressing)?", "id": "Alamat IP (Internet Protocol) Otomatis (Gagal DHCP)." },
  { "en": "Apa Itu CIDR (Classless Inter-Domain Routing)?", "id": "Metode Alokasi IP (Internet Protocol) (Notasi Prefix)." },
  { "en": "Apa Itu Notasi Prefix (Contoh: /24)?", "id": "Menunjukkan Jumlah Bit Subnet Mask." },
  { "en": "Apa Nama Lain Supernetting?", "id": "Route Summarization (Ringkasan Rute)." },
  { "en": "Apa Itu VLSM (Variable Length Subnet Masking)?", "id": "Subnetting (Panjang Mask Bervariasi)." },
  { "en": "Apa Itu Protokol Routing Classful?", "id": "Protokol (Tidak Mengirim Subnet Mask)." },
  { "en": "Contoh Protokol Classful?", "id": "RIPv1 (Routing Information Protocol version 1)." },
  { "en": "Apa Itu Protokol Routing Classless?", "id": "Protokol (Mengirim Subnet Mask, VLSM)." },
  { "en": "Contoh Protokol Classless?", "id": "RIPv2 (Routing Information Protocol version 2), OSPF." },
  { "en": "Apa Itu Manual Summary (Routing)?", "id": "Meringkas Rute (Dikonfigurasi Manual)." },
  { "en": "Apa Itu Distance Vector Protocol?", "id": "Protokol Routing (Berbasis Jarak Vektor)." },
  { "en": "Metrik Apa Digunakan Distance Vector?", "id": "Hop Count (Contoh: RIP)." },
  { "en": "Apa Itu Link-State Protocol?", "id": "Protokol Routing (Berbasis Status Link)." },
  { "en": "Metrik Apa Digunakan Link-State?", "id": "Cost (Contoh: OSPF)." },
  { "en": "Apa Itu Hybrid Protocol (Routing)?", "id": "Protokol (Gabungan Distance Vector Link-State)." },
  { "en": "Berapa AD (Administrative Distance) Connected?", "id": "0 (Nol)." },
  { "en": "Berapa AD (Administrative Distance) EIGRP (Enhanced Interior Gateway Routing Protocol) Internal?", "id": "90 (Sembilan Puluh)." },
  { "en": "Berapa AD (Administrative Distance) OSPF (Open Shortest Path First)?", "id": "110 (Seratus Sepuluh)." },
  { "en": "Berapa AD (Administrative Distance) IS-IS (Intermediate System to Intermediate System)?", "id": "115 (Seratus Lima Belas)." },
  { "en": "Berapa AD (Administrative Distance) RIP (Routing Information Protocol)?", "id": "120 (Seratus Dua Puluh)." },
  { "en": "Berapa AD (Administrative Distance) EIGRP (Enhanced Interior Gateway Routing Protocol) External?", "id": "170 (Seratus Tujuh Puluh)." },
  { "en": "Berapa AD (Administrative Distance) eBGP (External Border Gateway Protocol)?", "id": "20 (Dua Puluh)." },
  { "en": "Berapa AD (Administrative Distance) iBGP (Internal Border Gateway Protocol)?", "id": "200 (Dua Ratus)." },
  { "en": "Apa Itu Seed Metric?", "id": "Metrik Awal (Saat Redistribusi Rute)." },
  { "en": "Apa Itu Route Filtering?", "id": "Membatasi Rute (Diiklankan Diterima)." },
  { "en": "Apa Itu Distribute-List?", "id": "Filter Rute (Menggunakan Access-List)." },
  { "en": "Apa Itu Prefix-List?", "id": "Filter Rute (Berdasarkan Prefix IP)." },
  { "en": "Apa Itu Route-Map?", "id": "Alat Kebijakan Rute (Paling Kuat)." },
  { "en": "Apa Fungsi Lain Route-Map?", "id": "Modifikasi Atribut (BGP, PBR)." },
  { "en": "Apa Itu Policy-Based Routing (PBR)?", "id": "Routing (Berdasarkan Kebijakan, Bukan Tujuan)." },
  { "en": "Apa Itu IP (Internet Protocol) SLA (Service Level Agreement)?", "id": "Fitur Monitoring Kinerja Jaringan Cisco." },
  { "en": "Apa Itu Orbit HEO (Highly Elliptical Orbit)?", "id": "Orbit Satelit Sangat Lonjong (Elips)." },
  { "en": "Apa Contoh Satelit Orbit HEO (Highly Elliptical Orbit)?", "id": "Molniya, Tundra." },
  { "en": "Apa Itu Orbit Molniya?", "id": "Orbit HEO (Highly Elliptical Orbit) (Rusia)." },
  { "en": "Apa Tujuan Orbit Molniya?", "id": "Cakupan Lintang Tinggi (Kutub)." },
  { "en": "Apa Itu Orbit Tundra?", "id": "Orbit HEO (Highly Elliptical Orbit) Sinkron." },
  { "en": "Apa Itu Apogee (Orbit)?", "id": "Titik Terjauh Satelit Dari Bumi." },
  { "en": "Apa Itu Perigee (Orbit)?", "id": "Titik Terdekat Satelit Dari Bumi." },
  { "en": "Apa Itu Inklinasi Orbit (Orbital Inclination)?", "id": "Sudut Orbit Terhadap Ekuator." },
  { "en": "Apa Itu Segmen Antariksa (Space Segment) GPS?", "id": "Konstelasi Satelit GPS (Global Positioning System)." },
  { "en": "Apa Itu Segmen Kontrol (Control Segment) GPS?", "id": "Jaringan Stasiun Bumi (Monitor GPS)." },
  { "en": "Apa Itu Segmen Pengguna (User Segment) GPS?", "id": "Penerima Sinyal GPS (Global Positioning System)." },
  { "en": "Sinyal GPS (Global Positioning System) Apa Untuk Sipil?", "id": "Sinyal L1 C/A (Coarse/Acquisition)." },
  { "en": "Sinyal GPS (Global Positioning System) Apa Untuk Militer?", "id": "Sinyal P(Y) (Precise)." },
  { "en": "Apa Itu Trilaterasi (GPS)?", "id": "Metode Penentuan Posisi (Tiga Sinyal)." },
  { "en": "Apa Itu Propagasi Troposfer?", "id": "Perambatan Gelombang (Lapisan Troposfer)." },
  { "en": "Apa Itu Pembiasan (Refraksi) Atmosfer?", "id": "Pembelokan Sinyal Radio (Atmosfer)." },
  { "en": "Apa Itu Saluran Troposfer (Tropospheric Ducting)?", "id": "Perambatan Jauh (Sinyal Terperangkap)." },
  { "en": "Apa Itu Fading Datar (Flat Fading)?", "id": "Pelemahan Sinyal (Seragam Frekuensi)." },
  { "en": "Apa Itu Fading Selektif Frekuensi?", "id": "Pelemahan Sinyal (Berbeda Frekuensi)." },
  { "en": "Apa Itu Fading Cepat (Fast Fading)?", "id": "Fluktuasi Sinyal (Sangat Cepat)." },
  { "en": "Apa Itu Fading Lambat (Slow Fading)?", "id": "Fluktuasi Sinyal (Lambat, Shadowing)." },
  { "en": "Apa Itu Polarisasi Linear (Antena)?", "id": "Orientasi Gelombang (Vertikal Horizontal)." },
  { "en": "Apa Itu Polarisasi Circular (Antena)?", "id": "Orientasi Gelombang (Berputar Searah Jarum)." },
  { "en": "Apa Itu Polarisasi Eliptikal (Antena)?", "id": "Orientasi Gelombang (Bentuk Elips)." },
  { "en": "Apa Itu Directivity (Keterarahan Antena)?", "id": "Kemampuan Fokus Sinyal Satu Arah." },
  { "en": "Apa Itu Efisiensi Antena?", "id": "Rasio Daya Dipancarkan Daya Masukan." },
  { "en": "Apa Itu Lebar Bidang (Beamwidth) Antena?", "id": "Lebar Sudut Pancaran Utama Sinyal." },
  { "en": "Apa Itu Side Lobe (Antena)?", "id": "Pancaran Sinyal (Arah Samping, Lemah)." },
  { "en": "Apa Itu Back Lobe (Antena)?", "id": "Pancaran Sinyal (Arah Belakang, Lemah)." },
  { "en": "Apa Itu Front-to-Back Ratio (Antena)?", "id": "Rasio Pancaran Depan Belakang (Antena)." },
  { "en": "Apa Itu Antena Dipole Setengah Gelombang?", "id": "Antena Dasar (Panjang Setengah Gelombang)." },
  { "en": "Apa Itu Antena Monopole Seperempat Gelombang?", "id": "Antena (Seperempat Gelombang, Ground Plane)." },
  { "en": "Apa Itu Ground Plane (Antena)?", "id": "Bidang Konduktif (Reflektor Monopole)." },
  { "en": "Apa Itu Antena Yagi-Uda?", "id": "Antena Directional (Banyak Elemen)." },
  { "en": "Apa Itu Elemen Driven (Antena Yagi)?", "id": "Elemen Aktif (Terhubung Kabel)." },
  { "en": "Apa Itu Elemen Reflektor (Antena Yagi)?", "id": "Elemen Pasif (Memantulkan Sinyal)." },
  { "en": "Apa Itu Elemen Direktor (Antena Yagi)?", "id": "Elemen Pasif (Mengarahkan Sinyal)." },
  { "en": "Apa Itu Antena Heliks (Helical)?", "id": "Antena (Bentuk Spiral, Polarisasi Circular)." },
  { "en": "Apa Itu Antena Patch (Microstrip)?", "id": "Antena Datar (Di Atas PCB)." },
  { "en": "Apa Itu European Telecommunications Standards Institute (ETSI)?", "id": "Badan Standarisasi Telekomunikasi Eropa." },
  { "en": "Standar Apa Dibuat ETSI (European Telecommunications Standards Institute)?", "id": "Standar GSM (Global System for Mobile Communications), TETRA." },
  { "en": "Apa Itu 3rd Generation Partnership Project (3GPP)?", "id": "Organisasi Standarisasi (Jaringan Seluler)." },
  { "en": "Standar Apa Dibuat 3GPP (3rd Generation Partnership Project)?", "id": "UMTS (Universal Mobile Telecommunications System), LTE, 5G." },
  { "en": "Apa Itu 3GPP2 (3rd Generation Partnership Project 2)?", "id": "Organisasi Standarisasi (Teknologi CDMA)." },
  { "en": "Apa Kepanjangan Dari 3GPP2 (3rd Generation Partnership Project 2)?", "id": "3rd Generation Partnership Project 2." },
  { "en": "Standar Apa Dibuat 3GPP2 (3rd Generation Partnership Project 2)?", "id": "CDMA2000 (Code Division Multiple Access 2000)." },
  { "en": "Apa Itu Internet Engineering Task Force (IETF)?", "id": "Organisasi Standarisasi (Protokol Internet)." },
  { "en": "Apa Itu Request for Comments (RFC)?", "id": "Dokumen Standar IETF (Internet Engineering Task Force)." },
  { "en": "Apa Itu Institute of Electrical and Electronics Engineers (IEEE)?", "id": "Organisasi Standar (Teknik Elektro)." },
  { "en": "Apa Kepanjangan Dari IEEE (Institute of Electrical and Electronics Engineers)?", "id": "Institute of Electrical and Electronics Engineers." },
  { "en": "Apa Itu Standar IEEE (Institute of Electrical and Electronics Engineers) 802?", "id": "Standar Jaringan (LAN, MAN)." },
  { "en": "Apa Itu Standar IEEE (Institute of Electrical and Electronics Engineers) 802.3?", "id": "Standar Untuk Ethernet." },
  { "en": "Apa Itu Standar IEEE (Institute of Electrical and Electronics Engineers) 802.11?", "id": "Standar Untuk Wi-Fi (WLAN)." },
  { "en": "Apa Itu Standar IEEE (Institute of Electrical and Electronics Engineers) 802.15?", "id": "Standar Untuk WPAN (Bluetooth, Zigbee)." },
  { "en": "Apa Itu International Telecommunication Union (ITU)?", "id": "Badan Telekomunikasi (PBB)." },
  { "en": "Apa Itu Sektor ITU-T (Telecommunication Standardization Sector)?", "id": "Sektor ITU (International Telecommunication Union) (Standar Teknis)." },
  { "en": "Apa Itu Sektor ITU-R (Radiocommunication Sector)?", "id": "Sektor ITU (International Telecommunication Union) (Spektrum Radio)." },
  { "en": "Apa Itu Sektor ITU-D (Development Sector)?", "id": "Sektor ITU (International Telecommunication Union) (Pengembangan)." },
  { "en": "Apa Itu Rekomendasi ITU-T (International Telecommunication Union Telecommunication Standardization Sector)?", "id": "Dokumen Standar (ITU-T)." },
  { "en": "Apa Itu Serangan Smurf?", "id": "Serangan DDoS (Banjir ICMP Echo Reply)." },
  { "en": "Apa Itu Serangan Fraggle?", "id": "Serangan Smurf (Menggunakan UDP Echo)." },
  { "en": "Apa Itu Serangan Ping of Death?", "id": "Serangan DoS (Paket ICMP Terfragmentasi Besar)." },
  { "en": "Apa Itu Serangan Teardrop?", "id": "Serangan DoS (Fragmentasi IP Tumpang Tindih)." },
  { "en": "Apa Itu Serangan Land?", "id": "Serangan DoS (IP Sumber Tujuan Sama)." },
  { "en": "Apa Itu Serangan SYN (Synchronize) Flood?", "id": "Serangan DoS (Banjir Permintaan Koneksi TCP)." },
  { "en": "Apa Itu Serangan Refleksi (Reflection Attack)?", "id": "Serangan (Memantulkan Trafik Lewat Pihak Ketiga)." },
  { "en": "Apa Itu Serangan Amplifikasi (Amplification Attack)?", "id": "Serangan Refleksi (Respon Jauh Lebih Besar)." },
  { "en": "Protokol Apa Rentan Amplifikasi?", "id": "DNS (Domain Name System), NTP, SNMP." },
  { "en": "Apa Itu Serangan DNS (Domain Name System) Amplification?", "id": "Amplifikasi (Menggunakan Kueri DNS Palsu)." },
  { "en": "Apa Itu Serangan NTP (Network Time Protocol) Amplification?", "id": "Amplifikasi (Menggunakan Perintah NTP Palsu)." },
  { "en": "Apa Itu Serangan Phlashing?", "id": "Serangan (Merusak Firmware Perangkat Keras)." },
  { "en": "Apa Itu Serangan Salami?", "id": "Serangan (Pencurian Kecil Berulang-ulang)." },
  { "en": "Apa Itu Serangan Man-in-the-Browser (MITB)?", "id": "Serangan (Trojan Memodifikasi Transaksi Browser)." },
  { "en": "Apa Kepanjangan Dari MITB (Man-in-the-Browser)?", "id": "Man-in-the-Browser." },
  { "en": "Apa Itu Serangan Typosquatting?", "id": "Serangan (Menggunakan Nama Domain Mirip)." },
  { "en": "Apa Itu Serangan Clickjacking?", "id": "Serangan (Menipu Pengguna (Klik Tersembunyi))." },
  { "en": "Apa Itu Serangan Session Hijacking?", "id": "Serangan (Mengambil Alih Sesi Pengguna)." },
  { "en": "Apa Itu Session ID?", "id": "Token Unik (Identifikasi Sesi Pengguna)." },
  { "en": "Apa Itu Serangan SQL (Structured Query Language) Injection?", "id": "Serangan (Menyisipkan Kueri SQL Jahat)." },
  { "en": "Apa Itu Serangan Blind SQL (Structured Query Language) Injection?", "id": "SQL (Structured Query Language) Injection (Tanpa Respon Error)." },
  { "en": "Apa Itu Serangan Cross-Site Scripting (XSS)?", "id": "Serangan (Menyisipkan Skrip Sisi Klien)." },
  { "en": "Apa Itu Stored XSS (Cross-Site Scripting)?", "id": "Skrip Jahat (Disimpan Di Server)." },
  { "en": "Apa Itu Reflected XSS (Cross-Site Scripting)?", "id": "Skrip Jahat (Dipantulkan Lewat URL)." },
  { "en": "Apa Itu DOM-based XSS (Cross-Site Scripting)?", "id": "XSS (Cross-Site Scripting) (Di Sisi Klien, DOM)." },
  { "en": "Apa Itu Cross-Site Request Forgery (CSRF)?", "id": "Serangan (Memaksa Pengguna (Melakukan Aksi))." },
  { "en": "Apa Itu Server-Side Request Forgery (SSRF)?", "id": "Serangan (Memaksa Server (Membuat Permintaan))." },
  { "en": "Apa Kepanjangan Dari SSRF (Server-Side Request Forgery)?", "id": "Server-Side Request Forgery." },
  { "en": "Apa Itu Serangan Buffer Overflow?", "id": "Serangan (Menulis Data Melebihi Buffer)." },
  { "en": "Apa Itu Serangan Heap Overflow?", "id": "Buffer Overflow (Di Area Heap)." },
  { "en": "Apa Itu Serangan Stack Overflow?", "id": "Buffer Overflow (Di Area Stack)." },
  { "en": "Apa Itu Format String Attack?", "id": "Serangan (Memanfaatkan Fungsi Format String)." },
  { "en": "Apa Itu Serangan Integer Overflow?", "id": "Serangan (Error Perhitungan Aritmatika Integer)." },
  { "en": "Apa Itu Antena Cassegrain?", "id": "Antena Parabola (Reflektor Ganda)." },
  { "en": "Apa Itu Antena Gregorian?", "id": "Antena Parabola (Mirip Cassegrain)." },
  { "en": "Apa Itu Feedhorn (Antena)?", "id": "Pemandu Gelombang (Fokus Parabola)." },
  { "en": "Apa Itu Boresight (Antena)?", "id": "Sumbu Arah Pancaran Utama Antena." },
  { "en": "Apa Itu Azimuth (Penargetan Antena)?", "id": "Sudut Horizontal (Relatif Utara)." },
  { "en": "Apa Itu Elevasi (Penargetan Antena)?", "id": "Sudut Vertikal (Relatif Horison)." },
  { "en": "Apa Itu Telemetry, Tracking, and Command (TT&C)?", "id": "Sub-Sistem Komunikasi Satelit." },
  { "en": "Apa Kepanjangan Dari TT&C (Telemetry, Tracking, and Command)?", "id": "Telemetry, Tracking, and Command." },
  { "en": "Apa Itu Telemetri (Satelit)?", "id": "Pengiriman Data Status Satelit." },
  { "en": "Apa Itu Perintah (Command) (Satelit)?", "id": "Instruksi Kontrol (Dikirim Ke Satelit)." },
  { "en": "Apa Itu Pelacakan (Tracking) (Satelit)?", "id": "Pemantauan Posisi Orbit Satelit." },
  { "en": "Apa Itu Subsistem Daya (Satelit)?", "id": "Sistem (Menyediakan Listrik Satelit)." },
  { "en": "Sumber Daya Utama Satelit?", "id": "Panel Surya (Solar Panel)." },
  { "en": "Apa Itu Attitude Control (Satelit)?", "id": "Kontrol Orientasi Arah Satelit." },
  { "en": "Apa Itu Stasiun Bumi (Ground Station)?", "id": "Fasilitas Bumi (Komunikasi Satelit)." },
  { "en": "Apa Prinsip Dasar Radar?", "id": "Deteksi Objek (Pantulan Gelombang Radio)." },
  { "en": "Apa Kepanjangan Dari Radar (Radio Detection and Ranging)?", "id": "Radio Detection and Ranging." },
  { "en": "Apa Itu Radar Pulsa (Pulse Radar)?", "id": "Radar (Mengirim Pulsa Sinyal Pendek)." },
  { "en": "Apa Itu Radar Doppler?", "id": "Radar (Mengukur Kecepatan (Pergeseran Doppler))." },
  { "en": "Apa Itu Radar Cross-Section (RCS)?", "id": "Ukuran (Kemampuan Objek Pantulkan Sinyal)." },
  { "en": "Apa Kepanjangan Dari RCS (Radar Cross-Section)?", "id": "Radar Cross-Section." },
  { "en": "Apa Itu Phased-Array Radar?", "id": "Radar (Arah Sinyal Diatur Elektronik)." },
  { "en": "Apa Itu Radar Pasif?", "id": "Radar (Mendeteksi Sinyal (Tanpa Memancar))." },
  { "en": "Apa Itu Pemandu Gelombang (Waveguide)?", "id": "Saluran Logam (Transmisi Frekuensi Tinggi)." },
  { "en": "Dimana Waveguide Digunakan?", "id": "Sistem Microwave, Radar." },
  { "en": "Apa Itu Microwave (Gelombang Mikro)?", "id": "Gelombang Radio (Frekuensi Sangat Tinggi)." },
  { "en": "Apa Itu Circulator (Sirkulator)?", "id": "Perangkat (Mengatur Aliran Sinyal Searah)." },
  { "en": "Apa Itu Isolator (Microwave)?", "id": "Perangkat (Meloloskan Sinyal Satu Arah)." },
  { "en": "Apa Fungsi Isolator?", "id": "Melindungi Pemancar (Dari Pantulan Daya)." },
  { "en": "Kapan Duplexer Digunakan?", "id": "Radio Dua Arah, Repeater." },
  { "en": "Apa Itu Diplexer?", "id": "Menggabung Memisah (Sinyal Frekuensi Beda)." },
  { "en": "Apa Itu Filter Microwave?", "id": "Perangkat (Meloloskan Frekuensi Tertentu)." },
  { "en": "Apa Itu Attenuator (Pelemah)?", "id": "Perangkat (Mengurangi Kekuatan Sinyal)." },
  { "en": "Apa Itu Penerima Superheterodyne?", "id": "Arsitektur Penerima Radio (Paling Umum)." },
  { "en": "Apa Itu Mixer (Radio)?", "id": "Komponen (Mengubah Frekuensi Sinyal)." },
  { "en": "Apa Itu Osilator Lokal (Local Oscillator)?", "id": "Penghasil Sinyal Referensi (Mixer)." },
  { "en": "Apa Kepanjangan Dari LO (Local Oscillator)?", "id": "Local Oscillator." },
  { "en": "Apa Itu Frekuensi Menengah (Intermediate Frequency)?", "id": "Frekuensi Hasil (Setelah Mixer)." },
  { "en": "Apa Kepanjangan Dari IF (Intermediate Frequency)?", "id": "Intermediate Frequency." },
  { "en": "Keuntungan Penerima Superheterodyne?", "id": "Selektivitas Sensitivitas Yang Baik." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian (Menghasilkan Sinyal Periodik)." },
  { "en": "Apa Itu Penguat Daya (Power Amplifier)?", "id": "Komponen (Menguatkan Sinyal Pemancar)." },
  { "en": "Apa Kepanjangan Dari PA (Power Amplifier)?", "id": "Power Amplifier." },
  { "en": "Apa Itu Modulator?", "id": "Rangkaian (Melakukan Proses Modulasi)." },
  { "en": "Apa Itu Demodulator (Detektor)?", "id": "Rangkaian (Melakukan Proses Demodulasi)." },
  { "en": "Apa Itu Automatic Gain Control (AGC)?", "id": "Kontrol (Menjaga Level Sinyal Output)." },
  { "en": "Apa Kepanjangan Dari AGC (Automatic Gain Control)?", "id": "Automatic Gain Control." },
  { "en": "Apa Itu Direct Conversion Receiver?", "id": "Penerima (Langsung Ke Baseband, Tanpa IF)." },
  { "en": "Apa Itu Zero-IF (Zero Intermediate Frequency) Receiver?", "id": "Nama Lain Direct Conversion Receiver." },
  { "en": "Apa Itu Software-Defined Radio (SDR)?", "id": "Radio (Fungsi Didefinisikan Perangkat Lunak)." },
  { "en": "Apa Itu Analog-to-Digital Converter (ADC)?", "id": "Konverter (Sinyal Analog Ke Digital)." },
  { "en": "Apa Kepanjangan Dari ADC (Analog-to-Digital Converter)?", "id": "Analog-to-Digital Converter." },
  { "en": "Apa Itu Digital-to-Analog Converter (DAC)?", "id": "Konverter (Sinyal Digital Ke Analog)." },
  { "en": "Apa Kepanjangan Dari DAC (Digital-to-Analog Converter)?", "id": "Digital-to-Analog Converter." },
  { "en": "Apa Itu Digital Signal Processing (DSP)?", "id": "Pemrosesan Sinyal (Dalam Domain Digital)." },
  { "en": "Apa Kepanjangan Dari DSP (Digital Signal Processing)?", "id": "Digital Signal Processing." },
  { "en": "Apa Itu Filter Digital?", "id": "Filter (Diimplementasikan (Perangkat Lunak DSP))." },
  { "en": "Apa Itu Jaringan Telepon (PSTN)?", "id": "Jaringan Telepon Publik (Berbasis Kabel)." },
  { "en": "Apa Hierarki Sentral PSTN (Public Switched Telephone Network)?", "id": "Kelas 1 Hingga Kelas 5." },
  { "en": "Apa Itu Sentral Kelas 5 (Class 5)?", "id": "Sentral Lokal (Terhubung Pelanggan)." },
  { "en": "Apa Itu Sentral Kelas 4 (Class 4)?", "id": "Sentral Tandem (Menghubungkan Sentral Lokal)." },
  { "en": "Apa Itu Sinyal E&M (Ear and Mouth)?", "id": "Pensinyalan (Antar Trunk Analog)." },
  { "en": "Apa Itu Loop Start (Signaling)?", "id": "Pensinyalan (Off-Hook, Telepon Analog)." },
  { "en": "Apa Itu Ground Start (Signaling)?", "id": "Pensinyalan (Mencegah Tabrakan, PBX)." },
  { "en": "Apa Itu Sinkronisasi Jaringan?", "id": "Penyelarasan Waktu (Antar Perangkat Jaringan)." },
  { "en": "Mengapa Sinkronisasi Penting (TDM)?", "id": "Mencegah Slip (Kehilangan Data TDM)." },
  { "en": "Apa Itu Slip (Sinkronisasi)?", "id": "Error (Akibat Perbedaan Clock)." },
  { "en": "Apa Itu Primary Reference Clock (PRC)?", "id": "Sumber Clock Paling Akurat (Stratum 0)." },
  { "en": "Apa Kepanjangan Dari PRC (Primary Reference Clock)?", "id": "Primary Reference Clock." },
  { "en": "Apa Itu Stratum Clock?", "id": "Level Hierarki Akurasi Clock." },
  { "en": "Apa Itu Stratum 0?", "id": "Jam Atomik (Contoh: GPS, Cesium)." },
  { "en": "Apa Itu Stratum 1?", "id": "Clock (Sinkron Langsung Stratum 0)." },
  { "en": "Apa Itu Stratum 2?", "id": "Clock (Sinkron Ke Stratum 1)." },
  { "en": "Apa Itu Stratum 3?", "id": "Clock (Sinkron Ke Stratum 2)." },
  { "en": "Apa Itu Stratum 4?", "id": "Clock (Kualitas Terendah, Internal)." },
  { "en": "Apa Itu Network Time Protocol (NTP)?", "id": "Protokol Sinkronisasi Waktu (Jaringan IP)." },
  { "en": "Apa Itu Precision Time Protocol (PTP)?", "id": "Protokol Sinkronisasi (Sangat Akurat, IEEE 1588)." },
  { "en": "Apa Itu Synchronous Ethernet (SyncE)?", "id": "Sinkronisasi Frekuensi (Lapis Fisik Ethernet)." },
  { "en": "Apa Itu Wander (Clock)?", "id": "Penyimpangan Fasa (Sangat Lambat)." },
  { "en": "Apa Itu Jitter (Clock)?", "id": "Penyimpangan Fasa (Sangat Cepat)." },
  { "en": "Apa Itu Spectrum Analyzer?", "id": "Alat Ukur (Sinyal Domain Frekuensi)." },
  { "en": "Apa Itu Vector Network Analyzer (VNA)?", "id": "Alat Ukur (Karakteristik Jaringan, S-Parameter)." },
  { "en": "Apa Kepanjangan Dari VNA (Vector Network Analyzer)?", "id": "Vector Network Analyzer." },
  { "en": "Apa Itu S-Parameters (Scattering Parameters)?", "id": "Parameter (Menggambarkan Jaringan RF/Microwave)." },
  { "en": "Apa Itu S11 (S-Parameter)?", "id": "Return Loss (Pantulan Port 1)." },
  { "en": "Apa Itu S21 (S-Parameter)?", "id": "Insertion Loss (Transmisi Port 1 Ke 2)." },
  { "en": "Apa Itu Osiloskop?", "id": "Alat Ukur (Sinyal Domain Waktu)." },
  { "en": "Apa Itu Logic Analyzer?", "id": "Alat Ukur (Sinyal Digital Majemuk)." },
  { "en": "Apa Itu Protocol Analyzer?", "id": "Alat Ukur (Menganalisis Protokol Komunikasi)." },
  { "en": "Contoh Protocol Analyzer?", "id": "Wireshark, Tcpdump." },
  { "en": "Apa Itu Bit Error Rate Tester (BERT)?", "id": "Alat Ukur (Menghitung Bit Error Rate)." },
  { "en": "Apa Itu Kodek Audio?", "id": "Perangkat Lunak (Kompresi Dekompresi Audio)." },
  { "en": "Apa Itu AAC (Advanced Audio Coding)?", "id": "Format Kompresi Audio (Pengganti MP3)." },
  { "en": "Apa Kepanjangan Dari WMA (Windows Media Audio)?", "id": "Windows Media Audio." },
  { "en": "Apa Itu Kodek Video?", "id": "Perangkat Lunak (Kompresi Dekompresi Video)." },
  { "en": "Apa Kepanjangan Dari MPEG (Moving Picture Experts Group)?", "id": "Moving Picture Experts Group." },
  { "en": "Apa Itu H.264 (AVC)?", "id": "Standar Kompresi Video (Paling Umum)." },
  { "en": "Apa Itu Container (Video)?", "id": "Format File (Pembungkus Audio Video)." },
  { "en": "Contoh Format Container?", "id": "MP4 (MPEG-4 Part 14), MKV, AVI." },
  { "en": "Apa Itu Variable Bitrate (VBR)?", "id": "Bitrate (Berubah Sesuai Kompleksitas)." },
  { "en": "Apa Kepanjangan Dari VBR (Variable Bitrate)?", "id": "Variable Bitrate." },
  { "en": "Apa Itu Constant Bitrate (CBR)?", "id": "Bitrate (Tetap Konstan Sepanjang Video)." },
  { "en": "Apa Kepanjangan Dari CBR (Constant Bitrate)?", "id": "Constant Bitrate." },
  { "en": "Apa Itu Frame Rate (FPS)?", "id": "Jumlah Gambar (Frame) Per Detik." },
  { "en": "Apa Kepanjangan Dari FPS (Frames Per Second)?", "id": "Frames Per Second." },
  { "en": "Apa Itu Resolusi Video?", "id": "Jumlah Piksel (Lebar Kali Tinggi)." },
  { "en": "Apa Itu Aspek Rasio (Aspect Ratio)?", "id": "Rasio Perbandingan Lebar Tinggi Gambar." },
  { "en": "Apa Itu Interlaced Scan (Video)?", "id": "Pemindaian (Garis Ganjil Lalu Genap)." },
  { "en": "Apa Itu Progressive Scan (Video)?", "id": "Pemindaian (Semua Garis Berurutan)." },
  { "en": "Apa Arti 'p' (Contoh: 1080p)?", "id": "Progressive Scan." },
  { "en": "Apa Arti 'i' (Contoh: 1080i)?", "id": "Interlaced Scan." },
  { "en": "Apa Itu MPEG-DASH (Dynamic Adaptive Streaming over HTTP)?", "id": "Protokol Streaming Adaptif (Standar)." },
  { "en": "Apa Itu Transcoding (Video)?", "id": "Proses Konversi Format Ukuran Video." },
  { "en": "Apa Itu Digital Video Broadcasting (DVB)?", "id": "Standar Penyiaran TV Digital (Eropa)." },
  { "en": "Apa Itu Conditional Access System (CAS)?", "id": "Sistem Enkripsi (Siaran TV Berbayar)." },
  { "en": "Apa Kepanjangan Dari CAS (Conditional Access System)?", "id": "Conditional Access System." },
  { "en": "Apa Itu Smart Card (TV Berbayar)?", "id": "Kartu (Dekripsi Siaran Berbayar)." },
  { "en": "Apa Itu High-Definition Multimedia Interface (HDMI)?", "id": "Antarmuka Koneksi Audio Video Digital." },
  { "en": "Apa Itu High-Bandwidth Digital Content Protection (HDCP)?", "id": "Proteksi Salinan Digital (Koneksi HDMI)." },
  { "en": "Apa Itu Digital Audio Broadcasting (DAB)?", "id": "Standar Penyiaran Radio Digital." },
  { "en": "Apa Itu Radio Data System (RDS)?", "id": "Standar (Mengirim Data Teks Radio FM)." },
  { "en": "Apa Kepanjangan Dari RDS (Radio Data System)?", "id": "Radio Data System." },
  { "en": "Tujuan Network Slicing?", "id": "Layanan Berbeda (Satu Infrastruktur Fisik)." },
  { "en": "Contoh Slice (Network Slicing)?", "id": "eMBB (Enhanced Mobile Broadband), URLLC, mMTC." },
  { "en": "Apa Itu eMBB (Enhanced Mobile Broadband)?", "id": "Slice (Kecepatan Data Sangat Tinggi)." },
  { "en": "Apa Itu URLLC (Ultra-Reliable Low-Latency Communication)?", "id": "Slice (Keandalan Tinggi, Latensi Rendah)." },
  { "en": "Apa Itu mMTC (Massive Machine-Type Communication)?", "id": "Slice (Koneksi Perangkat IoT Masif)." },
  { "en": "Apa Itu Network Slice Instance (NSI)?", "id": "Instansi Jaringan Virtual (Slice Aktif)." },
  { "en": "Apa Itu Network Slice Subnet Instance (NSSI)?", "id": "Sub-Jaringan Dalam Satu Slice (NSI)." },
  { "en": "Apa Itu Network Slice Management Function (NSMF)?", "id": "Fungsi (Manajemen Siklus Hidup Slice)." },
  { "en": "Apa Itu Network Slice Selection Function (NSSF)?", "id": "Fungsi (Memilih Slice Untuk Perangkat)." },
  { "en": "Apa Itu Access and Mobility Management Function (AMF)?", "id": "Fungsi 5G Core (Manajemen Mobilitas)." },
  { "en": "Apa Itu Session Management Function (SMF)?", "id": "Fungsi 5G Core (Manajemen Sesi PDU)." },
  { "en": "Apa Itu User Plane Function (UPF)?", "id": "Fungsi 5G Core (Penerusan Data Pengguna)." },
  { "en": "Apa Itu Policy Control Function (PCF)?", "id": "Fungsi 5G Core (Manajemen Kebijakan)." },
  { "en": "Apa Itu Application Function (AF)?", "id": "Fungsi 5G Core (Interaksi Aplikasi Eksternal)." },
  { "en": "Apa Itu Network Repository Function (NRF)?", "id": "Fungsi 5G Core (Penemuan Layanan NF)." },
  { "en": "Apa Itu Network Exposure Function (NEF)?", "id": "Fungsi 5G Core (Ekspos Kemampuan Jaringan)." },
  { "en": "Apa Itu Authentication Server Function (AUSF)?", "id": "Fungsi 5G Core (Server Autentikasi)." },
  { "en": "Apa Keuntungan CUPS (Control and User Plane Separation)?", "id": "Skalabilitas, Penempatan User Plane Fleksibel." },
  { "en": "Apa Itu Frekuensi Menengah (Intermediate Frequency)?", "id": "Frekuensi Tetap (Hasil Konversi Mixer)." },
  { "en": "Apa Itu SDU (Service Data Unit)?", "id": "Data (Dari Lapis Atas, Sebelum Enkapsulasi)." },
  { "en": "Apa Kepanjangan Dari SDU (Service Data Unit)?", "id": "Service Data Unit." },
  { "en": "Apa Beda PDU (Protocol Data Unit) Dan SDU (Service Data Unit)?", "id": "PDU (Protocol Data Unit) Data Enkapsulasi, SDU Data Asli." },
  { "en": "Apa Itu Port Ephemeral (Efemeral)?", "id": "Port Temporer (Ditugaskan Klien Dinamis)." },
  { "en": "Apa Itu Multipath TCP (MPTCP)?", "id": "TCP (Transmission Control Protocol) (Menggunakan Banyak Jalur)." },
  { "en": "Apa Kepanjangan Dari MPTCP (Multipath TCP)?", "id": "Multipath TCP (Transmission Control Protocol)." },
  { "en": "Apa Keuntungan MPTCP (Multipath TCP)?", "id": "Redundansi, Peningkatan Bandwidth." },
  { "en": "Apa Itu Stream Control Transmission Protocol (SCTP)?", "id": "Protokol Transport (Mirip TCP, Fitur Baru)." },
  { "en": "Apa Kepanjangan Dari SCTP (Stream Control Transmission Protocol)?", "id": "Stream Control Transmission Protocol." },
  { "en": "Apa Fitur Utama SCTP (Stream Control Transmission Protocol)?", "id": "Multi-Streaming Dan Multi-Homing." },
  { "en": "Apa Itu Multi-Streaming (SCTP)?", "id": "Aliran Data Independen (Satu Koneksi)." },
  { "en": "Apa Itu Multi-Homing (SCTP)?", "id": "Memiliki Beberapa Alamat IP (Redundansi)." },
  { "en": "Apa Itu Header HTTP (Hypertext Transfer Protocol) Host?", "id": "Menentukan Nama Domain Server Tujuan." },
  { "en": "Apa Itu Header HTTP (Hypertext Transfer Protocol) User-Agent?", "id": "Informasi Perangkat Lunak Klien (Browser)." },
  { "en": "Apa Itu Header HTTP (Hypertext Transfer Protocol) Referer?", "id": "Alamat URL (Uniform Resource Locator) Halaman Sebelumnya." },
  { "en": "Apa Itu Cookie Sesi (Session Cookie)?", "id": "Cookie (Dihapus Saat Browser Ditutup)." },
  { "en": "Apa Itu Cookie Persisten (Persistent Cookie)?", "id": "Cookie (Disimpan (Tanggal Kedaluwarsa))." },
  { "en": "Apa Itu Cookie Pihak Ketiga (Third-Party Cookie)?", "id": "Cookie (Dibuat Domain Berbeda)." },
  { "en": "Apa Itu Metode HTTP (Hypertext Transfer Protocol) OPTIONS?", "id": "Meminta Opsi Komunikasi (Server)." },
  { "en": "Apa Itu Metode HTTP (Hypertext Transfer Protocol) PATCH?", "id": "Memodifikasi Sebagian Sumber Daya (Server)." },
  { "en": "Apa Itu Metode HTTP (Hypertext Transfer Protocol) CONNECT?", "id": "Membangun Tunnel (Terowongan) Koneksi." },
  { "en": "Apa Itu Metode HTTP (Hypertext Transfer Protocol) TRACE?", "id": "Melakukan Tes Loopback (Debug)." },
  { "en": "Apa Itu Kode Status 1xx (Informational)?", "id": "Respon (Informasi, Permintaan Diterima)." },
  { "en": "Apa Itu Kode Status 100 (Continue)?", "id": "Klien Boleh Melanjutkan Permintaan (Body)." },
  { "en": "Apa Itu Kode Status 101 (Switching Protocols)?", "id": "Server (Setuju Ganti Protokol)." },
  { "en": "Apa Itu Kode Status 201 (Created)?", "id": "Sukses (Sumber Daya Baru Dibuat)." },
  { "en": "Apa Itu Kode Status 202 (Accepted)?", "id": "Sukses (Permintaan Diterima Diproses Nanti)." },
  { "en": "Apa Itu Kode Status 204 (No Content)?", "id": "Sukses (Permintaan Berhasil, Tanpa Konten)." },
  { "en": "Apa Itu Kode Status 302 (Found)?", "id": "Pengalihan (Sementara, Pindah Temporer)." },
  { "en": "Apa Itu Kode Status 304 (Not Modified)?", "id": "Respon (Sumber Daya Tidak Berubah, Cache)." },
  { "en": "Apa Itu Kode Status 307 (Temporary Redirect)?", "id": "Pengalihan (Sementara, Metode Sama)." },
  { "en": "Apa Itu Kode Status 400 (Bad Request)?", "id": "Error (Permintaan Klien Tidak Valid)." },
  { "en": "Apa Itu Kode Status 401 (Unauthorized)?", "id": "Error (Memerlukan Autentikasi Pengguna)." },
  { "en": "Apa Itu Kode Status 403 (Forbidden)?", "id": "Error (Akses Ditolak, Tidak Ada Izin)." },
  { "en": "Apa Itu Kode Status 405 (Method Not Allowed)?", "id": "Error (Metode HTTP (Hypertext Transfer Protocol) Ditolak)." },
  { "en": "Apa Itu Kode Status 408 (Request Timeout)?", "id": "Error (Klien Terlalu Lama Mengirim)." },
  { "en": "Apa Itu Kode Status 429 (Too Many Requests)?", "id": "Error (Terlalu Banyak Permintaan, Rate Limit)." },
  { "en": "Apa Itu Kode Status 502 (Bad Gateway)?", "id": "Error (Server Menerima Respon Invalid)." },
  { "en": "Apa Itu Kode Status 503 (Service Unavailable)?", "id": "Error (Server Tidak Siap, Overload)." },
  { "en": "Apa Itu Kode Status 504 (Gateway Timeout)?", "id": "Error (Server Gateway Kehabisan Waktu)." },
  { "en": "Apa Itu Web Server Apache?", "id": "Perangkat Lunak Web Server (Populer)." },
  { "en": "Apa Itu Nginx?", "id": "Web Server (Kinerja Tinggi, Reverse Proxy)." },
  { "en": "Apa Itu Microsoft IIS (Internet Information Services)?", "id": "Web Server (Microsoft Windows)." },
  { "en": "Apa Itu Server-Side Scripting?", "id": "Skrip (Dijalankan Di Sisi Server)." },
  { "en": "Contoh Bahasa Server-Side?", "id": "PHP (Hypertext Preprocessor), Python, Node.js." },
  { "en": "Apa Itu Client-Side Scripting?", "id": "Skrip (Dijalankan Di Sisi Klien/Browser)." },
  { "en": "Contoh Bahasa Client-Side?", "id": "JavaScript." },
  { "en": "Apa Itu Common Gateway Interface (CGI)?", "id": "Standar (Eksekusi Skrip Web Server)." },
  { "en": "Apa Kepanjangan Dari CGI (Common Gateway Interface)?", "id": "Common Gateway Interface." },
  { "en": "Apa Itu FastCGI?", "id": "Peningkatan Dari CGI (Common Gateway Interface)." },
  { "en": "Apa Itu International Mobile Subscriber Identity (IMSI)?", "id": "Nomor Identitas Unik Pelanggan (SIM)." },
  { "en": "Apa Itu Temporary Mobile Subscriber Identity (TMSI)?", "id": "Nomor Identitas Temporer (Keamanan IMSI)." },
  { "en": "Apa Kepanjangan Dari TMSI (Temporary Mobile Subscriber Identity)?", "id": "Temporary Mobile Subscriber Identity." },
  { "en": "Apa Itu Kunci Ki (Autentikasi)?", "id": "Kunci Rahasia (Disimpan SIM AUC)." },
  { "en": "Apa Itu RAND (Nilai Acak)?", "id": "Nilai Acak (Dikirim Jaringan Ke MS)." },
  { "en": "Apa Itu SRES (Signed Response)?", "id": "Respon Terenkripsi (Dihasilkan SIM)." },
  { "en": "Apa Kepanjangan Dari SRES (Signed Response)?", "id": "Signed Response." },
  { "en": "Apa Itu Kunci Kc (Ciphering Key)?", "id": "Kunci Enkripsi (Dihasilkan Selama Autentikasi)." },
  { "en": "Apa Itu Algoritma A3 (GSM)?", "id": "Algoritma (Menghasilkan SRES (Signed Response))." },
  { "en": "Apa Itu Algoritma A8 (GSM)?", "id": "Algoritma (Menghasilkan Kunci Kc)." },
  { "en": "Apa Itu Algoritma A5 (GSM)?", "id": "Algoritma Enkripsi (Data Suara GSM)." },
  { "en": "Apa Itu Daftar Putih (Whitelist) EIR?", "id": "Daftar IMEI (International Mobile Equipment Identity) Valid." },
  { "en": "Apa Itu Daftar Hitam (Blacklist) EIR?", "id": "Daftar IMEI (International Mobile Equipment Identity) Diblokir." },
  { "en": "Apa Itu Daftar Abu-abu (Greylist) EIR?", "id": "Daftar IMEI (International Mobile Equipment Identity) Dipantau." },
  { "en": "Apa Itu Evolved Packet Data Gateway (ePDG)?", "id": "Gateway (Akses Wi-Fi Ke EPC)." },
  { "en": "Apa Kepanjangan Dari ePDG (Evolved Packet Data Gateway)?", "id": "Evolved Packet Data Gateway." },
  { "en": "Apa Itu Voice over Wi-Fi (VoWiFi)?", "id": "Layanan Suara (Melalui Jaringan Wi-Fi)." },
  { "en": "Apa Kepanjangan Dari VoWiFi (Voice over Wi-Fi)?", "id": "Voice over Wi-Fi." },
  { "en": "Apa Itu Unlicensed Mobile Access (UMA)?", "id": "Akses Jaringan Seluler (Spektrum Tidak Berlisensi)." },
  { "en": "Apa Kepanjangan Dari UMA (Unlicensed Mobile Access)?", "id": "Unlicensed Mobile Access." },
  { "en": "Apa Itu 4B/5B Encoding?", "id": "Encoding (4 Bit Data Jadi 5 Bit Kode)." },
  { "en": "Apa Tujuan 4B/5B Encoding?", "id": "Memastikan Transisi Sinyal (Sinkronisasi Clock)." },
  { "en": "Dimana 4B/5B Digunakan?", "id": "Fast Ethernet (100BASE-TX)." },
  { "en": "Apa Itu 8B/10B Encoding?", "id": "Encoding (8 Bit Data Jadi 10 Bit Kode)." },
  { "en": "Dimana 8B/10B Digunakan?", "id": "Gigabit Ethernet (1000BASE-T)." },
  { "en": "Apa Itu 64B/66B Encoding?", "id": "Encoding (64 Bit Data Jadi 66 Bit Kode)." },
  { "en": "Dimana 64B/66B Digunakan?", "id": "10 Gigabit Ethernet (10GBASE-R)." },
  { "en": "Apa Itu DC (Direct Current) Balance (Encoding)?", "id": "Menjaga Jumlah Rata-Rata (Bit 1 0)." },
  { "en": "Apa Itu Run-Length Limited (RLL)?", "id": "Encoding (Membatasi Jumlah Bit Sama Berurutan)." },
  { "en": "Apa Kepanjangan Dari RLL (Run-Length Limited)?", "id": "Run-Length Limited." },
  { "en": "Apa Itu Fading Cepat (Fast Fading)?", "id": "Fluktuasi Sinyal (Periode Simbol Pendek)." },
  { "en": "Apa Keuntungan HAPS (High-Altitude Platform Station)?", "id": "Cakupan Luas (Mirip Satelit, Latensi Rendah)." },
  { "en": "Apa Itu Komunikasi Kuantum?", "id": "Komunikasi Berbasis Prinsip Mekanika Kuantum." },
  { "en": "Apa Keuntungan QKD (Quantum Key Distribution)?", "id": "Sangat Aman (Deteksi Penyadap)." },
  { "en": "Apa Itu Quantum Cryptography?", "id": "Kriptografi (Menggunakan Prinsip Kuantum)." },
  { "en": "Apa Itu Post-Quantum Cryptography (PQC)?", "id": "Kriptografi (Tahan Serangan Kuantum)." },
  { "en": "Apa Itu ITU-T G.709?", "id": "Standar Kerangka Transport Optik (OTN)." },
  { "en": "Apa Itu ITU-T G.984?", "id": "Standar Untuk GPON (Gigabit PON)." },
  { "en": "Apa Itu ITU-T G.987?", "id": "Standar Untuk XG-PON (10G PON)." },
  { "en": "Apa Itu ITU-T G.8275.1?", "id": "Profil PTP (Precision Time Protocol) Telekomunikasi." },
  { "en": "Apa Itu Forward Channel (Satelit)?", "id": "Saluran Transmisi (Stasiun Bumi Ke Terminal)." },
  { "en": "Apa Itu Return Channel (Satelit)?", "id": "Saluran Transmisi (Terminal Ke Stasiun Bumi)." },
  { "en": "Apa Itu Scintillation (Propagasi)?", "id": "Fluktuasi Sinyal (Cepat, Akibat Ionosfer)." },
  { "en": "Apa Itu K-Factor (Propagasi Bumi)?", "id": "Faktor (Refraksi Atmosfer Efek Kelengkungan)." },
  { "en": "Apa Itu Model Okumura-Hata?", "id": "Model Propagasi (Area Urban, Empiris)." },
  { "en": "Apa Itu Model COST 231 Hata?", "id": "Ekstensi Model Hata (Frekuensi Tinggi)." },
  { "en": "Apa Itu Model Log-Distance Path Loss?", "id": "Model Path Loss (Logaritmik Jarak)." },
  { "en": "Apa Itu Model Two-Ray Ground-Reflection?", "id": "Model Propagasi (LOS Pantulan Tanah)." },
  { "en": "Apa Itu Difraksi (Gelombang)?", "id": "Pembelokan Gelombang (Sekitar Tepi Penghalang)." },
  { "en": "Apa Itu Scattering (Hamburan)?", "id": "Penyebaran Gelombang (Objek Kecil Acak)." },
  { "en": "Apa Itu Fading Margin?", "id": "Cadangan Daya (Mengatasi Efek Fading)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Atomic Aggregate?", "id": "Atribut (Menandakan Rute Agregat)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Aggregator?", "id": "Atribut (Identitas Router (Agregasi Rute))." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Originator ID?", "id": "Atribut (Mencegah Loop (Route Reflector))." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Cluster List?", "id": "Atribut (Mencegah Loop (Route Reflector))." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Weight?", "id": "Atribut (Lokal Router, Cisco, Tertinggi)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) MED (Multi-Exit Discriminator)?", "id": "Atribut (Jalur Masuk, Antar AS, Terendah)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Local Preference?", "id": "Atribut (Jalur Keluar, Internal AS, Tertinggi)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) AS_PATH?", "id": "Atribut (Daftar AS (Autonomous System) Dilewati)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Origin?", "id": "Atribut (Asal Rute: IGP, EGP)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Community No-Export?", "id": "Komunitas (Jangan Iklankan Keluar AS)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Community No-Advertise?", "id": "Komunitas (Jangan Iklankan Ke Peer)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Community Local-AS?", "id": "Komunitas (Jangan Iklankan Keluar Sub-Konfederasi)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Route Flap Dampening?", "id": "Mekanisme (Menstabilkan Rute Fluktuatif)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Multipath?", "id": "Load Balancing (Jalur BGP Sama Baik)." },
  { "en": "Apa Itu eBGP (External BGP) Multihop?", "id": "Sesi eBGP (External BGP) (Tidak Terhubung Langsung)." },
  { "en": "Apa Itu iBGP (Internal BGP) Full Mesh?", "id": "Setiap Router iBGP (Internal BGP) Peer." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Route Reflector?", "id": "Solusi Skalabilitas (Memantulkan Rute iBGP)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Address Family?", "id": "Kategori Protokol (IPv6, VPNv4)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) VPNv4 (Virtual Private Network Version 4) Address?", "id": "Alamat (Gabungan RD (Route Distinguisher) IPv4)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Next Hop Self?", "id": "Mengubah Next-Hop (Menjadi Diri Sendiri)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Soft Reconfiguration?", "id": "Update Kebijakan (Tanpa Reset Sesi)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Route Refresh?", "id": "Meminta Ulang Rute (Tanpa Reset)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Well-Known Mandatory Attribute?", "id": "Atribut (Wajib Ada, Dikenali Semua)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Well-Known Discretionary Attribute?", "id": "Atribut (Dikenali Semua, Opsional)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Optional Transitive Attribute?", "id": "Atribut (Opsional, Diteruskan Jika Tidak Dikenali)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Optional Non-Transitive Attribute?", "id": "Atribut (Opsional, Dibuang Jika Tidak Dikenali)." },
  { "en": "Apa Itu Stimulated Raman Scattering (SRS)?", "id": "Efek Non-Linear (Hamburan, Penguatan)." },
  { "en": "Apa Kepanjangan Dari SRS (Stimulated Raman Scattering)?", "id": "Stimulated Raman Scattering." },
  { "en": "Apa Itu Stimulated Brillouin Scattering (SBS)?", "id": "Efek Non-Linear (Hamburan, Pantulan)." },
  { "en": "Apa Kepanjangan Dari SBS (Stimulated Brillouin Scattering)?", "id": "Stimulated Brillouin Scattering." },
  { "en": "Apa Itu Optical Soliton?", "id": "Pulsa Optik (Menjaga Bentuk Jarak Jauh)." },
  { "en": "Apa Itu Dispersion-Shifted Fiber (DSF)?", "id": "Serat (Dispersi Nol Di 1550 nm)." },
  { "en": "Apa Kepanjangan Dari DSF (Dispersion-Shifted Fiber)?", "id": "Dispersion-Shifted Fiber." },
  { "en": "Apa Itu Non-Zero Dispersion-Shifted Fiber (NZ-DSF)?", "id": "Serat (Dispersi Kecil Di 1550 nm)." },
  { "en": "Apa Kepanjangan Dari NZ-DSF (Non-Zero Dispersion-Shifted Fiber)?", "id": "Non-Zero Dispersion-Shifted Fiber." },
  { "en": "Apa Itu Dispersion Compensation Fiber (DCF)?", "id": "Serat (Kompensasi Dispersi Kromatik)." },
  { "en": "Apa Itu Raman Amplifier?", "id": "Penguat Optik (Berbasis Efek Raman)." },
  { "en": "Apa Itu O-Band (Optical Band)?", "id": "Pita Optik (Original, 1260-1360 nm)." },
  { "en": "Apa Itu E-Band (Optical Band)?", "id": "Pita Optik (Extended, 1360-1460 nm)." },
  { "en": "Apa Itu S-Band (Optical Band)?", "id": "Pita Optik (Short, 1460-1530 nm)." },
  { "en": "Apa Itu U-Band (Optical Band)?", "id": "Pita Optik (Ultralong, 1625-1675 nm)." },
  { "en": "Apa Itu GLONASS?", "id": "Sistem Navigasi Satelit (Rusia)." },
  { "en": "Apa Kepanjangan Dari GLONASS (Global Navigation Satellite System)?", "id": "Global Navigation Satellite System." },
  { "en": "Apa Itu Galileo (Sistem Navigasi)?", "id": "Sistem Navigasi Satelit (Uni Eropa)." },
  { "en": "Apa Itu BeiDou (Sistem Navigasi)?", "id": "Sistem Navigasi Satelit (Tiongkok)." },
  { "en": "Apa Itu GNSS (Global Navigation Satellite System)?", "id": "Istilah Umum (Sistem Navigasi Satelit)." },
  { "en": "Apa Kepanjangan Dari GNSS (Global Navigation Satellite System)?", "id": "Global Navigation Satellite System." },
  { "en": "Apa Itu Frekuensi L1 (GPS)?", "id": "Frekuensi Sinyal GPS (Sipil C/A)." },
  { "en": "Apa Itu Frekuensi L2 (GPS)?", "id": "Frekuensi Sinyal GPS (Militer P(Y))." },
  { "en": "Apa Itu Frekuensi L5 (GPS)?", "id": "Frekuensi Sinyal GPS (Sipil Modern)." },
  { "en": "Apa Itu Kode C/A (GPS)?", "id": "Kode (Coarse/Acquisition, Sipil)." },
  { "en": "Apa Itu Kode P(Y) (GPS)?", "id": "Kode (Precise, Militer, Terenkripsi)." },
  { "en": "Apa Itu GPS (Global Positioning System) Almanac?", "id": "Data (Informasi Orbit Kasar Satelit)." },
  { "en": "Apa Itu GPS (Global Positioning System) Ephemeris?", "id": "Data (Informasi Orbit Detail Satelit)." },
  { "en": "Apa Itu Differential GPS (DGPS)?", "id": "GPS (Global Positioning System) (Koreksi Stasiun Bumi)." },
  { "en": "Apa Kepanjangan Dari DGPS (Differential GPS)?", "id": "Differential Global Positioning System." },
  { "en": "Apa Itu Wide Area Augmentation System (WAAS)?", "id": "Augmentasi GPS (Global Positioning System) (Amerika)." },
  { "en": "Apa Kepanjangan Dari WAAS (Wide Area Augmentation System)?", "id": "Wide Area Augmentation System." },
  { "en": "Apa Itu IKEv1 (Internet Key Exchange v1)?", "id": "Protokol Negosiasi Kunci IPsec (Versi 1)." },
  { "en": "Apa Itu IKEv2 (Internet Key Exchange v2)?", "id": "Protokol Negosiasi Kunci IPsec (Versi 2)." },
  { "en": "Apa Itu IKEv1 (Internet Key Exchange v1) Main Mode?", "id": "Mode IKEv1 (Internet Key Exchange v1) (6 Pesan)." },
  { "en": "Apa Itu IKEv1 (Internet Key Exchange v1) Aggressive Mode?", "id": "Mode IKEv1 (Internet Key Exchange v1) (3 Pesan)." },
  { "en": "Apa Itu IKE (Internet Key Exchange) Phase 1?", "id": "Membangun Tunnel (Channel) Aman (ISAKMP SA)." },
  { "en": "Apa Itu IKE (Internet Key Exchange) Phase 2?", "id": "Membangun Tunnel (Channel) Data (IPsec SA)." },
  { "en": "Apa Itu ISAKMP (Internet Security Association and Key Management Protocol)?", "id": "Kerangka Kerja (Manajemen Kunci IPsec)." },
  { "en": "Apa Kepanjangan Dari ISAKMP (Internet Security Association and Key Management Protocol)?", "id": "Internet Security Association Key Management Protocol." },
  { "en": "Apa Itu Oakley Protocol?", "id": "Protokol Pertukaran Kunci (Bagian IKE)." },
  { "en": "Apa Itu SKEME Protocol?", "id": "Protokol Pertukaran Kunci (Bagian IKE)." },
  { "en": "Apa Itu IPsec (Internet Protocol Security) AH (Authentication Header)?", "id": "Protokol IPsec (Internet Protocol Security) (Autentikasi)." },
  { "en": "Apa Itu IPsec (Internet Protocol Security) ESP (Encapsulating Security Payload)?", "id": "Protokol IPsec (Internet Protocol Security) (Enkripsi)." },
  { "en": "Apa Itu IPsec (Internet Protocol Security) Transport Mode?", "id": "Mode IPsec (Internet Protocol Security) (Enkripsi Payload)." },
  { "en": "Apa Itu IPsec (Internet Protocol Security) Tunnel Mode?", "id": "Mode IPsec (Internet Protocol Security) (Enkripsi Paket Utuh)." },
  { "en": "Apa Itu Public Safety Communication?", "id": "Komunikasi (Keamanan Publik, Darurat)." },
  { "en": "Apa Itu Land Mobile Radio (LMR)?", "id": "Sistem Radio (Dua Arah, Bergerak)." },
  { "en": "Apa Itu Mode Simplex (Radio)?", "id": "Komunikasi (Satu Frekuensi, Bergantian)." },
  { "en": "Apa Itu Mode Duplex (Radio)?", "id": "Komunikasi (Dua Frekuensi, Bersamaan)." },
  { "en": "Apa Itu Push-to-Talk (PTT)?", "id": "Tombol (Bicara Di Radio Dua Arah)." },
  { "en": "Apa Itu Repeater (Radio)?", "id": "Perangkat (Penerima Pemancar Ulang Sinyal)." },
  { "en": "Tujuan Repeater Radio?", "id": "Memperluas Jangkauan Sinyal Radio." },
  { "en": "Apa Itu Radio Trunking?", "id": "Sistem (Berbagi Saluran Radio Otomatis)." },
  { "en": "Keuntungan Radio Trunking?", "id": "Efisiensi Penggunaan Spektrum Frekuensi." },
  { "en": "Apa Itu Logic Trunked Radio (LTR)?", "id": "Standar Sistem Radio Trunking Analog." },
  { "en": "Apa Kepanjangan Dari LTR (Logic Trunked Radio)?", "id": "Logic Trunked Radio." },
  { "en": "Apa Itu APCO (Association of Public-Safety Communications Officials)?", "id": "Organisasi (Standar Komunikasi Publik)." },
  { "en": "Apa Kepanjangan Dari APCO (Association of Public-Safety Communications Officials)?", "id": "Association of Public-Safety Communications Officials." },
  { "en": "Apa Itu APCO (Association of Public-Safety Communications Officials) Project 25 (P25)?", "id": "Standar Radio Digital (Public Safety)." },
  { "en": "Apa Itu Radio Over IP (RoIP)?", "id": "Mengirim Sinyal Radio (Jaringan IP)." },
  { "en": "Apa Itu Interoperabilitas (Radio)?", "id": "Kemampuan (Sistem Berbeda Berkomunikasi)." },
  { "en": "Apa Itu Penyiaran (Broadcasting)?", "id": "Transmisi Sinyal (Satu Ke Banyak)." },
  { "en": "Apa Itu Studio (Penyiaran)?", "id": "Tempat Produksi Konten Siaran." },
  { "en": "Apa Itu Master Control Room (MCR)?", "id": "Ruang Kontrol Utama (Penyiaran)." },
  { "en": "Apa Kepanjangan Dari MCR (Master Control Room)?", "id": "Master Control Room." },
  { "en": "Apa Itu Transmitter (Pemancar)?", "id": "Perangkat (Memancarkan Sinyal RF Kuat)." },
  { "en": "Apa Itu Antena Pemancar (Broadcast)?", "id": "Antena (Memancarkan Sinyal Ke Area Luas)." },
  { "en": "Apa Itu Modulasi AM (Amplitude Modulation)?", "id": "Modulasi (Informasi Pada Amplitudo)." },
  { "en": "Pita Frekuensi Apa (Siaran AM)?", "id": "Medium Frequency (MF)." },
  { "en": "Apa Itu Modulasi FM (Frequency Modulation)?", "id": "Modulasi (Informasi Pada Frekuensi)." },
  { "en": "Pita Frekuensi Apa (Siaran FM)?", "id": "Very High Frequency (VHF)." },
  { "en": "Apa Itu Stereophonic FM (FM Stereo)?", "id": "Siaran FM (Dua Saluran Audio)." },
  { "en": "Apa Itu Radio Data System (RDS)?", "id": "Data Teks (Dikirim Sinyal FM)." },
  { "en": "Apa Itu Sinyal Luminance (Video)?", "id": "Informasi Kecerahan (Hitam Putih) Video." },
  { "en": "Apa Itu Sinyal Chrominance (Video)?", "id": "Informasi Warna (Merah, Biru) Video." },
  { "en": "Apa Itu Sinyal Video Komposit?", "id": "Sinyal (Luminance Chrominance Digabung)." },
  { "en": "Apa Itu Sinyal Video Komponen?", "id": "Sinyal (Luminance Chrominance Terpisah)." },
  { "en": "Apa Itu Digital Video Broadcasting (DVB)?", "id": "Standar TV Digital (Eropa)." },
  { "en": "Apa Itu DVB-T (Digital Video Broadcasting-Terrestrial)?", "id": "Standar DVB (Digital Video Broadcasting) (Siaran Darat)." },
  { "en": "Apa Itu DVB-S (Digital Video Broadcasting-Satellite)?", "id": "Standar DVB (Digital Video Broadcasting) (Siaran Satelit)." },
  { "en": "Apa Itu DVB-C (Digital Video Broadcasting-Cable)?", "id": "Standar DVB (Digital Video Broadcasting) (Siaran Kabel)." },
  { "en": "Apa Itu Polarisasi (Antena)?", "id": "Orientasi Arah Medan Listrik (Gelombang)." },
  { "en": "Apa Itu Polarisasi Vertikal?", "id": "Medan Listrik (Tegak Lurus Bumi)." },
  { "en": "Apa Itu Polarisasi Horizontal?", "id": "Medan Listrik (Sejajar Bumi)." },
  { "en": "Apa Itu Polarisasi Circular?", "id": "Medan Listrik (Berputar Saat Merambat)." },
  { "en": "Kapan Polarisasi Circular Digunakan?", "id": "Komunikasi Satelit (Mengurangi Efek Putaran)." },
  { "en": "Apa Itu Antenna Array?", "id": "Susunan (Gabungan Beberapa Elemen Antena)." },
  { "en": "Apa Itu Phased Array Antenna?", "id": "Antena Array (Arah Sinar Diatur Fasa)." },
  { "en": "Apa Itu Beamforming?", "id": "Teknik (Mengarahkan Sinyal Ke Pengguna)." },
  { "en": "Apa Itu Antena Sektoral?", "id": "Antena (Mencakup Sektor Tertentu, 120°)." },
  { "en": "Dimana Antena Sektoral Digunakan?", "id": "Menara BTS (Base Transceiver Station) Seluler." },
  { "en": "Apa Itu Antena Dipole?", "id": "Antena Dasar (Dua Konduktor Lurus)." },
  { "en": "Apa Itu Antena Monopole?", "id": "Antena (Satu Konduktor, Memerlukan Ground)." },
  { "en": "Apa Itu Ground Plane (Antena)?", "id": "Permukaan Konduktif (Reflektor Monopole)." },
  { "en": "Apa Itu Antena Yagi-Uda?", "id": "Antena Directional (Elemen Direktor Reflektor)." },
  { "en": "Apa Itu Antena Parabola?", "id": "Antena (Reflektor Bentuk Piringan Parabola)." },
  { "en": "Apa Itu Feedhorn (Antena Parabola)?", "id": "Komponen (Penerima Pemancar Di Fokus)." },
  { "en": "Apa Itu Antena Patch (Microstrip)?", "id": "Antena Datar (Dicetak Di Papan PCB)." },
  { "en": "Apa Itu Impedansi Antena?", "id": "Hambatan Antena (Pada Frekuensi Resonansi)." },
  { "en": "Apa Itu Standing Wave Ratio (SWR)?", "id": "Rasio Gelombang Berdiri (Kecocokan Impedansi)." },
  { "en": "Apa Itu Nilai SWR (Standing Wave Ratio) Ideal?", "id": "1:1 (Satu Banding Satu)." },
  { "en": "Apa Itu Impedance Matching (Pencocokan Impedansi)?", "id": "Menyamakan Impedansi (Sumber Ke Beban)." },
  { "en": "Apa Itu Return Loss (Rugi Pantul)?", "id": "Ukuran Daya (Dipantulkan Kembali Ke Sumber)." },
  { "en": "Apa Itu Balun?", "id": "Transformator (Balanced Ke Unbalanced)." },
  { "en": "Apa Itu Model FCAPS (ISO)?", "id": "Kerangka Kerja Manajemen Jaringan (ISO)." },
  { "en": "Apa Itu Fault Management (FCAPS)?", "id": "Manajemen (Deteksi Isolasi Kesalahan)." },
  { "en": "Apa Itu Configuration Management (FCAPS)?", "id": "Manajemen (Konfigurasi Perangkat)." },
  { "en": "Apa Itu Accounting Management (FCAPS)?", "id": "Manajemen (Pelacakan Penggunaan Sumber Daya)." },
  { "en": "Apa Itu Performance Management (FCAPS)?", "id": "Manajemen (Pemantauan Kinerja Jaringan)." },
  { "en": "Apa Itu Security Management (FCAPS)?", "id": "Manajemen (Kontrol Akses Keamanan)." },
  { "en": "Apa Itu Trouble Ticket?", "id": "Catatan Resmi (Insiden Atau Permintaan)." },
  { "en": "Apa Itu Eskalasi (Insiden)?", "id": "Meningkatkan Insiden (Ke Level Dukungan Tinggi)." },
  { "en": "Apa Itu Root Cause Analysis (RCA)?", "id": "Analisis (Mencari Akar Penyebab Masalah)." },
  { "en": "Apa Itu Network Baseline?", "id": "Ukuran (Kinerja Jaringan Normal)." },
  { "en": "Apa Itu Bottleneck (Leher Botol) Jaringan?", "id": "Titik (Kepadatan Yang Memperlambat Jaringan)." },
  { "en": "Apa Itu Logical Channel (Kanal Logis) (Seluler)?", "id": "Kanal (Definisi Jenis Informasi Ditransfer)." },
  { "en": "Apa Itu Transport Channel (Kanal Transport) (Seluler)?", "id": "Kanal (Definisi Cara Data Ditransfer)." },
  { "en": "Apa Itu Physical Channel (Kanal Fisik) (Seluler)?", "id": "Kanal (Lokasi Fisik Sumber Daya Radio)." },
  { "en": "Apa Itu Broadcast Control Channel (BCCH)?", "id": "Kanal Logis (Informasi Sistem Broadcast)." },
  { "en": "Apa Kepanjangan Dari BCCH (Broadcast Control Channel)?", "id": "Broadcast Control Channel." },
  { "en": "Apa Itu Paging Control Channel (PCCH)?", "id": "Kanal Logis (Informasi Paging Perangkat)." },
  { "en": "Apa Kepanjangan Dari PCCH (Paging Control Channel)?", "id": "Paging Control Channel." },
  { "en": "Apa Itu Common Control Channel (CCCH)?", "id": "Kanal Logis (Kontrol Umum Bersama)." },
  { "en": "Apa Kepanjangan Dari CCCH (Common Control Channel)?", "id": "Common Control Channel." },
  { "en": "Apa Itu Dedicated Control Channel (DCCH)?", "id": "Kanal Logis (Kontrol Khusus Perangkat)." },
  { "en": "Apa Kepanjangan Dari DCCH (Dedicated Control Channel)?", "id": "Dedicated Control Channel." },
  { "en": "Apa Itu Dedicated Traffic Channel (DTCH)?", "id": "Kanal Logis (Data Pengguna Khusus)." },
  { "en": "Apa Kepanjangan Dari DTCH (Dedicated Traffic Channel)?", "id": "Dedicated Traffic Channel." },
  { "en": "Apa Itu Broadcast Channel (BCH) (Transport)?", "id": "Kanal Transport (Membawa Informasi BCCH)." },
  { "en": "Apa Kepanjangan Dari BCH (Broadcast Channel)?", "id": "Broadcast Channel." },
  { "en": "Apa Itu Paging Channel (PCH) (Transport)?", "id": "Kanal Transport (Membawa Informasi PCCH)." },
  { "en": "Apa Kepanjangan Dari PCH (Paging Channel)?", "id": "Paging Channel." },
  { "en": "Apa Itu Downlink Shared Channel (DL-SCH)?", "id": "Kanal Transport (Data Downlink Bersama)." },
  { "en": "Apa Kepanjangan Dari DL-SCH (Downlink Shared Channel)?", "id": "Downlink Shared Channel." },
  { "en": "Apa Itu Uplink Shared Channel (UL-SCH)?", "id": "Kanal Transport (Data Uplink Bersama)." },
  { "en": "Apa Kepanjangan Dari UL-SCH (Uplink Shared Channel)?", "id": "Uplink Shared Channel." },
  { "en": "Apa Itu Random Access Channel (RACH)?", "id": "Kanal Transport (Akses Awal Acak)." },
  { "en": "Apa Kepanjangan Dari RACH (Random Access Channel)?", "id": "Random Access Channel." },
  { "en": "Apa Itu Physical Downlink Shared Channel (PDSCH)?", "id": "Kanal Fisik (Data Downlink Pengguna)." },
  { "en": "Apa Kepanjangan Dari PDSCH (Physical Downlink Shared Channel)?", "id": "Physical Downlink Shared Channel." },
  { "en": "Apa Itu Physical Uplink Shared Channel (PUSCH)?", "id": "Kanal Fisik (Data Uplink Pengguna)." },
  { "en": "Apa Kepanjangan Dari PUSCH (Physical Uplink Shared Channel)?", "id": "Physical Uplink Shared Channel." },
  { "en": "Apa Itu Physical Downlink Control Channel (PDCCH)?", "id": "Kanal Fisik (Kontrol Downlink)." },
  { "en": "Apa Kepanjangan Dari PDCCH (Physical Downlink Control Channel)?", "id": "Physical Downlink Control Channel." },
  { "en": "Apa Itu Physical Uplink Control Channel (PUCCH)?", "id": "Kanal Fisik (Kontrol Uplink)." },
  { "en": "Apa Kepanjangan Dari PUCCH (Physical Uplink Control Channel)?", "id": "Physical Uplink Control Channel." },
  { "en": "Apa Itu Physical Broadcast Channel (PBCH)?", "id": "Kanal Fisik (Informasi Broadcast Utama)." },
  { "en": "Apa Kepanjangan Dari PBCH (Physical Broadcast Channel)?", "id": "Physical Broadcast Channel." },
  { "en": "Apa Itu Physical Random Access Channel (PRACH)?", "id": "Kanal Fisik (Akses Acak)." },
  { "en": "Apa Kepanjangan Dari PRACH (Physical Random Access Channel)?", "id": "Physical Random Access Channel." },
  { "en": "Apa Itu Cell ID (Cell Identity)?", "id": "Identitas Unik Sel (Dalam Jaringan)." },
  { "en": "Apa Itu Physical Cell ID (PCI)?", "id": "Identitas Fisik Sel (Deteksi Sinyal)." },
  { "en": "Apa Kepanjangan Dari PCI (Physical Cell ID)?", "id": "Physical Cell ID." },
  { "en": "Apa Itu Tracking Area (TA) (LTE)?", "id": "Area Pelacakan Lokasi Perangkat (LTE)." },
  { "en": "Apa Kepanjangan Dari TA (Tracking Area)?", "id": "Tracking Area." },
  { "en": "Apa Itu Tracking Area Update (TAU)?", "id": "Proses Update Lokasi (Pindah TA)." },
  { "en": "Apa Kepanjangan Dari TAU (Tracking Area Update)?", "id": "Tracking Area Update." },
  { "en": "Apa Itu Location Area (LA) (GSM)?", "id": "Area Pelacakan (Jaringan GSM)." },
  { "en": "Apa Kepanjangan Dari LA (Location Area)?", "id": "Location Area." },
  { "en": "Apa Itu Routing Area (RA) (GPRS)?", "id": "Area Pelacakan (Jaringan GPRS)." },
  { "en": "Apa Kepanjangan Dari RA (Routing Area)?", "id": "Routing Area." },
  { "en": "Apa Itu Master Information Block (MIB) (Seluler)?", "id": "Informasi Sistem Paling Penting (PBCH)." },
  { "en": "Apa Kepanjangan Dari MIB (Master Information Block)?", "id": "Master Information Block." },
  { "en": "Apa Itu System Information Block (SIB) (Seluler)?", "id": "Informasi Sistem Detail (PDSCH)." },
  { "en": "Apa Kepanjangan Dari SIB (System Information Block)?", "id": "System Information Block." },
  { "en": "Apa Itu Packet Data Convergence Protocol (PDCP)?", "id": "Sub-Lapis (Header Kompresi, Enkripsi)." },
  { "en": "Apa Kepanjangan Dari PDCP (Packet Data Convergence Protocol)?", "id": "Packet Data Convergence Protocol." },
  { "en": "Fungsi PDCP (Packet Data Convergence Protocol)?", "id": "Kompresi Header, Enkripsi, Integritas." },
  { "en": "Apa Itu Radio Link Control (RLC)?", "id": "Sub-Lapis (Segmentasi, ARQ)." },
  { "en": "Apa Kepanjangan Dari RLC (Radio Link Control)?", "id": "Radio Link Control." },
  { "en": "Fungsi RLC (Radio Link Control)?", "id": "Segmentasi, Reassembly, Koreksi Error (ARQ)." },
  { "en": "Apa Itu RLC (Radio Link Control) Acknowledged Mode (AM)?", "id": "Mode RLC (Handal, Menggunakan ARQ)." },
  { "en": "Apa Itu RLC (Radio Link Control) Unacknowledged Mode (UM)?", "id": "Mode RLC (Tidak Handal, Cepat)." },
  { "en": "Apa Itu RLC (Radio Link Control) Transparent Mode (TM)?", "id": "Mode RLC (Tanpa Header, Transparan)." },
  { "en": "Apa Itu Medium Access Control (MAC) (Seluler)?", "id": "Sub-Lapis (Penjadwalan, Multiplexing)." },
  { "en": "Fungsi MAC (Medium Access Control) (Seluler)?", "id": "Penjadwalan Data, HARQ (Hybrid Automatic Repeat Request)." },
  { "en": "Apa Itu Physical Layer (PHY) (Seluler)?", "id": "Lapis Fisik (Encoding, Modulasi, RF)." },
  { "en": "Apa Itu Proxy-CSCF (P-CSCF) (IMS)?", "id": "Titik Kontak Pertama (Jaringan IMS)." },
  { "en": "Apa Kepanjangan Dari P-CSCF (Proxy-CSCF)?", "id": "Proxy-Call Session Control Function." },
  { "en": "Apa Fungsi P-CSCF (Proxy-CSCF)?", "id": "Meneruskan Sinyal, Keamanan (IPsec)." },
  { "en": "Apa Itu Interrogating-CSCF (I-CSCF) (IMS)?", "id": "Titik Kontak (Jaringan Asal Pengguna)." },
  { "en": "Apa Kepanjangan Dari I-CSCF (Interrogating-CSCF)?", "id": "Interrogating-Call Session Control Function." },
  { "en": "Apa Fungsi I-CSCF (Interrogating-CSCF)?", "id": "Mencari HSS (Home Subscriber Server), Memilih S-CSCF." },
  { "en": "Apa Itu Serving-CSCF (S-CSCF) (IMS)?", "id": "Node Pusat (Manajemen Sesi IMS)." },
  { "en": "Apa Kepanjangan Dari S-CSCF (Serving-CSCF)?", "id": "Serving-Call Session Control Function." },
  { "en": "Apa Fungsi S-CSCF (Serving-CSCF)?", "id": "Manajemen Sesi, Autentikasi Pengguna." },
  { "en": "Apa Itu Home Subscriber Server (HSS) (IMS)?", "id": "Database Induk (Profil Pengguna IMS)." },
  { "en": "Apa Itu Media Gateway Control Function (MGCF)?", "id": "Fungsi Kontrol (Gateway Ke PSTN)." },
  { "en": "Apa Kepanjangan Dari MGCF (Media Gateway Control Function)?", "id": "Media Gateway Control Function." },
  { "en": "Apa Itu Media Gateway (MGW)?", "id": "Gerbang (Konversi Media (IP Ke TDM))." },
  { "en": "Apa Kepanjangan Dari MGW (Media Gateway)?", "id": "Media Gateway." },
  { "en": "Apa Itu Protokol Diameter (Diameter Protocol)?", "id": "Protokol AAA (Authentication, Authorization, Accounting) (Pengganti RADIUS)." },
  { "en": "Fungsi Protokol Diameter?", "id": "Autentikasi, Otorisasi, Akunting (4G/5G)." },
  { "en": "Apa Itu Session Border Controller (SBC)?", "id": "Pengontrol Perbatasan Jaringan (Keamanan VoIP)." },
  { "en": "Apa Kepanjangan Dari SBC (Session Border Controller)?", "id": "Session Border Controller." },
  { "en": "Apa Fungsi SBC (Session Border Controller)?", "id": "Keamanan, Interoperabilitas, NAT Traversal." },
  { "en": "Apa Itu Message Session Relay Protocol (MSRP)?", "id": "Protokol (Transmisi Pesan Instan)." },
  { "en": "Apa Kepanjangan Dari MSRP (Message Session Relay Protocol)?", "id": "Message Session Relay Protocol." },
  { "en": "Apa Itu Rich Communication Services (RCS)?", "id": "Standar Komunikasi (Peningkatan SMS)." },
  { "en": "Apa Kepanjangan Dari RCS (Rich Communication Services)?", "id": "Rich Communication Services." },
  { "en": "Fitur RCS (Rich Communication Services)?", "id": "Chat Grup, Berbagi File, Status." },
  { "en": "Apa Itu ViLTE (Video over LTE)?", "id": "Layanan Panggilan Video (Jaringan LTE)." },
  { "en": "Apa Kepanjangan Dari ViLTE (Video over LTE)?", "id": "Video over Long-Term Evolution." },
  { "en": "Apa Itu Wi-Fi Calling (Panggilan Wi-Fi)?", "id": "Layanan Panggilan (Melalui Jaringan Wi-Fi)." },
  { "en": "Apa Nama Teknis Wi-Fi Calling?", "id": "Voice over Wi-Fi (VoWiFi)." },
  { "en": "Apa Itu Evolved Packet Data Gateway (ePDG)?", "id": "Gateway (Akses Jaringan Inti (Non-3GPP))." },
  { "en": "Fungsi ePDG (Evolved Packet Data Gateway)?", "id": "Menghubungkan VoWiFi (Voice over Wi-Fi) Ke EPC." },
  { "en": "Apa Itu Baseband Unit (BBU) (Seluler)?", "id": "Unit Pemrosesan Sinyal Baseband (BTS)." },
  { "en": "Apa Kepanjangan Dari BBU (Baseband Unit)?", "id": "Baseband Unit." },
  { "en": "Apa Itu Remote Radio Head (RRH)?", "id": "Unit Radio (Komponen RF Di Antena)." },
  { "en": "Apa Kepanjangan Dari RRH (Remote Radio Head)?", "id": "Remote Radio Head." },
  { "en": "Apa Itu Common Public Radio Interface (CPRI)?", "id": "Standar Antarmuka (Antara BBU RRH)." },
  { "en": "Apa Fungsi CPRI (Common Public Radio Interface)?", "id": "Mengirim Sinyal I/Q Digital." },
  { "en": "Apa Itu eCPRI (Evolved CPRI)?", "id": "CPRI (Common Public Radio Interface) (Lebih Efisien, 5G)." },
  { "en": "Apa Itu Small Cell (Sel Kecil)?", "id": "BTS (Base Transceiver Station) Mini (Daya Rendah)." },
  { "en": "Apa Itu Picocell?", "id": "Small Cell (Area Publik Internal)." },
  { "en": "Apa Itu Mode Simplex (Radio)?", "id": "Komunikasi Satu Frekuensi (Bergantian)." },
  { "en": "Apa Itu Mode Duplex (Radio)?", "id": "Komunikasi Dua Frekuensi (Bersamaan)." }



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
            }, 7000);
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
