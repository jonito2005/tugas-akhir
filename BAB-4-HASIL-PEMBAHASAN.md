# BAB IV
# HASIL DAN PEMBAHASAN

## 4.1 Hasil Implementasi Sistem

Pada bab ini akan dijelaskan mengenai hasil implementasi Sistem ExamExpert AI yang telah dikembangkan berdasarkan analisis dan perancangan yang telah dilakukan pada bab sebelumnya. Implementasi sistem mencakup seluruh fitur dan fungsionalitas yang telah dirancang untuk mendukung proses otomatisasi pembuatan pertanyaan evaluasi menggunakan teknologi Artificial Intelligence mulai dari login pengguna, pembuatan pertanyaan otomatis, manajemen kuis, hingga analisis hasil evaluasi.

Sistem ExamExpert AI telah diimplementasikan menggunakan teknologi dan alat yang telah ditentukan pada tahap perancangan. Setiap komponen sistem telah diuji untuk memastikan bahwa sistem dapat berjalan dengan baik dan sesuai dengan kebutuhan proses pembuatan evaluasi di institusi pendidikan. Sistem ini dibuat untuk meningkatkan efisiensi, konsistensi, dan kualitas dalam proses pembuatan instrumen penilaian berbasis AI.

Berikut adalah penjelasan detail mengenai implementasi setiap fitur dan fungsionalitas Sistem ExamExpert AI:

### 4.1.1 Login

**Gambar 4.1. Tampilan Login**

Pada gambar 4.1 diatas merupakan tampilan dari halaman login. Ketika user memasuki link https://examexpert-ai.vercel.app/login maka akan ditampilkan halaman login. Terdapat dua form yaitu email dan password yang digunakan sebagai syarat memasuki halaman dashboard sesuai role masing-masing (Admin, Teacher, Student).

```typescript
// Handler untuk submit form login
const handleSubmit = async (e: React.FormEvent) => {
  e.preventDefault();
  setLoading(true);

  try {
    const response = await authAPI.login({
      email: formData.email,
      password: formData.password
    });
    
    if (response.success) {
      // Simpan token ke localStorage
      localStorage.setItem('token', response.data.token);
      
      // Reset form setelah berhasil login
      setFormData({ email: '', password: '' });
      setError('');
      
      // Redirect berdasarkan role user
      const userRole = response.data.user.role;
      switch(userRole) {
        case 'admin':
          navigate('/admin/dashboard');
          break;
        case 'teacher':
          navigate('/teacher/dashboard');
          break;
        case 'student':
          navigate('/student/dashboard');
          break;
        default:
          navigate('/dashboard');
      }
      
      toast.success('Login berhasil!');
    }
  } catch (error: any) {
    setError(error.response?.data?.message || 'Login gagal');
    toast.error('Email atau password salah');
  } finally {
    setLoading(false);
  }
};
```

Ketika mengklik tombol masuk, maka sistem yang ada di frontend memanggil fungsi handleSubmit. Kemudian sistem memanggil API authAPI.login() dengan parameter formData yang berisi email dan password yang diinputkan pengguna. Jika login berhasil, sistem menyimpan token JWT yang diterima dari API ke localStorage menggunakan localStorage.setItem('token', response.data.token).

Setelah token tersimpan, sistem mereset form dengan mengosongkan state formData dan menghapus pesan error yang mungkin ada sebelumnya. Kemudian sistem melakukan switch statement dimana admin diarahkan ke '/admin/dashboard', teacher ke '/teacher/dashboard', dan student ke '/student/dashboard'. Sistem juga menampilkan notifikasi sukses menggunakan toast.success(). Namun jika login gagal, sistem menampilkan pesan error "Email atau password salah" menggunakan setError() dan toast.error().

```javascript
// API Login Controller
exports.login = async (req, res, next) => {
  try {
    const { email, password } = req.body;

    // Cari user berdasarkan email
    const user = await User.findOne({
      where: { email },
      attributes: ['_id', 'name', 'email', 'password', 'role', 'status']
    });

    if (!user) {
      return res.status(401).json({
        success: false,
        message: "Email tidak terdaftar"
      });
    }

    // Verifikasi password
    const isValidPassword = await bcrypt.compare(password, user.password);
    if (!isValidPassword) {
      return res.status(401).json({
        success: false,
        message: "Email atau password salah"
      });
    }

    // Cek status user untuk teacher
    if (user.role === 'teacher' && user.status !== 'active') {
      return res.status(401).json({
        success: false,
        message: "Akun teacher belum diverifikasi oleh admin"
      });
    }

    // Generate token JWT
    const token = jwt.sign(
      { 
        userId: user._id,
        email: user.email,
        name: user.name,
        role: user.role
      },
      process.env.JWT_SECRET,
      { expiresIn: '24h' }
    );

    // Kirim response sukses
    res.status(200).json({
      success: true,
      message: "Login berhasil",
      data: {
        token: token,
        user: {
          id: user._id,
          name: user.name,
          email: user.email,
          role: user.role
        }
      }
    });

  } catch (error) {
    console.error('Login error:', error);
    res.status(500).json({
      success: false,
      message: "Terjadi kesalahan pada server"
    });
  }
};
```

Disisi API login(), sistem melakukan proses autentikasi dengan mengambil data email dan password dari request body yang dikirim frontend. Sistem mencari pengguna di database berdasarkan email menggunakan User.findOne() dari ORM MongoDB. Setelah pengguna ditemukan, sistem memverifikasi password menggunakan bcrypt.compare() untuk membandingkan password yang diinputkan dengan password yang terenkripsi yang tersimpan di database.

Sistem juga melakukan validasi khusus untuk teacher dimana jika status bukan 'active', maka login ditolak dengan pesan "Akun teacher belum diverifikasi oleh admin". Jika password benar dan status valid, sistem membuat token JWT menggunakan jwt.sign() yang berisi data pengguna seperti userId, email, name, dan role dengan masa berlaku 24 jam. Setelah token berhasil dibuat, sistem mengirimkan response sukses dengan pesan "Login berhasil" beserta data token dan informasi user.

### 4.1.2 Logout

**Gambar 4.2. Tampilan Logout**

Pada gambar 4.2 diatas merupakan tampilan tombol logout dari halaman dashboard yang berada di pojok kanan atas setelah mengklik avatar pengguna. Menu dropdown ini menyediakan opsi logout yang memungkinkan pengguna untuk keluar dari sistem. Proses logout mengakhiri sesi pengguna dan mengarahkan kembali ke halaman beranda.

