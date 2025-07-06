# BAB II
# TINJAUAN PUSTAKA

## 2.1 Landasan Teori

### 2.1.1 Artificial Intelligence dalam Pendidikan

Artificial Intelligence (AI) atau kecerdasan buatan telah menjadi salah satu teknologi yang paling berpengaruh dalam transformasi digital berbagai sektor, termasuk pendidikan. Menurut Russell & Norvig (2020), AI adalah bidang ilmu komputer yang berfokus pada pembuatan mesin yang dapat melakukan tugas-tugas yang biasanya memerlukan kecerdasan manusia [1].

Dalam konteks pendidikan, AI memiliki potensi untuk merevolusi cara pembelajaran dan evaluasi dilakukan. Luckin et al. (2016) mengidentifikasi beberapa aplikasi AI dalam pendidikan, antara lain:

1. **Intelligent Tutoring Systems (ITS)**: Sistem yang dapat memberikan instruksi yang dipersonalisasi berdasarkan kebutuhan individual siswa [2].

2. **Automated Assessment**: Sistem penilaian otomatis yang dapat mengevaluasi jawaban siswa, termasuk essay dan soal terbuka [3].

3. **Question Generation**: Teknologi yang dapat menghasilkan pertanyaan secara otomatis berdasarkan materi pembelajaran [4].

4. **Learning Analytics**: Analisis data pembelajaran untuk memberikan insight tentang proses belajar mengajar [5].

#### 2.1.1.1 Natural Language Processing (NLP)

Natural Language Processing adalah cabang AI yang memungkinkan komputer untuk memahami, menafsirkan, dan menghasilkan bahasa manusia. Dalam konteks pembuatan soal otomatis, NLP berperan penting dalam:

- **Text Generation**: Menghasilkan teks soal yang koheren dan sesuai konteks
- **Content Analysis**: Menganalisis materi pembelajaran untuk mengidentifikasi konsep kunci
- **Language Understanding**: Memahami nuansa bahasa untuk menghasilkan soal dengan tingkat kesulitan yang tepat

Penelitian oleh Devlin et al. (2019) tentang BERT (Bidirectional Encoder Representations from Transformers) menunjukkan kemajuan signifikan dalam pemahaman bahasa natural oleh mesin [6].

#### 2.1.1.2 Machine Learning dalam Pembuatan Soal

Machine Learning (ML) memungkinkan sistem untuk belajar dari data dan meningkatkan performanya seiring waktu. Dalam pembuatan soal otomatis, ML digunakan untuk:

- **Pattern Recognition**: Mengenali pola dalam soal-soal berkualitas
- **Difficulty Estimation**: Memperkirakan tingkat kesulitan soal berdasarkan karakteristik tertentu
- **Quality Assessment**: Menilai kualitas soal yang dihasilkan

### 2.1.2 Sistem Manajemen Pembelajaran (Learning Management System)

Learning Management System (LMS) adalah platform perangkat lunak yang dirancang untuk mengelola, mendokumentasikan, melacak, melaporkan, dan menyampaikan program pendidikan. Menurut Coates et al. (2005), LMS yang efektif harus memiliki karakteristik berikut [7]:

1. **User Management**: Kemampuan mengelola berbagai peran pengguna
2. **Content Management**: Pengelolaan materi pembelajaran
3. **Assessment Tools**: Alat untuk membuat dan mengelola evaluasi
4. **Communication Tools**: Fasilitas komunikasi antara pengguna
5. **Tracking and Reporting**: Pelacakan dan pelaporan aktivitas pembelajaran

#### 2.1.2.1 Komponen Utama LMS

**Authentication dan Authorization**
Sistem keamanan yang memastikan hanya pengguna yang berwenang yang dapat mengakses sistem dengan hak akses yang sesuai.

**Content Delivery**
Mekanisme penyampaian materi pembelajaran dalam berbagai format seperti teks, video, audio, dan multimedia interaktif.

**Assessment and Testing**
Fitur untuk membuat, mendistribusikan, dan mengevaluasi berbagai jenis assessment seperti quiz, ujian, dan tugas.

