Panduan Instalasi dan Tutorial Hector SLAM
Hector SLAM adalah algoritma pemetaan yang hanya menggunakan informasi laser scan untuk memetakan lingkungan.
Prasyarat
•	Ubuntu 16.04
•	ROS Kinetic
•	Gazebo
Instalasi
Langkah 1: Instalasi Paket ROS TurtleBot3 (Opsional)
$ sudo apt-get install ros-kinetic-turtlebot3
Langkah 2: Instalasi Hector SLAM Menggunakan Apt Package Manager
$ sudo apt-get install ros-kinetic-hector-slam
Catatan: Hector SLAM kompatibel hanya dengan distribusi ROS Kinetic dan Indigo sesuai dengan hector_slam ROS wiki.
Dengan ini, instalasi sudah selesai. kita dapat melanjutkan ke bagian tutorial.
Tutorial
Tutorial 1: Menjalankan Hector SLAM dengan TurtleBot3 di Gazebo
Untuk informasi lebih lanjut, silakan kunjungi referensi sumber.
1.	Tentukan model TurtleBot3 yang digunakan. Pilih salah satu model.
2.	$ export TURTLEBOT3_MODEL=waffle_pi
Catatan: Perintah ini harus dieksekusi setiap kali membuka terminal baru. Tambahkan perintah tersebut ke file .bashrc untuk otomatisasi.
3.	Buka lingkungan Gazebo dengan dunia yang telah ditentukan.
4.	$ roslaunch turtlebot3_gazebo turtlebot3_world.launch
5.	Jalankan Hector SLAM.
6.	$ roslaunch turtlebot3_slam turtlebot3_slam.launch slam_methods:=hector
7.	Gerakkan TurtleBot3 menggunakan keyboard.
8.	$ roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
Anda sekarang akan melihat peta di Rviz seperti gambar berikut:
 
 
5.	Simpan peta yang telah diperoleh ke lokasi yang diinginkan. 
6.	$ rosrun map_server map_saver -f ~/map1
Tutorial 2: Menggerakkan TurtleBot3 dengan Rekaman Rosbag
Menggunakan teleoperasi setiap kali untuk menguji parameter Hector SLAM bisa menjadi kurang efisien. Anda dapat merekam gerakan TurtleBot3 dengan rosbag dan memutarnya kembali.
1.	Buka lingkungan Gazebo.
2.	$ roslaunch turtlebot3_gazebo turtlebot3_world.launch
3.	Jalankan Hector SLAM.
4.	$ roslaunch turtlebot3_slam turtlebot3_slam.launch slam_methods:=hector
5.	Arahkan ke file rosbag yang merekam gerakan TurtleBot3.
6.	$ cd oko_slam/ros_ws/bagfiles
7.	Putar file rosbag.
8.	$ rosbag play turtlebot3_movement.bag
Catatan: Jika menggunakan model lain selain waffle_pi, simpan file rosbag Anda sendiri.
Tutorial 3: Kustomisasi Parameter LIDAR di Gazebo
Jika menggunakan LIDAR baru atau berbeda, Anda dapat memodifikasi parameter LIDAR di simulasi.
1.	Arahkan ke direktori deskripsi TurtleBot3.
2.	$ roscd turtlebot3_description
3.	Buka direktori urdf dan edit file xacro.
4.	$ cd urdf & gedit turtlebot3_waffle_pi.gazebo.xacro
Catatan: Simpan salinan file asli untuk menjaga nilai default.
3.	Ubah parameter LIDAR sesuai kebutuhan seperti contoh berikut:
 
Tutorial 4: Menjalankan Hector SLAM dengan Data Sendiri Tanpa Gazebo
1.	Buka file peluncuran bawaan dari paket Hector SLAM.
2.	$ roscd hector_slam_launch
3.	$ cd launch
4.	$ gedit tutorial.launch
5.	File ini memanggil mapping_default.launch. Buka file tersebut.
6.	$ roscd hector_mapping
7.	$ cd launch
8.	$ gedit mapping_default.launch
9.	Analisis dan ubah parameter yang diperlukan, seperti resolusi peta.
10.	Jalankan file peluncuran.
11.	$ roslaunch hector_slam_launch tutorial.launch
12.	Putar file rosbag yang Anda rekam.
13.	$ cd oko_slam/ros_ws/bagfiles/simulation_data
14.	$ rosbag play turtlebot3_scan2.bag --clock
Jika berhasil, Anda akan melihat output Rviz seperti ini:
 
Catatan Penting: Hector SLAM memerlukan konfigurasi transformasi (tf) yang spesifik. Jika tidak ada output di Rviz, sesuaikan tf tree Anda. Periksa file peluncuran kami seperti oko_slam.launch dan oko_hector_mapping.launch untuk konfigurasi transformasi yang benar.
$ roslaunch kamu_robotu_launch oko_slam.launch sim_time:=true slam_type:=hector_mapping
Putar kembali file rosbag.
Konfigurasi tf tree yang benar:
 
Catatan: Gunakan parameter /use_sim_time sebagai false untuk data waktu nyata dan true untuk file rosbag.