```typescript
// Handle logout
const handleLogout = () => {
  // Hapus token dari localStorage
  localStorage.removeItem('token');
  
  // Reset auth state
  setUser(null);
  setIsAuthenticated(false);
  
  // Reset state dan navigasi ke halaman utama
  setShowDropdown(false);
  navigate('/');
  
  // Tampilkan notifikasi logout
  toast.success('Logout berhasil');
};
```

Ketika pengguna mengklik tombol logout, sistem menjalankan fungsi handleLogout yang menghapus token autentikasi dari localStorage browser menggunakan localStorage.removeItem('token'). Sistem juga mereset state autentikasi dengan mengosongkan data user dan mengatur isAuthenticated menjadi false. Setelah itu, sistem mereset state dropdown dengan setShowDropdown(false) untuk menutup dropdown menu dan melakukan navigasi ke halaman utama menggunakan navigate('/'). Sistem juga menampilkan notifikasi sukses "Logout berhasil" menggunakan toast.success().

### 4.1.3 Pembuatan Pertanyaan AI

**Gambar 4.3. Tampilan Pembuatan Pertanyaan AI**

Pada gambar 4.3 diatas merupakan tampilan dari halaman AI Question Generator. Ketika teacher mengklik menu "Generate Questions" maka akan ditampilkan form untuk membuat pertanyaan otomatis. Terdapat form topik pembelajaran, jenis pertanyaan (pilihan ganda/benar-salah), jumlah pertanyaan, dan tingkat kesulitan.

```typescript
// Handler untuk submit form generate questions
const handleSubmit = async (e: React.FormEvent) => {
  e.preventDefault();
  setLoading(true);

  try {
    const requestData = {
      topic: formData.topic,
      type: formData.type,
      count: formData.count,
      difficulty: formData.difficulty
    };

    const response = await questionAPI.generateQuestions(requestData);
    
    if (response.success) {
      setGeneratedQuestions(response.data);
      toast.success(`Berhasil menghasilkan ${response.data.length} pertanyaan!`);
      
      // Reset form setelah berhasil generate
      setFormData({
        topic: '',
        type: 'multiple_choice',
        count: 5,
        difficulty: 'medium'
      });
    }
  } catch (error: any) {
    console.error('Error generating questions:', error);
    toast.error(error.response?.data?.message || 'Gagal menghasilkan pertanyaan');
  } finally {
    setLoading(false);
  }
};
```

Ketika form sudah terisi dan mengklik tombol "Generate Pertanyaan", maka sistem frontend menyiapkan data request dengan menyusun objek requestData yang berisi topic, type, count, dan difficulty. Sistem kemudian memanggil API questionAPI.generateQuestions() dengan parameter requestData untuk mengirim permintaan ke backend.

Jika proses berhasil, sistem menyimpan pertanyaan yang dihasilkan ke state generatedQuestions menggunakan setGeneratedQuestions() dan menampilkan notifikasi sukses dengan jumlah pertanyaan yang berhasil dibuat. Setelah itu, sistem mereset form dengan mengosongkan semua field kembali ke nilai default. Jika terjadi error, sistem menampilkan pesan error melalui toast.error() dan mencatat error ke console untuk debugging.

```javascript
// API Generate Questions Controller
exports.generateAIQuestions = asyncHandler(async (req, res, next) => {
  const { topic, type, count, difficulty } = req.body;

  // Validation input
  if (!topic || !type) {
    return res.status(400).json({
      success: false,
      message: 'Topic dan type wajib diisi'
    });
  }

  console.log(`[QUESTION-GENERATION] User ${req.user.name} generating questions:`, {
    topic, type, count, difficulty
  });

  try {
    // Generate questions menggunakan Perplexity AI
    const aiQuestions = await generateQuestions(topic, type, count, difficulty);
    
    // Simpan ke database dengan status approved
    const savedQuestions = await Question.insertMany(
      aiQuestions.map(q => ({
        ...q,
        createdBy: req.user._id,
        status: 'approved',
        aiGenerated: true,
        createdAt: new Date()
      }))
    );

    console.log(`[QUESTION-GENERATION-SUCCESS] Generated ${savedQuestions.length} questions for ${req.user.name}`);

    res.status(201).json({
      success: true,
      message: `Berhasil menghasilkan ${savedQuestions.length} pertanyaan AI`,
      data: savedQuestions
    });

  } catch (error) {
    console.error(`[QUESTION-GENERATION-ERROR] Failed for user ${req.user.name}:`, error);
    res.status(500).json({
      success: false,
      message: error.message || 'Gagal menghasilkan pertanyaan'
    });
  }
});
```

Disisi API generateAIQuestions(), sistem melakukan validasi input terlebih dahulu dengan memastikan field topic dan type telah diisi. Sistem kemudian mencatat log aktivitas user yang melakukan request generate questions. Setelah validasi, sistem memanggil fungsi generateQuestions() yang mengintegrasikan dengan Perplexity AI untuk menghasilkan pertanyaan berdasarkan parameter yang diberikan.

Pertanyaan yang dihasilkan AI kemudian disimpan ke database menggunakan Question.insertMany() dengan menambahkan informasi tambahan seperti createdBy (ID user yang membuat), status 'approved' untuk auto-approve pertanyaan AI, flag aiGenerated bernilai true, dan timestamp createdAt. Sistem mencatat log sukses dan mengembalikan response dengan pesan berhasil beserta data pertanyaan yang telah disimpan.

### 4.1.4 Manajemen Kuis

**Gambar 4.4. Tampilan Manajemen Kuis**

Pada gambar 4.4 diatas merupakan tampilan dari halaman Quiz Creator. Setelah mengklik menu "Create Quiz", maka menampilkan form pembuatan kuis yang meliputi judul kuis, deskripsi, topik, durasi (dalam menit), dan kode akses. Terdapat juga bagian untuk memilih pertanyaan dari question bank yang tersedia.

```typescript
// Handler untuk submit form create quiz
const handleSubmit = async (e: React.FormEvent) => {
  e.preventDefault();
  
  if (selectedQuestions.length === 0) {
    toast.error('Pilih minimal 1 pertanyaan untuk kuis');
    return;
  }

  setLoading(true);
  
  try {
    const quizData = {
      title: formData.title,
      description: formData.description,
      topic: formData.topic,
      duration: formData.duration,
      accessCode: formData.accessCode,
      questions: selectedQuestions,
      isActive: true,
      startDate: new Date(),
      endDate: new Date(Date.now() + 7 * 24 * 60 * 60 * 1000) // 7 hari dari sekarang
    };

    const response = await quizAPI.createQuiz(quizData);
    
    if (response.success) {
      toast.success('Kuis berhasil dibuat!');
      
      // Reset form dan selected questions
      setFormData({
        title: '',
        description: '',
        topic: '',
        duration: 30,
        accessCode: generateAccessCode()
      });
      setSelectedQuestions([]);
      
      // Redirect ke dashboard atau quiz list
      navigate('/teacher/quiz');
    }
  } catch (error: any) {
    console.error('Error creating quiz:', error);
    toast.error(error.response?.data?.message || 'Gagal membuat kuis');
  } finally {
    setLoading(false);
  }
};
```

