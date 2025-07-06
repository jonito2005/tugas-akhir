# BAB III
# METODOLOGI PENELITIAN

## 3.1 Pendekatan Penelitian

Penelitian ini menggunakan pendekatan **Design Science Research (DSR)** yang dikombinasikan dengan **Agile Software Development**. Design Science Research dipilih karena penelitian ini bertujuan untuk menciptakan solusi teknologi (artefak) yang dapat mengatasi masalah praktis dalam bidang pendidikan. Menurut Hevner et al. (2004), DSR berfokus pada pemecahan masalah melalui penciptaan dan evaluasi artefak teknologi yang inovatif [1].

### 3.1.1 Design Science Research Framework

Framework DSR yang digunakan dalam penelitian ini mengikuti model yang dikembangkan oleh Peffers et al. (2007) yang terdiri dari enam tahap [2]:

1. **Problem Identification and Motivation**: Mengidentifikasi masalah spesifik dan memotivasi nilai solusi
2. **Define Objectives of Solution**: Mendefinisikan tujuan solusi berdasarkan masalah yang teridentifikasi
3. **Design and Development**: Merancang dan mengembangkan artefak
4. **Demonstration**: Mendemonstrasikan penggunaan artefak untuk mengatasi masalah
5. **Evaluation**: Mengobservasi dan mengukur seberapa baik artefak mendukung solusi masalah
6. **Communication**: Mengkomunikasikan masalah, artefak, utilitas, dan kebaruan kepada audiens yang relevan

### 3.1.2 Agile Software Development

Metodologi Agile dipilih untuk proses pengembangan perangkat lunak karena sifatnya yang iteratif dan adaptif. Prinsip-prinsip Agile yang diterapkan meliputi [3]:

- **Iterative Development**: Pengembangan dilakukan dalam sprint-sprint pendek
- **Customer Collaboration**: Kolaborasi berkelanjutan dengan stakeholder
- **Responding to Change**: Kemampuan beradaptasi terhadap perubahan requirement
- **Working Software**: Fokus pada delivery software yang berfungsi

## 3.2 Metode Pengumpulan Data

### 3.2.1 Data Primer

**Observasi Langsung**
Melakukan observasi terhadap proses pembelajaran dan evaluasi di lingkungan pendidikan untuk memahami kebutuhan aktual pengguna.

**Wawancara Terstruktur**
Melakukan wawancara dengan stakeholder kunci:
- **Guru/Pendidik** (5 responden): Untuk memahami kebutuhan dalam pembuatan soal dan manajemen ujian
- **Siswa** (10 responden): Untuk memahami preferensi dan pengalaman dalam mengikuti ujian online
- **Administrator IT Sekolah** (3 responden): Untuk memahami aspek teknis dan infrastruktur

**Survei Online**
Menyebarkan kuesioner terstruktur kepada 50 responden (25 guru dan 25 siswa) untuk mendapatkan data kuantitatif tentang:
- Pengalaman dengan sistem ujian online
- Preferensi fitur yang diinginkan
- Tantangan yang dihadapi dalam proses evaluasi

### 3.2.2 Data Sekunder

**Studi Literatur**
Mengumpulkan data dari sumber-sumber berikut:
- Jurnal ilmiah tentang AI in Education
- Conference proceedings tentang e-learning systems
- Technical documentation dari teknologi yang digunakan
- Best practices dalam pengembangan LMS

**Analisis Sistem Sejenis**
Melakukan analisis terhadap sistem yang sudah ada seperti:
- Google Classroom
- Moodle
- Kahoot
- Quizlet
- Edpuzzle

## 3.3 Analisis Kebutuhan Sistem

### 3.3.1 Functional Requirements Analysis

Analisis kebutuhan fungsional dilakukan melalui teknik **Use Case Analysis** dengan mengidentifikasi:

**Actor Identification**
- **Admin**: Mengelola sistem secara keseluruhan
- **Teacher**: Membuat dan mengelola ujian
- **Student**: Mengikuti ujian dan melihat hasil