**Progress Tracking**
Kemampuan untuk melacak kemajuan belajar siswa dan memberikan feedback yang relevan.

### 2.1.3 Teknologi Web Modern

#### 2.1.3.1 Frontend Development

**React.js**
React adalah library JavaScript yang dikembangkan oleh Facebook untuk membangun user interface yang interaktif. Keunggulan React meliputi [8]:

- **Component-based Architecture**: Memungkinkan pengembangan yang modular dan reusable
- **Virtual DOM**: Meningkatkan performa aplikasi
- **Rich Ecosystem**: Ekosistem yang kaya dengan berbagai library pendukung

**TypeScript**
TypeScript adalah superset dari JavaScript yang menambahkan static typing. Manfaat penggunaan TypeScript antara lain [9]:

- **Type Safety**: Mengurangi error saat runtime
- **Better IDE Support**: Dukungan IntelliSense yang lebih baik
- **Improved Code Quality**: Kode yang lebih maintainable dan readable

#### 2.1.3.2 Backend Development

**Node.js**
Node.js adalah runtime environment JavaScript yang memungkinkan eksekusi JavaScript di server. Keunggulan Node.js meliputi [10]:

- **Non-blocking I/O**: Performa yang tinggi untuk aplikasi real-time
- **Unified Language**: Menggunakan JavaScript di frontend dan backend
- **Large Package Ecosystem**: npm registry dengan jutaan package

**Express.js**
Express adalah web framework untuk Node.js yang minimal dan fleksibel. Fitur utama Express mencakup [11]:

- **Routing**: Sistem routing yang powerful
- **Middleware**: Dukungan middleware untuk berbagai keperluan
- **Template Engine**: Integrasi dengan berbagai template engine

#### 2.1.3.3 Database

**MongoDB**
MongoDB adalah database NoSQL yang menggunakan document-oriented data model. Keunggulan MongoDB antara lain [12]:

- **Flexible Schema**: Schema yang fleksibel dan dapat berubah
- **Horizontal Scaling**: Kemampuan scaling yang baik
- **Rich Query Language**: Bahasa query yang powerful

### 2.1.4 API Integration dan Microservices

#### 2.1.4.1 RESTful API

REST (Representational State Transfer) adalah arsitektur untuk sistem terdistribusi, khususnya web services. Prinsip-prinsip REST meliputi [13]:

- **Stateless**: Setiap request bersifat independen
- **Cacheable**: Response dapat di-cache untuk meningkatkan performa
- **Uniform Interface**: Interface yang konsisten
- **Layered System**: Arsitektur berlapis

#### 2.1.4.2 Third-party API Integration

Integrasi dengan API pihak ketiga memungkinkan aplikasi untuk memanfaatkan layanan eksternal. Dalam konteks ExamExpert AI, integrasi dengan Perplexity API memberikan kemampuan:

- **AI-powered Content Generation**: Pembuatan konten menggunakan AI
- **Natural Language Understanding**: Pemahaman bahasa natural
- **Contextual Response**: Respons yang sesuai konteks

## 2.2 Tinjauan Penelitian Terdahulu

### 2.2.1 Automatic Question Generation

Penelitian tentang automatic question generation telah berkembang pesat dalam dekade terakhir. Beberapa penelitian penting dalam bidang ini antara lain:

**Heilman & Smith (2010)** mengembangkan sistem untuk menghasilkan pertanyaan secara otomatis dari teks menggunakan teknik transformasi sintaksis. Penelitian ini menunjukkan bahwa sistem otomatis dapat menghasilkan pertanyaan dengan kualitas yang sebanding dengan pertanyaan buatan manusia [14].

**Du et al. (2017)** mengusulkan pendekatan menggunakan attention-based sequence-to-sequence model untuk question generation. Model ini mampu menghasilkan pertanyaan yang lebih natural dan contextually relevant [15].

**Zhao et al. (2018)** mengembangkan sistem yang dapat menghasilkan berbagai jenis pertanyaan (factual, inferential, dan evaluative) dengan menggunakan neural networks. Penelitian ini menunjukkan pentingnya diversifikasi jenis pertanyaan untuk evaluasi yang komprehensif [16].