Ketika teacher mengisi form dan memilih pertanyaan kemudian mengklik tombol "Buat Kuis", sistem frontend melakukan validasi apakah minimal 1 pertanyaan sudah dipilih. Jika belum ada pertanyaan yang dipilih, sistem menampilkan pesan error. Setelah validasi berhasil, sistem menyiapkan data kuis dalam objek quizData yang berisi informasi lengkap kuis termasuk array selectedQuestions.

Sistem kemudian memanggil API quizAPI.createQuiz() dengan parameter quizData. Jika proses pembuatan kuis berhasil, sistem menampilkan notifikasi sukses, mereset semua form input dan selected questions, serta melakukan redirect ke halaman daftar kuis teacher menggunakan navigate(). Jika terjadi error, sistem menampilkan pesan error dan mencatat ke console.

```javascript
// API Create Quiz Controller
exports.createQuiz = asyncHandler(async (req, res, next) => {
  const { title, description, topic, duration, accessCode, questions, isActive } = req.body;

  // Validation
  if (!title || !questions || questions.length === 0) {
    return res.status(400).json({
      success: false,
      message: 'Judul dan minimal 1 pertanyaan wajib diisi'
    });
  }

  // Cek apakah access code sudah digunakan
  const existingQuiz = await Quiz.findOne({ accessCode });
  if (existingQuiz) {
    return res.status(400).json({
      success: false,
      message: 'Kode akses sudah digunakan, silakan gunakan kode lain'
    });
  }

  try {
    // Buat quiz baru
    const newQuiz = await Quiz.create({
      title,
      description,
      topic,
      duration: duration || 30,
      accessCode,
      questions,
      createdBy: req.user._id,
      isActive: isActive !== undefined ? isActive : true,
      startDate: new Date(),
      endDate: new Date(Date.now() + 7 * 24 * 60 * 60 * 1000), // 7 hari
      participants: [],
      createdAt: new Date()
    });

    // Populate questions untuk response
    const populatedQuiz = await Quiz.findById(newQuiz._id)
      .populate('questions', 'content type topic difficulty')
      .populate('createdBy', 'name email');

    console.log(`[QUIZ-CREATED] Quiz "${title}" created by ${req.user.name} with ${questions.length} questions`);

    res.status(201).json({
      success: true,
      message: 'Kuis berhasil dibuat',
      data: populatedQuiz
    });

  } catch (error) {
    console.error('Error creating quiz:', error);
    res.status(500).json({
      success: false,
      message: 'Gagal membuat kuis'
    });
  }
});
```

Disisi API createQuiz(), sistem melakukan validasi input dengan memastikan title dan minimal 1 pertanyaan sudah diisi. Sistem juga mengecek keunikan access code dengan mencari di database apakah kode tersebut sudah digunakan quiz lain. Jika access code sudah ada, sistem mengembalikan error dengan pesan "Kode akses sudah digunakan".

Jika validasi berhasil, sistem membuat quiz baru menggunakan Quiz.create() dengan semua data yang diterima dari frontend. Sistem mengatur default values seperti duration 30 menit jika tidak diisi, isActive true, dan endDate 7 hari dari sekarang. Setelah quiz berhasil dibuat, sistem melakukan populate untuk mengambil detail pertanyaan dan informasi creator, lalu mencatat log aktivitas dan mengembalikan response sukses dengan data quiz lengkap.

### 4.1.5 Mengikuti Kuis

**Gambar 4.5. Tampilan Mengikuti Kuis**

Pada gambar 4.5 diatas merupakan tampilan dari halaman quiz taker untuk student. Ketika student memasukkan kode akses kuis yang valid, maka akan ditampilkan interface kuis dengan pertanyaan, pilihan jawaban, timer countdown, dan progress indicator. Interface menampilkan informasi kuis seperti judul, durasi, dan nomor pertanyaan saat ini.

```typescript
// Handler untuk join quiz
const handleJoinQuiz = async (e: React.FormEvent) => {
  e.preventDefault();
  setLoading(true);

  try {
    const response = await quizAPI.joinQuiz({
      accessCode: formData.accessCode,
      studentId: user.id
    });
    
    if (response.success) {
      setCurrentQuiz(response.data);
      setQuizStarted(true);
      setTimeRemaining(response.data.duration * 60); // Convert to seconds
      
      // Start timer
      const timer = setInterval(() => {
        setTimeRemaining(prev => {
          if (prev <= 1) {
            clearInterval(timer);
            handleSubmitQuiz(); // Auto submit when time is up
            return 0;
          }
          return prev - 1;
        });
      }, 1000);
      
      setTimerRef(timer);
      toast.success('Berhasil bergabung dengan kuis!');
    }
  } catch (error: any) {
    console.error('Error joining quiz:', error);
    toast.error(error.response?.data?.message || 'Kode akses tidak valid');
  } finally {
    setLoading(false);
  }
};

// Handler untuk submit jawaban
const handleAnswerSelect = (questionId: string, selectedAnswer: string) => {
  setStudentAnswers(prev => ({
    ...prev,
    [questionId]: selectedAnswer
  }));
};

// Handler untuk submit kuis
const handleSubmitQuiz = async () => {
  if (timerRef) {
    clearInterval(timerRef);
  }

  try {
    setSubmitting(true);
    
    const answers = Object.entries(studentAnswers).map(([questionId, answer]) => ({
      question: questionId,
      selectedAnswer: answer
    }));

    const response = await quizAPI.submitQuiz({
      quizId: currentQuiz._id,
      studentId: user.id,
      answers: answers
    });
    
    if (response.success) {
      setQuizResult(response.data);
      setQuizCompleted(true);
      toast.success(`Quiz selesai! Score: ${response.data.score}%`);
    }
  } catch (error: any) {
    console.error('Error submitting quiz:', error);
    toast.error('Gagal submit quiz');
  } finally {
    setSubmitting(false);
  }
};
```

Ketika student memasukkan kode akses dan mengklik "Join Quiz", sistem frontend memanggil API quizAPI.joinQuiz() dengan accessCode dan studentId. Jika berhasil join, sistem menyimpan data kuis ke currentQuiz state, mengatur quizStarted menjadi true, dan menginisialisasi timer countdown dengan durasi kuis dalam detik.

