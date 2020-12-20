<!-- SIMPAN DALAM FILE INDEX.PHP -->

</html>
<!doctype html>
<html lang="en">

<head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Roboto&display=swap" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Poppins&display=swap" rel="stylesheet">

    <!-- Font Awesome -->
    <script src="https://kit.fontawesome.com/fef9209b86.js" crossorigin="anonymous"></script>

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css" integrity="sha384-TX8t27EcRE3e/ihU7zmQxVncDAy5uIKz4rEkgIXeMed4M0jlfIDPvg6uqKI2xXr2" crossorigin="anonymous">

    <title>Validasi CFG</title>
</head>

<body>
    <!-- Start Navbar -->
    <nav class="navbar navbar-expand-lg bg-dark border-bottom">
        <div class="container">
            <ul class="navbar-nav mr-auto">
                <li>
                    <a class="navbar-brand text-white" href=""><i class="fas fa-book-reader"></i>&nbsp;Uji Validasi Context Free Grammar</a>
                </li>
            </ul>
        </div>
    </nav>
    <!-- End Navbar -->

    <div class="jumbotron jumbotron-fluid bg-white">
        <div class="container">
            <div class="card border-0 text-center">
                <div class="card-body">
                    <h3>Masukan Kalimat</h3>
                    <form action="" method="POST">
                        <input class="form-control rounded mb-3" type="text" name="teks">
                        <button class="btn btn-danger rounded" type="submit" name="submit">UJI VALIDASI</button>
                    </form>
                </div>
            </div>
            <?php
            include('kelas_kata.php');
            // saat user submit input
            if (isset($_POST['submit'])) {
                // mengambil inputan
                $teks = $_POST['teks'];
                // memecah input berdasarkan spasi
                $PecahStr = explode(" ", $teks);

                // deklasrasi variabel
                $save_kelas = array();
                $save_frasa = array();
                $save_pola = array();
                $index = 0;
                $n = 0;

                // cek pola
                foreach ($pola_kata as $pola => $frasa_kata) {
                    // cek input user
                    for ($i = $index; $i < count($PecahStr); $i++) {
                        $save_pola[$i] = "NULL";
                        $save_frasa[$i] = "NULL";
                        $save_kelas[$i] = "NULL";
                        $cek = 0;
                        // cek frasa
                        foreach ($frasa_kata as $frasa => $kelas_kata) {
                            // cek kelas
                            foreach ($kelas_kata as $kelas => $key) {
                                // cek kata dalam database
                                foreach ($key as $value) {
                                    if (strtolower($PecahStr[$i]) == $value) {
                                        $save_pola[$i] = $pola;
                                        $save_frasa[$i] = $frasa;
                                        $save_kelas[$i] = $kelas;
                                        $cek += 1;
                                        break;
                                    }
                                } //cek kata
                                if ($cek > 0) {
                                    break;
                                }
                            } //cek kelas
                            if ($cek > 0) {
                                break;
                            }
                        } //cek frasa
                        if ($cek > 0) {
                            $index += 1;
                        } else {
                            break;
                        }
                    } //cek input
                } //cek pola

                // hapus duplikat pola kalimat
                $result = array();
                foreach ($save_pola as $key => $value) {
                    if (!in_array($value, $result))
                        $result[$key] = $value;
                }

                // deklarasi rules
                $rules = array(
                    "1" => array("s", "p"),
                    "2" => array("s", "p", "o"),
                    "3" => array("s", "p", "pel"),
                    "4" => array("s", "p", "o", "pel"),
                    "5" => array("s", "p", "ket"),
                    "6" => array("s", "p", "o", "ket"),
                    "7" => array("s", "p", "pel", "ket"),
                    "8" => array("s", "p", "o", "pel", "ket")
                );

                $result = array_values($result);
                // cek apakah input valid atau tidak
                $outputValid = false;
                foreach ($rules as $rule) {
                    if ($result == $rule) {
                        $outputValid = true;
                    }
                }
            } //isset
            ?>

            <div class="container mt-5 mb-5">
                <div class="row justify-content-center">
                    <div class="col-lg-4">
                        <div class="card mb-3" style="width: 100%;">
                            <div class="card-header text-center bg-primary text-white">
                                <i class="far fa-calendar-check"></i>&nbsp;Hasil Validasi
                            </div>
                            <div class="card-body text-dark text-center">
                                <table class="table table-bordered">
                                    <tr>
                                        <td class="text-center">
                                            <?php
                                            if (isset($_POST['submit'])) {
                                                for ($i = 0; $i < count($PecahStr); $i++) {
                                                    echo ucfirst($PecahStr[$i]) . " ";
                                                }
                                            } else {
                                                echo '
                                                <h6 class="text-danger">Masukan Kalimat Terlebih Dahulu!</h6>';
                                            }
                                            ?>
                                        </td>
                                    </tr>
                                </table>
                                <?php
                                if (isset($_POST['submit'])) {
                                    if ($outputValid) {
                                        echo '
                                        <h1 class="text-success">Valid</h1>';
                                    } else {
                                        echo '
                                        <h1 class="text-danger">Tidak Valid</h1>';
                                    }
                                } else {
                                    echo '
                                    <p class="text-muted font-italic">Tidak Ada Kalimat Teruji!</p>';
                                }
                                ?>
                            </div>
                        </div>
                    </div>
                    <div class="col-lg-8">
                        <div class="card">
                            <div class="card-header text-center bg-warning text-dark">
                                <i class="fas fa-tasks"></i>&nbsp;Kelompok 2 Teori Bahasa Dan Otomata
                            </div>
                            <div class="card-body">
                                <table class="table table-bordered">
                                    <tbody>
                                        <tr>
                                            <th>1808561025</th>
                                            <td>I Kadek Agus Chandra Pradika</td>
                                        </tr>
                                        <tr>
                                            <th>1808561029</th>
                                            <td>Theresia Seftiani Girsang</td>
                                        </tr>
                                        <tr>
                                            <th>1808561034</th>
                                            <td>I Putu Sedana Wijaya</td>
                                        </tr>
                                        <tr>
                                            <th>1808561039</th>
                                            <td>Nusan Bagus Wibisana</td>
                                        </tr>
                                        <tr>
                                            <th>1808561043</th>
                                            <td>Al Habib Muhammad</td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <!-- Start Footer -->
            <div class="footer text-center">
                <h5 class="text-secondary">&copy; 2020 - All Rights Reserved by Kelompok 2 TBO</h5>
            </div>
            <!-- End Footer -->
</body>

</html>