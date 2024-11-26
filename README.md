# Ujian Akhir Semester Pemrograman Berorientasi Objek
Hallo Semua 😄👋 Pada Tugas Pemograman Berorientasi Objek ini, saya menerapkan membuat menu login menggunakan persistence program CRUD (Create, Read, Update, Delete)  dan  penggunaan jasper juga upload file csv pada java GUI(Graphical User Interfaces) dengan IDE netbeans dan yang utama disini saya menggunakan **persistence**. Untuk mengimplementasikan program tersebut saya menggunakan bahasa pemrograman java dan menggunakan aplikasi pengelola database yaitu postgreSQL dan juga plugin iReport.
dan saya menggunakan tabel  entitas Mahasiswa dengan atribut NIM, Nama, Alamat, AsalSMA, NamaOrangTua. dan entitas pengguna dengan atribut username dan password
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
-  mengecek karakter pada password

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

## Kode Import CSV dan Cetak Laporan
- Kode Import CSV

        private void jButton1ActionPerformed(java.awt.event.ActionEvent evt) {                                         
        JFileChooser jfc = new JFileChooser(FileSystemView.getFileSystemView().getHomeDirectory());
        int returnValue = jfc.showOpenDialog(null);
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("UasPBO");
        EntityManager em = emf.createEntityManager();
        if (returnValue == JFileChooser.APPROVE_OPTION) {
            File filePilihan = jfc.getSelectedFile();
            System.out.println("yang dipilih : " + filePilihan.getAbsolutePath());

            try (BufferedReader br = new BufferedReader(new FileReader(filePilihan))) {
                Class.forName(driver);
                conn = DriverManager.getConnection(koneksi, user, password);
                String baris;
                String pemisah = ";";

                while ((baris = br.readLine()) != null) {
                    String[] data = baris.split(pemisah);
                    String nim = data[0];
                    String nama = data[1];
                    String alamat = data[2];
                    String asal = data[3];
                    String ortu = data[4];
                    
                    

                    if (!nim.isEmpty() && !nama.isEmpty()&& !alamat.isEmpty()&& !asal.isEmpty()&& !ortu.isEmpty()) {
                        em.getTransaction().begin();

                        Mahasiswa ms = new Mahasiswa();
                        ms.setNim(nim);
                        ms.setNama(nama);
                        ms.setAlamat(alamat);
                        ms.setAsalsma(asal);
                        ms.setNamaorangtua(ortu);

                        em.persist(ms);

                        em.getTransaction().commit();

                    } else {
                        JOptionPane.showMessageDialog(null, "Gagal diinput");
                    }
                }
                JOptionPane.showMessageDialog(null, "Sukses diinput");
                tampil1();
                em.close();
                emf.close();
            } catch (FileNotFoundException ex) {
                JOptionPane.showMessageDialog(null, "Gagal diinput");
            } catch (IOException ex) {
                JOptionPane.showMessageDialog(null, "Gagal diinput");
            } catch (ClassNotFoundException | SQLException ex) {
            }
        }
        }

- Kode Cetak Laporan atau jasperReport

    private void btnCetakActionPerformed(java.awt.event.ActionEvent evt) {                                         
        // TODO add your handling code here:
        try {
            // TODO add your handling code here:
            conn = DriverManager.getConnection(koneksi, user, password);
            String sql = "select * from mahasiswa";
            Statement st = conn.createStatement();
            ResultSet rs = st.executeQuery(sql);
            File mahasiswa = new File(".");
            System.out.println(mahasiswa.getCanonicalPath());

            File jasperFile = new File(mahasiswa.getCanonicalPath() + "/src/UasPBO/" + "DataMahasiswa.jrxml");
            JasperDesign jd = JRXmlLoader.load(jasperFile);
            JRResultSetDataSource jds = new JRResultSetDataSource(rs);
            JasperReport jr = JasperCompileManager.compileReport(jd);
            JasperPrint jp = (JasperPrint) JasperFillManager.fillReport(jr, new HashMap<String, Object>(), jds);

            JasperViewer.viewReport(jp);

        } catch (SQLException ex) {
            Logger.getLogger(DataMahasiswa.class
                    .getName()).log(Level.SEVERE, null, ex);

        } catch (IOException ex) {
            Logger.getLogger(DataMahasiswa.class
                    .getName()).log(Level.SEVERE, null, ex);

        } catch (JRException ex) {
            Logger.getLogger(DataMahasiswa.class
                    .getName()).log(Level.SEVERE, null, ex);
        }
    }

