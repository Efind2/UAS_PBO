# Menu Login Menggunakan Persistence
Hallo Semua 😄👋 Pada Tugas Pemograman Berorientasi Objek ini, saya menerapkan membuat menu login menggunakan persistence program CRUD (Create, Read, Update, Delete)  dan  penggunaan jasper juga upload file csv pada java GUI(Graphical User Interfaces) dengan IDE netbeans dan yang utama disini saya menggunakan **persistence**. Untuk mengimplementasikan program tersebut saya menggunakan bahasa pemrograman java dan menggunakan aplikasi pengelola database yaitu postgreSQL dan juga plugin iReport.
dan saya menggunakan tabel  entitas Matakuliah dengan atribut Kode MK, SKS, NamaMk, Semester Ajar. dan entitas pengguna dengan atribut username dan password
## Aplikasi
- IDE NetBeans 16
- PostgreSql
- Java Development Kit 11
## Plugin
- [IReport 5.6.0](https://drive.google.com/drive/folders/1gbbMttGeyns5mqb-_hfIAuMjkTzb-XPZ?usp=sharing)
## Library
- PostgreSQL JDBC Driver `NetBeans`
- EclipseLink (JPA 2.1) `NetBeans`
- Persistence (JPA 2.1) `NetBeans`
- [JasperReport](https://drive.google.com/drive/folders/1_i8xBCdLXeMcGdmnTWa2SEnL4oc2sW78?usp=sharing)
## Feature
-  Login menggunakan data user 
-  Melakukan insert data
-  Melakukan update data
-  Menghapus data
-  Menampilkan data ke tabel
-  Melakukan import file csv
-  Mencetak laporan data

## Cara Instalasi 
Install Plugin Ireport di IDE Netbeans dengan mencari menu tools > plugins > donload > add plugins . Tambahkan plugins ireport yang ada di dalam folder yang memiliki tipe file nbm jika tterjadi error install dulu `org-jdeskop-layout-realese.nbm` kemudian baru instal file nbm yang ada didalam folder ireport
## Membuat File jasper
Klik kiri pada package project anda kemudian new > cari report wizard jika belum ada klik other pilih report wizard ![Cuplikan layar 2024-10-26 220058](https://github.com/user-attachments/assets/e4b2bb54-2e9c-4253-8e91-ebbe55364533) 
Kemudain next pilih layout kemudain sesuaikan nama file jasper dengan apa yang anda inginkan kemudian sambungkan query database disini saya menggunkan database Mahasiswa dengan SQL Query `SELECT * FROM mahasiswa` ![image](https://github.com/user-attachments/assets/eae7b901-3c6a-41ce-a468-80a2c77e999c)
kemudian next pindah seluruh kolom ke kiri dan next lagi --finish.
Setelah File jadi anda bisa membuat tampilan layout anda sesuka hati, namun layout anda harus berisi tabel yang ingin anda tampilkan atau cetak
![image](https://github.com/user-attachments/assets/7fd7d5f1-db54-43d7-bdac-57ac60bf4a10)

## Cara Pembuatan Persistence 
- Klik kiri pada package kemudian new lalu Entity Classes From Database
  ![image](https://github.com/user-attachments/assets/af29d699-ec5e-4c7f-88e5-e9dc61d4af3c)
- Kemudian buat koneksi ke database
  ![image](https://github.com/user-attachments/assets/7b82d6d8-d439-453e-ae79-d0f1be96c05f)
- Setelah terkoneksi semua tabel akan berada dikiri, kemudian pilih tabel yang akan digunkan dan pindah kekanan
  ![image](https://github.com/user-attachments/assets/0a50dbec-804a-4ac7-9e31-2eabd5107c94)
- Kemudian Klik Next
  ![image](https://github.com/user-attachments/assets/3be1861f-7495-4274-9e9e-4a4cd2924ee9)
- Lalu klik Finish
  ![image](https://github.com/user-attachments/assets/e06e58c2-cc46-49e6-ac44-b59c3b36bd79)

## Kode Persisten Untuk Login, Lupa Password dan buat akun user baru
- Kode Login

        private void btnLoginActionPerformed(java.awt.event.ActionEvent evt) {                                         
        if (txtUser.getText().equals("") | pwUser.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi Terlebih Dahulu");
        } else {
            EntityManagerFactory emf = Persistence.createEntityManagerFactory("TugasPersis");
            EntityManager em = emf.createEntityManager();

            em.getTransaction().begin();

            String user = txtUser.getText();
            String pw = pwUser.getText();
            Pengguna y = em.find(Pengguna.class, user);

            if (y == null) {
                JOptionPane.showMessageDialog(null, "Data User tidak ditemukan");
            } else if (y.getPassword().equals(pw)) {
                JOptionPane.showMessageDialog(null, "Selamat datang");
                ReportMataKuliah p = new ReportMataKuliah();
                p.setVisible(true);
                this.dispose();
            } else {
                JOptionPane.showMessageDialog(null, "Username atau Password salah!");
            }
            em.getTransaction().commit();
            em.close();
            emf.close();
        }
        } 

- Kode Lupa Passwoord

        private void btnLupaPasswordActionPerformed(java.awt.event.ActionEvent evt) {                                                
        if (txtPassword.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi Terlebih Dahulu");
        } else {
            EntityManagerFactory emf = Persistence.createEntityManagerFactory("TugasPersis");
            EntityManager em = emf.createEntityManager();

            try {
                em.getTransaction().begin();
                String user = txtUsername.getText();
                String pw = txtPassword.getText();

                Pengguna x = em.find(Pengguna.class, user);
                if (x != null) {
                    x.setUsername(user);
                    x.setPassword(pw);

                    em.getTransaction().commit();

                    JOptionPane.showMessageDialog(null, "Password berhasil diubah");
                    em.close();
                    emf.close();
                    Login y = new Login();
                    y.setVisible(true);
                    this.dispose();
                } else {

                    JOptionPane.showMessageDialog(null, "Username tidak ditemukan", "Error", JOptionPane.ERROR_MESSAGE);
                    em.getTransaction().rollback();
                }
            } catch (Exception ex) {

                em.getTransaction().rollback();
                ex.printStackTrace();
                JOptionPane.showMessageDialog(null, "Terjadi kesalahan saat mengubah password", "Error", JOptionPane.ERROR_MESSAGE);
            } 
        }
        }                                               


- Kode Buat akun user baru

      private void btnBuatakunActionPerformed(java.awt.event.ActionEvent evt) {                                            
       if (txtUser.getText().equals("") || pwUser.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi semua data");
        } else {
            String user, pw;
            user = txtUser.getText();
            pw = pwUser.getText();

            EntityManagerFactory emf = Persistence.createEntityManagerFactory("TugasPersis");
            EntityManager em = emf.createEntityManager();
            em.getTransaction().begin();

            Pengguna y = em.find(Pengguna.class, user);
            if (y != null) {
                JOptionPane.showMessageDialog(null, "Username sudah ada, gunakan username lain");
                //bersih();
            } else {
                Pengguna x = new Pengguna();
                x.setUsername(user);
                x.setPassword(pw);
                em.persist(x);

                em.getTransaction().commit();
                JOptionPane.showMessageDialog(null, "Sukses dibuat");

                //bersih();
                Login z = new Login();
                z.setVisible(true);
                this.dispose();
            }
            em.close();
            emf.close();
        }
        }                                           


## Kode SQl 
- Tabel mata kuliah

      CREATE TABLE mata_kuliah (
      kodemk CHAR(10) NOT NULL PRIMARY KEY,
      sks INTEGER NOT NULL,
      namamk VARCHAR(30),
      semesterajar INTEGER
      );
    
- Tabel Pengguna

      CREATE TABLE pengguna (
      username CHAR(100) PRIMARY KEY,
      password CHAR(20) NOT NULL
      );
 