### 2.2.2 AI dalam E-Learning Systems

**Wang et al. (2019)** melakukan studi komprehensif tentang penerapan AI dalam e-learning systems. Penelitian ini mengidentifikasi bahwa AI dapat meningkatkan personalisasi pembelajaran dan efektivitas assessment [17].

**Zhang & Aslan (2021)** mengembangkan sistem intelligent tutoring yang mengintegrasikan AI untuk adaptive assessment. Hasil penelitian menunjukkan peningkatan signifikan dalam learning outcomes siswa [18].

**Kumar et al. (2020)** meneliti penggunaan AI untuk automated essay scoring dan menunjukkan bahwa sistem AI dapat memberikan penilaian yang konsisten dan objektif [19].

### 2.2.3 Learning Management Systems

**Alias & Zainuddin (2005)** melakukan evaluasi terhadap berbagai LMS dan mengidentifikasi faktor-faktor kunci yang mempengaruhi efektivitas LMS dalam lingkungan pendidikan [20].

**Mtebe & Raphael (2018)** meneliti adopsi LMS di institusi pendidikan tinggi dan menemukan bahwa kemudahan penggunaan dan utilitas yang dirasakan adalah faktor utama dalam adopsi LMS [21].

**Al-Busaidi & Al-Shihi (2012)** mengembangkan model untuk mengevaluasi kualitas LMS dari perspektif pengguna, dengan fokus pada aspek fungsionalitas, kegunaan, dan kepuasan pengguna [22].

### 2.2.4 Gap Analysis

Berdasarkan tinjauan penelitian terdahulu, dapat diidentifikasi beberapa gap yang menjadi peluang untuk penelitian ini:

1. **Limited Integration**: Kebanyakan penelitian fokus pada satu aspek saja (question generation atau LMS), belum ada yang mengintegrasikan keduanya secara komprehensif.

2. **Language Support**: Sebagian besar penelitian menggunakan bahasa Inggris, sementara penelitian untuk bahasa Indonesia masih terbatas.

3. **Real-world Implementation**: Banyak penelitian yang bersifat teoretis atau prototype, belum ada implementasi yang siap digunakan dalam lingkungan pendidikan nyata.

4. **Multiple Question Types**: Sistem yang dapat menghasilkan berbagai jenis soal (pilihan ganda, essay, true/false) secara terintegrasi masih jarang ditemukan.

5. **User Experience**: Fokus pada aspek teknis AI seringkali mengabaikan aspek user experience dan praktikalitas penggunaan.

## 2.3 Kerangka Pemikiran

Berdasarkan tinjauan pustaka yang telah dilakukan, kerangka pemikiran penelitian ini dapat digambarkan sebagai berikut:

```
Input:
- Topik pembelajaran
- Jenis soal yang diinginkan
- Tingkat kesulitan
- Jumlah soal

Proses:
AI Question Generation
↓
Content Validation
↓
Question Bank Management
↓
Exam Creation & Delivery
↓
Automated Assessment
↓
Analytics & Reporting

Output:
- Soal berkualitas
- Ujian online yang efisien
- Analisis hasil pembelajaran
- Laporan komprehensif
```

Kerangka pemikiran ini menunjukkan alur proses dari input pengguna hingga output yang dihasilkan oleh sistem ExamExpert AI. Setiap tahap dalam proses ini didukung oleh teknologi dan metodologi yang telah dibahas dalam tinjauan pustaka.

## 2.4 Hipotesis Penelitian

Berdasarkan landasan teori dan tinjauan penelitian terdahulu, dapat dirumuskan hipotesis penelitian sebagai berikut:

**Hipotesis 1**: Sistem ExamExpert AI yang mengintegrasikan teknologi AI untuk pembuatan soal otomatis dapat menghasilkan soal dengan kualitas yang sebanding dengan soal buatan manusia dalam waktu yang lebih singkat.

**Hipotesis 2**: Implementasi sistem manajemen ujian yang komprehensif dapat meningkatkan efisiensi proses evaluasi pembelajaran dibandingkan dengan metode konvensional.