**Use Case Specification**
Setiap use case didokumentasikan dengan detail mencakup:
- Use case name dan ID
- Primary actor
- Preconditions dan postconditions
- Main flow dan alternative flow
- Exception handling

### 3.3.2 Non-Functional Requirements Analysis

**Performance Requirements**
- Response time < 2 detik untuk operasi standar
- Throughput minimum 1000 concurrent users
- Availability 99.9% uptime

**Security Requirements**
- Authentication dan authorization
- Data encryption in transit dan at rest
- Input validation dan sanitization
- Session management

**Usability Requirements**
- Intuitive user interface
- Mobile-responsive design
- Accessibility compliance (WCAG 2.1)
- Multi-language support (fokus Bahasa Indonesia)

### 3.3.3 Requirements Validation

Validasi requirement dilakukan melalui:
- **Stakeholder Review**: Review oleh representative dari setiap user group
- **Prototype Validation**: Validasi menggunakan prototype low-fidelity
- **Expert Review**: Review oleh expert dalam bidang educational technology

## 3.4 Perancangan Sistem

### 3.4.1 System Architecture Design

**High-Level Architecture**
Sistem dirancang menggunakan **3-tier architecture**:
- **Presentation Tier**: React.js frontend application
- **Logic Tier**: Node.js/Express.js backend services
- **Data Tier**: MongoDB database

**Microservices Architecture Considerations**
Meskipun dimulai dengan monolithic architecture, sistem dirancang dengan mempertimbangkan future scaling ke microservices:
- **User Service**: Manajemen pengguna dan authentication
- **Question Service**: Pembuatan dan manajemen soal
- **Exam Service**: Pelaksanaan ujian
- **Analytics Service**: Analisis dan reporting

### 3.4.2 Database Design

**Conceptual Data Model**
Menggunakan **Entity Relationship Diagram (ERD)** untuk menggambarkan entitas utama dan relasi antar entitas:
- User (Admin, Teacher, Student)
- Question (MultipleChoice, TrueFalse, Essay)
- Quiz
- QuizAttempt
- Result

**Logical Data Model**
Transformasi conceptual model ke logical model dengan mempertimbangkan:
- Data types dan constraints
- Indexing strategy
- Normalization vs denormalization trade-offs

**Physical Data Model**
Implementasi logical model ke MongoDB dengan mempertimbangkan:
- Document structure design
- Embedding vs referencing strategy
- Performance optimization

### 3.4.3 API Design

**RESTful API Design Principles**
- Resource-based URLs
- HTTP methods untuk different operations
- Consistent response format
- Proper HTTP status codes
- API versioning strategy

**API Documentation**
Menggunakan **OpenAPI Specification (Swagger)** untuk dokumentasi API yang comprehensive dan interactive.

### 3.4.4 User Interface Design

**Design Thinking Process**
1. **Empathize**: Memahami kebutuhan pengguna melalui user research
2. **Define**: Mendefinisikan problem statement yang specific
3. **Ideate**: Brainstorming solusi dan ide-ide creative
4. **Prototype**: Membuat prototype untuk testing
5. **Test**: Testing dengan real users dan iterasi

**UI/UX Design Principles**
- **User-Centered Design**: Fokus pada kebutuhan dan preferensi pengguna
- **Consistency**: Konsistensi dalam design elements dan interaction patterns
- **Accessibility**: Memastikan aplikasi dapat diakses oleh pengguna dengan berbagai kemampuan
- **Responsive Design**: Optimasi untuk berbagai ukuran layar

## 3.5 Teknologi dan Tools

### 3.5.1 Development Stack

**Frontend Technologies**
- **React.js 18.x**: UI library untuk building interactive interfaces
- **TypeScript 5.x**: Static typing untuk JavaScript
- **Tailwind CSS 3.x**: Utility-first CSS framework
- **Vite**: Build tool untuk development dan production

**Backend Technologies**
- **Node.js 18.x LTS**: JavaScript runtime environment
- **Express.js 4.x**: Web application framework
- **MongoDB 6.x**: NoSQL document database
- **Mongoose**: MongoDB object modeling untuk Node.js