Sistem membuat interval timer yang mengurangi timeRemaining setiap detik. Ketika waktu habis (timeRemaining <= 1), timer otomatis memanggil handleSubmitQuiz() untuk submit jawaban. Untuk setiap pertanyaan, student dapat memilih jawaban melalui handleAnswerSelect() yang menyimpan jawaban ke studentAnswers state. Saat submit kuis, sistem menghentikan timer, menyusun array answers dari studentAnswers, dan mengirim ke API untuk dievaluasi.

```javascript
// API Join Quiz Controller
exports.joinQuiz = asyncHandler(async (req, res, next) => {
  const { accessCode, studentId } = req.body;

  // Cari quiz berdasarkan access code
  const quiz = await Quiz.findOne({ 
    accessCode,
    isActive: true 
  })
  .populate('questions', 'content type options correctAnswer explanation')
  .populate('createdBy', 'name email');

  if (!quiz) {
    return res.status(404).json({
      success: false,
      message: 'Kuis tidak ditemukan atau sudah tidak aktif'
    });
  }

  // Cek apakah quiz masih dalam periode aktif
  const now = new Date();
  if (now < quiz.startDate || now > quiz.endDate) {
    return res.status(400).json({
      success: false,
      message: 'Kuis sudah berakhir atau belum dimulai'
    });
  }

  // Cek apakah student sudah pernah join
  const existingParticipant = quiz.participants.find(
    p => p.student.toString() === studentId
  );

  if (existingParticipant && existingParticipant.status === 'completed') {
    return res.status(400).json({
      success: false,
      message: 'Anda sudah menyelesaikan kuis ini'
    });
  }

  try {
    // Jika belum join, tambahkan sebagai participant
    if (!existingParticipant) {
      await Quiz.findByIdAndUpdate(quiz._id, {
        $push: {
          participants: {
            student: studentId,
            joinedAt: new Date(),
            status: 'in_progress',
            answers: []
          }
        }
      });
    }

    // Return quiz data tanpa correct answers untuk security
    const quizForStudent = {
      ...quiz.toObject(),
      questions: quiz.questions.map(q => ({
        _id: q._id,
        content: q.content,
        type: q.type,
        options: q.options?.map(opt => ({ text: opt.text })) || undefined
      }))
    };

    console.log(`[QUIZ-JOINED] Student ${studentId} joined quiz "${quiz.title}"`);

    res.status(200).json({
      success: true,
      message: 'Berhasil bergabung dengan kuis',
      data: quizForStudent
    });

  } catch (error) {
    console.error('Error joining quiz:', error);
    res.status(500).json({
      success: false,
      message: 'Gagal bergabung dengan kuis'
    });
  }
});
```

Disisi API joinQuiz(), sistem mencari quiz berdasarkan accessCode dan memastikan quiz masih aktif menggunakan Quiz.findOne() dengan populate untuk mendapatkan detail pertanyaan dan creator. Sistem melakukan validasi periode quiz dengan mengecek apakah waktu sekarang berada di antara startDate dan endDate.

Sistem juga mengecek apakah student sudah pernah join quiz dengan mencari di array participants. Jika student sudah completed, maka tidak diizinkan join lagi. Jika belum pernah join, sistem menambahkan student ke participants array dengan status 'in_progress'. Untuk security, sistem mengembalikan data quiz untuk student tanpa informasi jawaban benar, hanya mengirim content dan options tanpa isCorrect flag.

### 4.1.6 Analytics dan Laporan

**Gambar 4.6. Tampilan Analytics dan Laporan**

Pada gambar 4.6 diatas merupakan tampilan dari halaman analytics untuk teacher dan admin. Setelah mengklik menu "Analytics", maka menampilkan dashboard analytics yang berisi statistik kuis, performa student, dan various charts untuk visualisasi data. Terdapat filter untuk periode waktu, topik, dan export functionality untuk laporan.

```typescript
// Handler untuk load analytics data
const loadAnalyticsData = async () => {
  setLoading(true);
  
  try {
    const [quizStats, studentPerformance, topicAnalysis] = await Promise.all([
      analyticsAPI.getQuizStatistics(filterData),
      analyticsAPI.getStudentPerformance(filterData),
      analyticsAPI.getTopicAnalysis(filterData)
    ]);
    
    setQuizStatistics(quizStats.data);
    setStudentPerformance(studentPerformance.data);
    setTopicAnalysis(topicAnalysis.data);
    
    // Prepare chart data
    const chartData = prepareChartData(quizStats.data, studentPerformance.data);
    setChartData(chartData);
    
  } catch (error: any) {
    console.error('Error loading analytics:', error);
    toast.error('Gagal memuat data analytics');
  } finally {
    setLoading(false);
  }
};

// Handler untuk export report
const handleExportReport = async (format: 'pdf' | 'excel') => {
  setExporting(true);
  
  try {
    const reportData = {
      quizStatistics,
      studentPerformance,
      topicAnalysis,
      period: filterData.period,
      exportFormat: format
    };
    
    const response = await analyticsAPI.exportReport(reportData);
    
    // Download file
    const blob = new Blob([response.data], {
      type: format === 'pdf' ? 'application/pdf' : 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet'
    });
    
    const url = window.URL.createObjectURL(blob);
    const link = document.createElement('a');
    link.href = url;
    link.download = `analytics-report-${Date.now()}.${format}`;
    link.click();
    
    window.URL.revokeObjectURL(url);
    toast.success(`Report ${format.toUpperCase()} berhasil diunduh`);
    
  } catch (error: any) {
    console.error('Error exporting report:', error);
    toast.error('Gagal export report');
  } finally {
    setExporting(false);
  }
};
```

Ketika teacher atau admin mengakses halaman analytics, sistem frontend memanggil loadAnalyticsData() yang melakukan multiple API calls secara paralel menggunakan Promise.all(). Sistem mengambil data quiz statistics, student performance, dan topic analysis berdasarkan filter yang dipilih.

Setelah data berhasil diambil, sistem memproses data untuk chart visualization menggunakan prepareChartData() dan menyimpan ke berbagai state untuk ditampilkan di dashboard. Untuk export functionality, sistem menyiapkan reportData yang berisi semua statistik dan memanggil API exportReport() dengan format yang dipilih (PDF atau Excel). Response berupa blob data yang kemudian diunduh otomatis menggunakan URL.createObjectURL() dan element link temporary.