## Kode Persisten Untuk CRUD

- Kode Tambah Data

        private void btnSimpanActionPerformed(java.awt.event.ActionEvent evt) {                                          

        EntityManagerFactory emf = Persistence.createEntityManagerFactory("UasPBO");
        EntityManager em = emf.createEntityManager();

        em.getTransaction().begin();

        Mahasiswa satu = new Mahasiswa();
        satu.setNim(txtNIM.getText());
        satu.setNama(txtNama.getText());
        satu.setAlamat(txtAlamat.getText());
        satu.setAsalsma(txtAsalSMA.getText());
        satu.setNamaorangtua(txtNamaOrtu.getText());

        em.persist(satu);

        em.getTransaction().commit();
        JOptionPane.showMessageDialog(null, "Berhasil diSimpan");
        em.close();
        emf.close();
        tampil1();
        reset();
      }
    

- Kode Tampil Data

        private void tampil1() {

        EntityManager em = Persistence.createEntityManagerFactory("UasPBO").createEntityManager();

        try {
            List<Mahasiswa> hasil = em.createNamedQuery("Mahasiswa.findAll", Mahasiswa.class).getResultList();

            DefaultTableModel tbmk = new DefaultTableModel(new String[]{"NIM", "Nama", "Alamat", "Asal Sekolah", "Nama Orang Tua"}, 0);
            for (Mahasiswa data : hasil) {
                tbmk.addRow(new Object[]{
                    data.getNim(),
                    data.getNama(),
                    data.getAlamat(),
                    data.getAsalsma(),
                    data.getNamaorangtua(),}
                );
            }
            tabelMahasiswa.setModel(tbmk);
        } finally {
            reset();
            em.close();
        }

      }
        

- Kode Update Data

        private void btnUpdateActionPerformed(java.awt.event.ActionEvent evt) {                                          
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("UasPBO");
        EntityManager em = emf.createEntityManager();

        em.getTransaction().begin();
        Mahasiswa mahasiswa = em.find(Mahasiswa.class,
                 txtNIM.getText());
        if (mahasiswa != null) {
            mahasiswa.setNama(txtNama.getText());
            mahasiswa.setAlamat(txtAlamat.getText());
            mahasiswa.setAsalsma(txtAsalSMA.getText());
            mahasiswa.setNamaorangtua(txtNamaOrtu.getText());
            

        }
        em.getTransaction().commit();
        JOptionPane.showMessageDialog(null, "Berhasil diUpdate");
        em.close();
        emf.close();
        tampil1();
        reset();
      }
  
- Kode Hapus Data

        private void btnhapusActionPerformed(java.awt.event.ActionEvent evt) {                                         
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("UasPBO");
        EntityManager em = emf.createEntityManager();

        em.getTransaction().begin();
        Mahasiswa mahasiswa = em.find(Mahasiswa.class,
                 txtNIM.getText());
        if (mahasiswa != null) {
            em.remove(mahasiswa);
        }
        em.getTransaction().commit();
        JOptionPane.showMessageDialog(null, "Berhasil dihapus");
        em.close();
        emf.close();
        tampil1();
      }
      
  