**Authentication & Security**
- **JSON Web Tokens (JWT)**: Untuk authentication dan authorization
- **bcrypt**: Password hashing
- **helmet**: Security middleware untuk Express
- **cors**: Cross-Origin Resource Sharing handling

### 3.5.2 External APIs and Services

**AI Integration**
- **Perplexity API**: Untuk AI-powered question generation
- **Rate limiting dan error handling** untuk API calls
- **Fallback mechanisms** untuk service reliability

**Email Services**
- **Nodemailer**: Email sending capabilities
- **SMTP integration** dengan providers seperti Gmail atau SendGrid

### 3.5.3 Development Tools

**Version Control**
- **Git**: Distributed version control system
- **GitHub**: Repository hosting dan collaboration

**Code Quality**
- **ESLint**: JavaScript/TypeScript linting
- **Prettier**: Code formatting
- **Husky**: Git hooks untuk pre-commit checks

**Testing Tools**
- **Jest**: Unit testing framework
- **React Testing Library**: Testing utilities untuk React components
- **Supertest**: HTTP assertion library untuk API testing
- **Cypress**: End-to-end testing framework

**Development Environment**
- **VS Code**: Primary IDE dengan extensions
- **Postman**: API development dan testing
- **MongoDB Compass**: Database management GUI

## 3.6 Tahapan Implementasi

### 3.6.1 Sprint Planning

Implementasi dilakukan dalam 6 sprint dengan durasi 2 minggu per sprint:

**Sprint 1: Foundation Setup**
- Project initialization dan setup
- Database schema design dan implementation
- Basic authentication system
- Development environment configuration

**Sprint 2: User Management**
- User registration dan login
- Role-based access control
- Profile management
- Password reset functionality

**Sprint 3: Question Management**
- Manual question creation
- Question bank management
- AI integration untuk question generation
- Question approval workflow

**Sprint 4: Quiz Creation dan Management**
- Quiz builder interface
- Quiz configuration (duration, scoring, etc.)
- Question selection dan arrangement
- Quiz preview functionality

**Sprint 5: Exam Execution**
- Student exam interface
- Real-time exam monitoring
- Auto-save functionality
- Time management features

**Sprint 6: Results dan Analytics**
- Automated grading system
- Results dashboard untuk students
- Analytics dashboard untuk teachers
- Report generation

### 3.6.2 Quality Assurance

**Testing Strategy**
- **Unit Testing**: Minimum 80% code coverage
- **Integration Testing**: Testing API endpoints dan database operations
- **System Testing**: End-to-end testing scenarios
- **User Acceptance Testing**: Testing dengan real users

**Performance Testing**
- **Load Testing**: Testing dengan multiple concurrent users
- **Stress Testing**: Testing system limits
- **Volume Testing**: Testing dengan large amounts of data

### 3.6.3 Deployment Strategy

**Environment Setup**
- **Development**: Local development environment
- **Staging**: Testing environment yang mirror production
- **Production**: Live environment untuk end users

**CI/CD Pipeline**
- **Continuous Integration**: Automated testing on code commits
- **Continuous Deployment**: Automated deployment ke staging
- **Manual Production Deployment**: Manual approval untuk production releases

## 3.7 Metode Evaluasi

### 3.7.1 Evaluation Metrics

**Technical Metrics**
- **Performance Metrics**: Response time, throughput, resource utilization
- **Quality Metrics**: Bug density, code coverage, maintainability index
- **Reliability Metrics**: Uptime, error rates, recovery time

**User Experience Metrics**
- **Usability Metrics**: Task completion rate, error rate, time on task
- **User Satisfaction**: System Usability Scale (SUS) score
- **Adoption Metrics**: User engagement, feature usage, retention rate

**Educational Effectiveness Metrics**
- **Question Quality**: Expert evaluation of AI-generated questions
- **Learning Outcomes**: Comparison of assessment results
- **Efficiency Gains**: Time saved in question creation dan exam management

### 3.7.2 Evaluation Methods