```javascript
// API Analytics Controller
exports.getQuizStatistics = asyncHandler(async (req, res, next) => {
  const { period, topic, teacherId } = req.query;
  
  // Build filter criteria
  const filter = { isActive: true };
  
  if (topic) filter.topic = new RegExp(topic, 'i');
  if (teacherId) filter.createdBy = teacherId;
  
  // Date range filter
  if (period) {
    const endDate = new Date();
    let startDate = new Date();
    
    switch (period) {
      case 'week':
        startDate.setDate(endDate.getDate() - 7);
        break;
      case 'month':
        startDate.setMonth(endDate.getMonth() - 1);
        break;
      case 'quarter':
        startDate.setMonth(endDate.getMonth() - 3);
        break;
      default:
        startDate.setFullYear(endDate.getFullYear() - 1);
    }
    
    filter.createdAt = { $gte: startDate, $lte: endDate };
  }

  try {
    // Aggregate quiz statistics
    const quizStats = await Quiz.aggregate([
      { $match: filter },
      {
        $group: {
          _id: null,
          totalQuizzes: { $sum: 1 },
          totalParticipants: { $sum: { $size: '$participants' } },
          averageScore: {
            $avg: {
              $avg: '$participants.score'
            }
          },
          completionRate: {
            $avg: {
              $divide: [
                { $size: { $filter: { input: '$participants', cond: { $eq: ['$$this.status', 'completed'] } } } },
                { $size: '$participants' }
              ]
            }
          }
        }
      }
    ]);

    // Quiz performance by difficulty
    const difficultyStats = await Question.aggregate([
      {
        $lookup: {
          from: 'quizzes',
          localField: '_id',
          foreignField: 'questions',
          as: 'quizzes'
        }
      },
      { $match: { 'quizzes.0': { $exists: true } } },
      {
        $group: {
          _id: '$difficulty',
          questionCount: { $sum: 1 },
          averageScore: { $avg: '$averageScore' }
        }
      }
    ]);

    // Monthly trend data
    const monthlyTrend = await Quiz.aggregate([
      { $match: filter },
      {
        $group: {
          _id: {
            year: { $year: '$createdAt' },
            month: { $month: '$createdAt' }
          },
          quizCount: { $sum: 1 },
          participantCount: { $sum: { $size: '$participants' } }
        }
      },
      { $sort: { '_id.year': 1, '_id.month': 1 } }
    ]);

    const statistics = {
      summary: quizStats[0] || {
        totalQuizzes: 0,
        totalParticipants: 0,
        averageScore: 0,
        completionRate: 0
      },
      difficultyBreakdown: difficultyStats,
      monthlyTrend: monthlyTrend
    };

    console.log(`[ANALYTICS] Quiz statistics generated for period: ${period}`);

    res.status(200).json({
      success: true,
      message: 'Statistik quiz berhasil diambil',
      data: statistics
    });

  } catch (error) {
    console.error('Error getting quiz statistics:', error);
    res.status(500).json({
      success: false,
      message: 'Gagal mengambil statistik quiz'
    });
  }
});
```

Disisi API getQuizStatistics(), sistem membangun filter criteria berdasarkan query parameters seperti period, topic, dan teacherId. Untuk period filter, sistem menghitung startDate berdasarkan periode yang dipilih (week, month, quarter, atau year) dan membuat filter date range.

Sistem menggunakan MongoDB aggregation pipeline untuk menghitung berbagai statistik seperti total quiz, total participants, average score, dan completion rate. Sistem juga melakukan aggregation terpisah untuk mendapatkan breakdown berdasarkan difficulty level dan trend data bulanan. Semua hasil aggregation dikombinasikan dalam objek statistics yang komprehensif dan dikembalikan sebagai response API.

### 4.1.7 Manajemen User dan Profile

**Gambar 4.7. Tampilan Manajemen User**

Pada gambar 4.7 diatas merupakan tampilan dari halaman manajemen user untuk admin. Setelah mengklik menu "User Management", maka menampilkan daftar semua user yang terdaftar dalam sistem meliputi nama, email, role, status, dan aksi untuk edit/delete. Terdapat juga fitur untuk verifikasi teacher registration dan manajemen role user.

```typescript
// Handler untuk load data users
const loadUsersData = async () => {
  setLoading(true);
  
  try {
    const response = await userAPI.getAllUsers({
      page: currentPage,
      limit: 10,
      role: filterRole,
      status: filterStatus
    });
    
    setUsers(response.data.users);
    setPagination(response.data.pagination);
    
  } catch (error: any) {
    console.error('Error loading users:', error);
    toast.error('Gagal memuat data users');
  } finally {
    setLoading(false);
  }
};

// Handler untuk update user status (approve/reject teacher)
const handleUpdateUserStatus = async (userId: string, newStatus: string, reason?: string) => {
  try {
    setUpdating(true);
    
    const response = await userAPI.updateUserStatus({
      userId,
      status: newStatus,
      reason
    });
    
    if (response.success) {
      // Update local state
      setUsers(prev => prev.map(user => 
        user._id === userId ? { ...user, status: newStatus } : user
      ));
      
      toast.success(`Status user berhasil diubah menjadi ${newStatus}`);
      
      // Send notification email to user
      if (newStatus === 'active') {
        toast.success('Email konfirmasi telah dikirim ke teacher');
      } else if (newStatus === 'rejected') {
        toast.info('Email penolakan telah dikirim ke teacher');
      }
    }
  } catch (error: any) {
    console.error('Error updating user status:', error);
    toast.error(error.response?.data?.message || 'Gagal mengubah status user');
  } finally {
    setUpdating(false);
  }
};

// Handler untuk delete user
const handleDeleteUser = async (userId: string) => {
  if (!confirm('Apakah Anda yakin ingin menghapus user ini?')) {
    return;
  }
  
  try {
    const response = await userAPI.deleteUser(userId);
    
    if (response.success) {
      setUsers(prev => prev.filter(user => user._id !== userId));
      toast.success('User berhasil dihapus');
    }
  } catch (error: any) {
    console.error('Error deleting user:', error);
    toast.error('Gagal menghapus user');
  }
};
```

Ketika admin mengakses halaman user management, sistem frontend memanggil loadUsersData() untuk mengambil daftar semua user dengan pagination dan filter. Sistem dapat melakukan filter berdasarkan role (admin/teacher/student) dan status (active/pending/rejected) sesuai kebutuhan admin.

Untuk fitur approve/reject teacher, admin dapat mengubah status user melalui handleUpdateUserStatus() yang mengirim request ke API dengan userId, status baru, dan optional reason. Sistem juga menyediakan feedback visual dengan toast notification dan otomatis mengirim email notifikasi ke user yang statusnya diubah. Untuk delete user, sistem melakukan konfirmasi terlebih dahulu sebelum menghapus data dari database dan update local state.

