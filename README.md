PBO TM 11
1. Buat File di excel sesuai kolom yang ada
   ![image](https://github.com/user-attachments/assets/e839e6b2-8780-4def-b497-f63597066d3b)
3. Save File menjadi CSV
   ![image](https://github.com/user-attachments/assets/57a76b78-ecfb-4628-b6a1-1c9a109770d1)
5. Tambahkan button upload 
 ![image](https://github.com/user-attachments/assets/04889a41-e0b4-4b15-ae70-d0316bdd47aa)
6. coding button upload nya 
  private void jButton1ActionPerformed(java.awt.event.ActionEvent evt) {                                         
        // TODO add your handling code here:
         JFileChooser jfc = new JFileChooser(FileSystemView.getFileSystemView().getHomeDirectory());
        int returnValue = jfc.showOpenDialog(null);
        if (returnValue == JFileChooser.APPROVE_OPTION) {
            File filePilihan = jfc.getSelectedFile();
            System.out.println("File yang dipilih : " + filePilihan.getAbsolutePath());
            try {
                BufferedReader br = new BufferedReader(new FileReader(filePilihan));
                String baris;
                String pemisah = ",";
                while ((baris = br.readLine()) != null) {
                    String[] matakuliah = baris.split(pemisah);

                    if (matakuliah.length >= 4) {
                        String kodeMK = matakuliah[0];
                        String sks = matakuliah[1];
                        String namaMK = matakuliah[2];
                        String semesterAjar = matakuliah[3];

                        if (!kodeMK.isEmpty() && !sks.isEmpty() && !namaMK.isEmpty() && !semesterAjar.isEmpty()) {
                            try {
                                Class.forName(driver);
                                conn = DriverManager.getConnection(koneksi, user, password);
                                conn.setAutoCommit(false);
                                String sql = "INSERT INTO MataKuliah (kodeMK, sks, namaMK, semesterAjar) VALUES (?, ?, ?, ?)";
                                pstmt = conn.prepareStatement(sql);

                                pstmt.setString(1, kodeMK);
                                pstmt.setInt(2, Integer.parseInt(sks));
                                pstmt.setString(3, namaMK);                   
                                pstmt.setInt(4, Integer.parseInt(semesterAjar));

                                int k = pstmt.executeUpdate();

                                if (k > 0) {
                                    conn.commit();
                                    JOptionPane.showMessageDialog(null, "Input Data Berhasil!");
                                }
                            } catch (SQLException e) {
                                JOptionPane.showMessageDialog(null, "Input Data Gagal: " + e.getMessage());
                            } catch (ClassNotFoundException ex) {
                                JOptionPane.showMessageDialog(null, "Driver tidak ditemukan: " + ex.getMessage());
                            } finally {
                                try {
                                    if (pstmt != null) {
                                        pstmt.close();
                                    }
                                    if (conn != null) {
                                        conn.close();
                                    }
                                } catch (SQLException closeEx) {
                                    JOptionPane.showMessageDialog(null, "Close Gagal: " + closeEx.getMessage());
                                }
                            }
                        }
                    } else {
                        System.out.println("Baris tidak memiliki cukup kolom: " + baris);
                    }
                }
                br.close();
            } catch (FileNotFoundException ex) {
                Logger.getLogger(DbUtils.class.getName()).log(Level.SEVERE, null, ex);
            } catch (IOException ex) {
                Logger.getLogger(DbUtils.class.getName()).log(Level.SEVERE, null, ex);
            }
        }
        tampil();
     }
7. run dan pilih upload 
 ![image](https://github.com/user-attachments/assets/d8b10270-3c9b-477a-9d9c-d8c43a0581b9)
8. lalu upload
![image](https://github.com/user-attachments/assets/f0006fbb-9786-452a-93bf-00a507dd1018)
6. tampilkan 
 ![Uploading image.png…]()