**Quantitative Evaluation**
- **A/B Testing**: Comparing system performance with traditional methods
- **Statistical Analysis**: Analysis of user performance data
- **Benchmark Testing**: Performance comparison dengan existing systems

**Qualitative Evaluation**
- **User Interviews**: In-depth feedback dari representative users
- **Focus Group Discussions**: Group discussions untuk gather insights
- **Expert Reviews**: Evaluation oleh domain experts

### 3.7.3 Success Criteria

**Primary Success Criteria**
1. System dapat menghasilkan soal berkualitas dalam waktu < 5 menit per soal
2. User satisfaction score ≥ 4.0/5.0 (80%)
3. System performance response time < 2 seconds
4. 95% bug-free operation selama testing period

**Secondary Success Criteria**
1. 50% reduction dalam time spent untuk question creation
2. Positive feedback dari minimum 80% test users
3. Successful integration dengan semua required external APIs
4. Scalability untuk minimum 1000 concurrent users

## 3.8 Timeline Penelitian

```
Bulan 1-2: Analisis Kebutuhan dan Perancangan
- Literature review
- Stakeholder interviews
- Requirements analysis
- System design

Bulan 3-5: Implementasi
- Sprint 1-3: Core system development
- Sprint 4-6: Advanced features dan integration

Bulan 6: Testing dan Evaluasi
- Comprehensive testing
- User acceptance testing
- Performance evaluation
- Bug fixes dan optimization

Bulan 7: Dokumentasi dan Finalisasi
- Documentation completion
- Final testing
- Deployment preparation
- Thesis writing completion
```

## 3.9 Risk Management

### 3.9.1 Technical Risks

**API Dependencies**
- **Risk**: Perplexity API downtime atau changes
- **Mitigation**: Implement fallback mechanisms dan alternative APIs

**Performance Issues**
- **Risk**: System performance degradation dengan increased load
- **Mitigation**: Regular performance testing dan optimization

**Data Security**
- **Risk**: Security breaches atau data leaks
- **Mitigation**: Implement robust security measures dan regular audits

### 3.9.2 Project Risks

**Scope Creep**
- **Risk**: Uncontrolled expansion of project scope
- **Mitigation**: Clear requirement documentation dan change control process

**Timeline Delays**
- **Risk**: Development delays due to technical challenges
- **Mitigation**: Agile methodology dengan regular sprint reviews dan adjustments

**Resource Constraints**
- **Risk**: Limited access to testing environments atau users
- **Mitigation**: Early stakeholder engagement dan alternative testing strategies

---

## Referensi

[1] Hevner, A. R., March, S. T., Park, J., & Ram, S. (2004). Design science in information systems research. *MIS Quarterly*, 28(1), 75-105.

[2] Peffers, K., Tuunanen, T., Rothenberger, M. A., & Chatterjee, S. (2007). A design science research methodology for information systems research. *Journal of Management Information Systems*, 24(3), 45-77.

[3] Beck, K., Beedle, M., Van Bennekum, A., Cockburn, A., Cunningham, W., Fowler, M., ... & Thomas, D. (2001). Manifesto for agile software development. *Agile Alliance*.

[4] Sommerville, I. (2015). *Software Engineering* (10th ed.). Pearson.

[5] Pressman, R., & Maxim, B. (2014). *Software Engineering: A Practitioner's Approach* (8th ed.). McGraw-Hill Education.

[6] Fowler, M. (2018). *Refactoring: Improving the Design of Existing Code* (2nd ed.). Addison-Wesley Professional.

[7] Newman, S. (2021). *Building Microservices: Designing Fine-Grained Systems* (2nd ed.). O'Reilly Media.

[8] Richardson, C. (2018). *Microservices Patterns: With Examples in Java*. Manning Publications.

[9] Krug, S. (2013). *Don't Make Me Think, Revisited: A Common Sense Approach to Web Usability* (3rd ed.). New Riders.

[10] Cooper, A., Reimann, R., Cronin, D., & Noessel, C. (2014). *About Face: The Essentials of Interaction Design* (4th ed.). Wiley.