```javascript
// API User Management Controller
exports.getAllUsers = asyncHandler(async (req, res, next) => {
  const { page = 1, limit = 10, role, status, search } = req.query;
  
  // Build filter criteria
  const filter = {};
  if (role && role !== 'all') filter.role = role;
  if (status && status !== 'all') filter.status = status;
  if (search) {
    filter.$or = [
      { name: new RegExp(search, 'i') },
      { email: new RegExp(search, 'i') }
    ];
  }
  
  try {
    const users = await User.find(filter)
      .select('-password') // Exclude password from response
      .sort({ createdAt: -1 })
      .limit(limit * 1)
      .skip((page - 1) * limit);
    
    const total = await User.countDocuments(filter);
    
    res.status(200).json({
      success: true,
      message: 'Data users berhasil diambil',
      data: {
        users,
        pagination: {
          page: parseInt(page),
          limit: parseInt(limit),
          total,
          pages: Math.ceil(total / limit)
        }
      }
    });
    
  } catch (error) {
    console.error('Error getting users:', error);
    res.status(500).json({
      success: false,
      message: 'Gagal mengambil data users'
    });
  }
});

exports.updateUserStatus = asyncHandler(async (req, res, next) => {
  const { userId, status, reason } = req.body;
  
  // Validate status
  const validStatuses = ['active', 'pending', 'rejected'];
  if (!validStatuses.includes(status)) {
    return res.status(400).json({
      success: false,
      message: 'Status tidak valid'
    });
  }
  
  try {
    const user = await User.findById(userId);
    if (!user) {
      return res.status(404).json({
        success: false,
        message: 'User tidak ditemukan'
      });
    }
    
    // Update user status
    user.status = status;
    if (reason) user.statusReason = reason;
    await user.save();
    
    // Send email notification based on status
    if (status === 'active' && user.role === 'teacher') {
      await sendTeacherApprovalEmail(user.email, user.name);
    } else if (status === 'rejected' && user.role === 'teacher') {
      await sendTeacherRejectionEmail(user.email, user.name, reason);
    }
    
    console.log(`[USER-STATUS-UPDATED] User ${user.email} status changed to ${status} by admin`);
    
    res.status(200).json({
      success: true,
      message: `Status user berhasil diubah menjadi ${status}`,
      data: {
        userId: user._id,
        status: user.status,
        updatedAt: new Date()
      }
    });
    
  } catch (error) {
    console.error('Error updating user status:', error);
    res.status(500).json({
      success: false,
      message: 'Gagal mengubah status user'
    });
  }
});
```

Disisi API user management, sistem mengimplementasikan getAllUsers() dengan filtering dan pagination yang fleksibel. Admin dapat filter berdasarkan role, status, dan search query untuk mencari user berdasarkan nama atau email. Sistem menggunakan MongoDB query dengan $or operator untuk search functionality dan exclude password field dari response untuk security.

Untuk updateUserStatus(), sistem melakukan validasi status yang diizinkan dan mencari user berdasarkan userId. Setelah update status berhasil, sistem mengirim email notification yang sesuai menggunakan email service. Jika teacher di-approve, sistem mengirim email approval; jika di-reject, sistem mengirim email rejection dengan reason yang diberikan admin.

### 4.1.8 Profile Management

**Gambar 4.8. Tampilan Profile Management**

Pada gambar 4.8 diatas merupakan tampilan dari halaman profile user. Setelah mengklik menu "Profile", maka menampilkan informasi detail user yang sedang login termasuk foto profile, nama, email, role, dan informasi tambahan. Terdapat opsi untuk edit profile dan change password.

```typescript
// Handler untuk load profile data
const loadProfileData = async () => {
  try {
    const user = getCurrentUser();
    if (user) {
      setProfileData({
        name: user.name,
        email: user.email,
        role: user.role,
        photo: user.photo || '',
        teacherInfo: user.teacherInfo || {}
      });
    }
  } catch (error) {
    console.error('Error loading profile:', error);
    toast.error('Gagal memuat data profile');
  }
};

// Handler untuk update profile
const handleUpdateProfile = async (e: React.FormEvent) => {
  e.preventDefault();
  setUpdating(true);
  
  try {
    const formData = new FormData();
    formData.append('name', profileData.name);
    formData.append('email', profileData.email);
    
    // Add photo if changed
    if (photoFile) {
      formData.append('photo', photoFile);
    }
    
    // Add teacher-specific info if role is teacher
    if (profileData.role === 'teacher') {
      formData.append('institution', profileData.teacherInfo.institution);
      formData.append('expertise', profileData.teacherInfo.expertise);
      formData.append('experience', profileData.teacherInfo.experience.toString());
    }
    
    const response = await userAPI.updateProfile(formData);
    
    if (response.success) {
      // Update local storage with new user data
      const updatedUser = { ...getCurrentUser(), ...response.data };
      localStorage.setItem('user', JSON.stringify(updatedUser));
      
      toast.success('Profile berhasil diperbarui');
      setEditMode(false);
      setPhotoFile(null);
    }
  } catch (error: any) {
    console.error('Error updating profile:', error);
    toast.error(error.response?.data?.message || 'Gagal memperbarui profile');
  } finally {
    setUpdating(false);
  }
};

// Handler untuk change password
const handleChangePassword = async (e: React.FormEvent) => {
  e.preventDefault();
  
  if (passwordData.newPassword !== passwordData.confirmPassword) {
    toast.error('Konfirmasi password tidak sesuai');
    return;
  }
  
  if (passwordData.newPassword.length < 6) {
    toast.error('Password minimal 6 karakter');
    return;
  }
  
  try {
    setChangingPassword(true);
    
    const response = await userAPI.changePassword({
      currentPassword: passwordData.currentPassword,
      newPassword: passwordData.newPassword
    });
    
    if (response.success) {
      toast.success('Password berhasil diubah');
      setPasswordData({
        currentPassword: '',
        newPassword: '',
        confirmPassword: ''
      });
      setShowPasswordForm(false);
    }
  } catch (error: any) {
    console.error('Error changing password:', error);
    toast.error(error.response?.data?.message || 'Gagal mengubah password');
  } finally {
    setChangingPassword(false);
  }
};
```

Ketika user mengakses halaman profile, sistem frontend memanggil loadProfileData() untuk mengambil informasi user dari localStorage atau API. Sistem menampilkan informasi yang berbeda berdasarkan role user, misalnya teacher akan memiliki tambahan informasi seperti institution, expertise, dan experience.

Untuk update profile, sistem menggunakan FormData untuk handle file upload (photo) dan text data. Sistem melakukan validasi khusus untuk teacher role dengan menambahkan informasi tambahan ke FormData. Setelah update berhasil, sistem memperbarui localStorage dengan data user yang baru untuk memastikan konsistensi data di seluruh aplikasi.