**Hipotesis 3**: Penggunaan teknologi web modern (React.js, Node.js, MongoDB) dapat menghasilkan sistem yang scalable, maintainable, dan user-friendly.

---

## Referensi

[1] Russell, S., & Norvig, P. (2020). *Artificial Intelligence: A Modern Approach* (4th ed.). Pearson.

[2] Luckin, R., Holmes, W., Griffiths, M., & Forcier, L. B. (2016). *Intelligence Unleashed: An Argument for AI in Education*. Pearson.

[3] Burrows, S., Gurevych, I., & Stein, B. (2015). The eras and trends of automatic short answer grading. *International Journal of Artificial Intelligence in Education*, 25(1), 60-117.

[4] Kurdi, G., Leo, J., Parsia, B., Sattler, U., & Al-Emari, S. (2020). A systematic review of automatic question generation for educational purposes. *International Journal of Artificial Intelligence in Education*, 30(1), 121-204.

[5] Siemens, G., & Long, P. (2011). Penetrating the fog: Analytics in learning and education. *EDUCAUSE Review*, 46(5), 30-32.

[6] Devlin, J., Chang, M. W., Lee, K., & Toutanova, K. (2019). BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding. *Proceedings of NAACL-HLT*, 4171-4186.

[7] Coates, H., James, R., & Baldwin, G. (2005). A critical examination of the effects of learning management systems on university teaching and learning. *Tertiary Education and Management*, 11(1), 19-36.

[8] Gackenheimer, C. (2015). *Introduction to React*. Apress.

[9] Cherny, B. (2019). *Programming TypeScript: Making Your JavaScript Applications Scale*. O'Reilly Media.

[10] Hughes-Croucher, T., & Wilson, M. (2012). *Node: Up and Running: Scalable Server-Side JavaScript with Node.js*. O'Reilly Media.

[11] Brown, E. (2019). *Web Development with Node and Express: Leveraging the JavaScript Stack*. O'Reilly Media.

[12] Chodorow, K. (2013). *MongoDB: The Definitive Guide*. O'Reilly Media.

[13] Fielding, R. T. (2000). *Architectural Styles and the Design of Network-based Software Architectures* (Doctoral dissertation). University of California, Irvine.

[14] Heilman, M., & Smith, N. A. (2010). Good question! Statistical ranking for question generation. *Proceedings of the 2010 Annual Conference of the North American Chapter of the Association for Computational Linguistics*, 609-617.

[15] Du, X., Shao, J., & Cardie, C. (2017). Learning to ask: Neural question generation for reading comprehension. *Proceedings of the 55th Annual Meeting of the Association for Computational Linguistics*, 1342-1352.

[16] Zhao, Y., Ni, X., Ding, Y., & Ke, Q. (2018). Paragraph-level neural question generation with maxout pointer and gated self-attention networks. *Proceedings of the 2018 Conference on Empirical Methods in Natural Language Processing*, 3901-3910.

[17] Wang, Y., Liu, C., & Tu, X. (2019). AI-enabled e-learning systems: A systematic review. *Computers & Education*, 142, 103645.

[18] Zhang, K., & Aslan, A. B. (2021). AI technologies for education: Recent research & future directions. *Computers and Education: Artificial Intelligence*, 2, 100025.

[19] Kumar, V., Boulanger, D., Seanosky, J., Kinshuk, & Panneerselvam, K. (2020). Explainable automated essay scoring: Deep learning really has pedagogical value. *Frontiers in Education*, 5, 572367.

[20] Alias, N. A., & Zainuddin, A. M. (2005). Innovation for better teaching and learning: Adopting the learning management system. *Malaysian Online Journal of Instructional Technology*, 2(2), 27-40.

[21] Mtebe, J. S., & Raphael, C. (2018). Key factors affecting the adoption and use of learning management systems in higher education institutions in Tanzania. *International Journal of Education and Development Using Information and Communication Technology*, 14(2), 4-19.

[22] Al-Busaidi, K. A., & Al-Shihi, H. (2012). Key factors to instructors' satisfaction of learning management systems in blended learning. *Journal of Computing in Higher Education*, 24(1), 18-39.