## Kode Persisten Untuk Login, Lupa Password dan buat akun user baru
- Kode Login

        private void btnLoginActionPerformed(java.awt.event.ActionEvent evt) {                                         
        if (txtUser.getText().equals("") | passField.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi Terlebih Dahulu");
        } else {
            EntityManagerFactory emf = Persistence.createEntityManagerFactory("UasPBO");
            EntityManager em = emf.createEntityManager();

            em.getTransaction().begin();

            String user = txtUser.getText();
            String pw = passField.getText();
            Pengguna y = em.find(Pengguna.class, user);

            if (y == null) {
                JOptionPane.showMessageDialog(null, "Data User tidak ditemukan");
            } else if (y.getPassword().equals(pw)) {
                JOptionPane.showMessageDialog(null, "Selamat datang");
                DataMahasiswa p = new DataMahasiswa();
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

       private void btnLogin1ActionPerformed(java.awt.event.ActionEvent evt) {                                          

        String password1 = new String(passField.getPassword());
        String password2 = new String(passField1.getPassword());
        String username = txtUsername.getText();

        
        validatePassword(password1);

        
        if (username.isEmpty() || password1.isEmpty() || password2.isEmpty()) {
            JOptionPane.showMessageDialog(null, "Semua field harus diisi", "Peringatan", JOptionPane.WARNING_MESSAGE);
            return;
        }

        if (!password1.equals(password2)) {
            jLabel12.setText("Password tidak sama");
            return;
        } else {
            jLabel12.setText("Password cocok");
        }

        EntityManagerFactory emf = Persistence.createEntityManagerFactory("UasPBO");
        EntityManager em = emf.createEntityManager();

        try {
            em.getTransaction().begin();

            Pengguna user = em.find(Pengguna.class, username);
            if (user != null) {

                user.setUsername(username);
                user.setPassword(password1);

                em.getTransaction().commit();

                JOptionPane.showMessageDialog(null, "Password berhasil diubah");
                em.close();
                emf.close();

                Login loginPage = new Login();
                loginPage.setVisible(true);
                this.dispose();
            } else {
                JOptionPane.showMessageDialog(null, "Username tidak ditemukan", "Error", JOptionPane.ERROR_MESSAGE);
                em.getTransaction().rollback();
            }
        } catch (Exception ex) {
            em.getTransaction().rollback();
            ex.printStackTrace();
            JOptionPane.showMessageDialog(null, "Terjadi kesalahan saat mengubah password", "Error", JOptionPane.ERROR_MESSAGE);
        } finally {
            if (em.isOpen()) {
                em.close();
            }
            if (emf.isOpen()) {
                emf.close();
            }
        }


      }  
                                                             


- Kode Buat akun user baru

      private void btnBuatActionPerformed(java.awt.event.ActionEvent evt) {                                        
        String password1 = new String(passField.getPassword());
        String password2 = new String(pwUser.getPassword());
        String username = txtUser.getText();

        
        validatePassword(password1);

        
        if (username.isEmpty() || password1.isEmpty() || password2.isEmpty()) {
            JOptionPane.showMessageDialog(null, "Semua field harus diisi", "Peringatan", JOptionPane.WARNING_MESSAGE);
            return;
        }

        if (!password1.equals(password2)) {
            jLabel12.setText("Password tidak sama");
            return;
        } else {
            jLabel12.setText("Password cocok");
        }
        if (txtUser.getText().equals("") || pwUser.getText().equals("") || passField.getPassword().equals("")) {
            JOptionPane.showMessageDialog(null, "Isi semua data");
        } else {
            String user, pw;
            user = txtUser.getText();
            pw = pwUser.getText();

            EntityManagerFactory emf = Persistence.createEntityManagerFactory("UasPBO");
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
- Tabel Mahasiswa

      CREATE TABLE Mahasiswa (
      nim VARCHAR(20) PRIMARY KEY NOT NULL,
      nama VARCHAR(50) NOT NULL,
      alamat VARCHAR(100) NOT NULL,
      asalsma VARCHAR(100) NOT NULL,
      namaorangtua VARCHAR(50) NOT NULL
);

    
- Tabel Pengguna

      CREATE TABLE pengguna (
      username CHAR(100) PRIMARY KEY,
      password CHAR(20) NOT NULL
      );
 