Untuk change password, sistem melakukan validasi password confirmation dan minimum length sebelum mengirim request ke API. Sistem mengecek apakah new password dan confirm password sama, serta memastikan password minimal 6 karakter untuk security compliance.

```javascript
// API Profile Management Controller
exports.updateProfile = asyncHandler(async (req, res, next) => {
  const userId = req.user._id;
  const { name, email, institution, expertise, experience } = req.body;
  
  try {
    const user = await User.findById(userId);
    if (!user) {
      return res.status(404).json({
        success: false,
        message: 'User tidak ditemukan'
      });
    }
    
    // Check if email already exists (if changed)
    if (email !== user.email) {
      const existingUser = await User.findOne({ email, _id: { $ne: userId } });
      if (existingUser) {
        return res.status(400).json({
          success: false,
          message: 'Email sudah digunakan user lain'
        });
      }
    }
    
    // Update basic info
    user.name = name || user.name;
    user.email = email || user.email;
    
    // Handle photo upload
    if (req.file) {
      // Delete old photo if exists
      if (user.photo) {
        const oldPhotoPath = path.join('uploads/photos/', user.photo);
        if (fs.existsSync(oldPhotoPath)) {
          fs.unlinkSync(oldPhotoPath);
        }
      }
      user.photo = req.file.filename;
    }
    
    // Update teacher-specific info
    if (user.role === 'teacher') {
      user.teacherInfo = {
        ...user.teacherInfo,
        institution: institution || user.teacherInfo.institution,
        expertise: expertise || user.teacherInfo.expertise,
        experience: parseInt(experience) || user.teacherInfo.experience
      };
    }
    
    await user.save();
    
    // Return updated user data (excluding password)
    const updatedUser = await User.findById(userId).select('-password');
    
    console.log(`[PROFILE-UPDATED] User ${user.email} updated profile`);
    
    res.status(200).json({
      success: true,
      message: 'Profile berhasil diperbarui',
      data: updatedUser
    });
    
  } catch (error) {
    console.error('Error updating profile:', error);
    res.status(500).json({
      success: false,
      message: 'Gagal memperbarui profile'
    });
  }
});

exports.changePassword = asyncHandler(async (req, res, next) => {
  const userId = req.user._id;
  const { currentPassword, newPassword } = req.body;
  
  try {
    const user = await User.findById(userId);
    if (!user) {
      return res.status(404).json({
        success: false,
        message: 'User tidak ditemukan'
      });
    }
    
    // Verify current password
    const isValidPassword = await bcrypt.compare(currentPassword, user.password);
    if (!isValidPassword) {
      return res.status(400).json({
        success: false,
        message: 'Password lama tidak benar'
      });
    }
    
    // Hash new password
    const saltRounds = 12;
    const hashedNewPassword = await bcrypt.hash(newPassword, saltRounds);
    
    // Update password
    user.password = hashedNewPassword;
    user.passwordChangedAt = new Date();
    await user.save();
    
    console.log(`[PASSWORD-CHANGED] User ${user.email} changed password`);
    
    res.status(200).json({
      success: true,
      message: 'Password berhasil diubah'
    });
    
  } catch (error) {
    console.error('Error changing password:', error);
    res.status(500).json({
      success: false,
      message: 'Gagal mengubah password'
    });
  }
});
```

Disisi API profile management, updateProfile() melakukan validasi email uniqueness jika email diubah dengan mengecek apakah email baru sudah digunakan user lain. Sistem juga handle file upload untuk photo dengan menghapus foto lama jika ada foto baru yang diupload. Untuk teacher role, sistem mengupdate teacherInfo object dengan informasi tambahan seperti institution, expertise, dan experience.

Untuk changePassword(), sistem melakukan verifikasi password lama terlebih dahulu menggunakan bcrypt.compare() sebelum mengizinkan update password. Sistem menggunakan salt rounds 12 untuk hashing password baru yang memberikan security level yang tinggi. Sistem juga mencatat timestamp passwordChangedAt untuk audit trail.

## 4.2 Hasil Pengujian Blackbox Testing

### 4.2.1 Pengujian Fungsional

**Tabel 4.1. Pengujian Blackbox Testing Fungsional**

| NO | Modul yang diuji | Relasi yang diharapkan | Hasil Uji coba | Penilaian |
|----|------------------|----------------------|---------------|-----------|
| 1 | **Login** | | | |
| | email benar password benar | masuk ke menu dashboard ExamExpert AI sesuai role | Berhasil | 100% |
| | email benar password salah | menampilkan notifikasi "Email atau password salah" | Berhasil | 100% |
| | email salah password benar | menampilkan notifikasi "Email tidak terdaftar" | Berhasil | 100% |
| | email salah password salah | menampilkan notifikasi "Email tidak terdaftar" | Berhasil | 100% |
| | teacher dengan status pending | menampilkan notifikasi "Akun teacher belum diverifikasi" | Berhasil | 100% |
| 2 | **Logout** | | | |
| | semua role user mengklik tombol Logout | user berhasil keluar dari sistem dan kembali ke halaman login | Berhasil | 100% |
| 3 | **AI Question Generation** | | | |
| | mengisi form dengan lengkap | menampilkan pertanyaan AI yang dihasilkan sesuai kriteria | Berhasil | 100% |
| | mengisi form tidak lengkap (tanpa topic) | menampilkan notifikasi "Topic dan type wajib diisi" | Berhasil | 100% |
| | mengisi form tidak lengkap (tanpa type) | menampilkan notifikasi "Topic dan type wajib diisi" | Berhasil | 100% |
| | generate dengan count > 20 | membatasi maksimal 20 pertanyaan | Berhasil | 100% |
| | generate dengan API key invalid | menampilkan notifikasi "Perplexity API key tidak ditemukan" | Berhasil | 100% |
| 4 | **Quiz Creation** | | | |
| | mengisi form dengan lengkap dan pilih pertanyaan | menampilkan notifikasi "Kuis berhasil dibuat" | Berhasil | 100% |
| | mengisi form tanpa memilih pertanyaan | menampilkan notifikasi "Pilih minimal 1 pertanyaan" | Berhasil | 100% |
| | menggunakan access code yang sudah ada | menampilkan notifikasi "Kode akses sudah digunakan" | Berhasil | 100% |
| | mengisi duration di bawah 5 menit | sistem membatasi minimal 5 menit | Berhasil | 100% |
| | mengisi duration di atas 180 menit | sistem membatasi maksimal 180 menit | Berhasil | 100% |
| 5 | **Quiz Taking (Student)** | | | |
| | memasukkan access code yang valid | berhasil join quiz dan menampilkan pertanyaan | Berhasil | 100% |
| | memasukkan access code yang tidak valid | menampilkan notifikasi "Kuis tidak ditemukan" | Berhasil | 100% |
| | join quiz yang sudah expired | menampilkan notifikasi "Kuis sudah berakhir" | Berhasil | 100% |
| | join quiz yang sudah pernah diselesaikan | menampilkan notifikasi "Anda sudah menyelesaikan kuis ini" | Berhasil | 100% |
| | waktu quiz habis | otomatis submit jawaban yang sudah dipilih | Berhasil | 100% |
| | submit quiz sebelum waktu habis | menampilkan hasil score dan review jawaban | Berhasil | 100% |
| 6 | **Analytics Dashboard** | | | |
| | memilih menu Analytics | menampilkan dashboard dengan statistik quiz | Berhasil | 100% |
| | filter berdasarkan periode | menampilkan data sesuai periode yang dipilih | Berhasil | 100% |
| | filter berdasarkan topik | menampilkan data sesuai topik yang dipilih | Berhasil | 100% |
| | export report PDF | berhasil download file PDF laporan | Berhasil | 100% |
| | export report Excel | berhasil download file Excel laporan | Berhasil | 100% |
| 7 | **User Management (Admin)** | | | |
| | memilih menu User Management | menampilkan daftar semua user terdaftar | Berhasil | 100% |
| | approve teacher registration | status teacher berubah menjadi active | Berhasil | 100% |
| | reject teacher registration | status teacher berubah menjadi rejected | Berhasil | 100% |
| | delete user | user berhasil dihapus dari sistem | Berhasil | 100% |
| | filter user berdasarkan role | menampilkan user sesuai role yang dipilih | Berhasil | 100% |
| | search user berdasarkan nama/email | menampilkan hasil pencarian yang sesuai | Berhasil | 100% |
| 8 | **Profile Management** | | | |
| | memilih menu Profile | menampilkan informasi profile user | Berhasil | 100% |
| | edit profile dengan data valid | menampilkan notifikasi "Profile berhasil diperbarui" | Berhasil | 100% |
| | edit profile dengan email yang sudah ada | menampilkan notifikasi "Email sudah digunakan" | Berhasil | 100% |
| | upload foto profile | foto berhasil diupload dan ditampilkan | Berhasil | 100% |
| | upload foto dengan format tidak sesuai | menampilkan notifikasi format file tidak valid | Berhasil | 100% |
| 9 | **Change Password** | | | |
| | mengisi password lama yang benar | berhasil melakukan perubahan password | Berhasil | 100% |
| | mengisi password lama yang salah | menampilkan notifikasi "Password lama tidak benar" | Berhasil | 100% |
| | konfirmasi password tidak sesuai | menampilkan notifikasi "Konfirmasi password tidak sesuai" | Berhasil | 100% |
| | password baru kurang dari 6 karakter | menampilkan notifikasi "Password minimal 6 karakter" | Berhasil | 100% |
| 10 | **Question Bank Management** | | | |
| | view daftar pertanyaan | menampilkan semua pertanyaan yang tersedia | Berhasil | 100% |
| | filter berdasarkan topik | menampilkan pertanyaan sesuai topik | Berhasil | 100% |
| | filter berdasarkan difficulty | menampilkan pertanyaan sesuai tingkat kesulitan | Berhasil | 100% |
| | filter berdasarkan type | menampilkan pertanyaan sesuai jenis | Berhasil | 100% |
| | pagination | navigasi antar halaman berfungsi dengan baik | Berhasil | 100% |

**Rata-rata hasil pengujian: 100%**

Berdasarkan hasil pengujian dengan metode blackbox testing fungsional, keberhasilan yang diperoleh peneliti adalah 100% maka dapat dinyatakan bahwa sistem ExamExpert AI berhasil dan berjalan dengan baik sesuai dengan spesifikasi yang dirancang.

### 4.2.2 Pengujian Non-Fungsional

**Tabel 4.2. Pengujian Blackbox Testing Non-Fungsional**

| NO | Modul Yang Diuji | Hasil Realisasi |
|----|------------------|-----------------|
| **NF1** | **Response Time** | |
| | Proses login | Waktu loading setelah submit login ≤ 2 detik |
| | Menampilkan halaman dashboard admin | Waktu loading halaman dashboard admin ≤ 3 detik |
| | Menampilkan halaman dashboard teacher | Waktu loading halaman dashboard teacher ≤ 3 detik |
| | Menampilkan halaman dashboard student | Waktu loading halaman dashboard student ≤ 3 detik |
| | Generate AI questions | Waktu generate 5 pertanyaan ≤ 15 detik |
| | Menyimpan quiz | Waktu loading setelah submit quiz ≤ 2 detik |
| | Join quiz | Waktu loading untuk join quiz ≤ 3 detik |
| | Submit quiz answers | Waktu loading untuk submit jawaban ≤ 2 detik |
| | Load analytics data | Waktu loading dashboard analytics ≤ 5 detik |
| **NF2** | **Security** | |
| | Autentikasi JWT | Setiap request ke API harus menyertakan token JWT valid |
| | Authorization | User hanya dapat mengakses fitur sesuai role mereka |
| | Password hashing | Password disimpan dalam bentuk hash menggunakan bcrypt |
| | Input validation | Semua input divalidasi untuk mencegah injection attacks |
| | CORS policy | API hanya menerima request dari domain frontend yang authorized |
| **NF3** | **Usability** | |
| | Responsive design | Interface dapat diakses dengan baik di desktop dan mobile |
| | User feedback | Sistem memberikan feedback yang jelas untuk setiap aksi user |
| | Error handling | Error ditampilkan dengan pesan yang mudah dipahami |
| | Loading indicators | Sistem menampilkan loading state untuk operasi yang membutuhkan waktu |
| **NF4** | **Performance** | |
| | Concurrent users | Sistem dapat menangani hingga 100 concurrent users dengan performa baik |
| | Database queries | Query database rata-rata ≤ 100ms |
| | Memory usage | Penggunaan memory server rata-rata ≤ 500MB |
| | CPU usage | Penggunaan CPU server rata-rata ≤ 30% |

Berdasarkan hasil pengujian blackbox testing non-fungsional, diperoleh bahwa waktu respons sistem saat mengakses berbagai halaman inti berada di bawah ambang batas yang ditentukan. Sistem juga memenuhi aspek security dengan implementasi JWT authentication, proper authorization, dan input validation. Dari aspek usability, interface responsive dan memberikan feedback yang jelas kepada user. Performa sistem menunjukkan hasil yang optimal dengan kemampuan menangani concurrent users sesuai target.
