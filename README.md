# Upload Data File Excel ke dalam php dan mysql
## Silahkan download liberary PHPExcelnya
Silahkan cek videonya di [RudiEdukasi](https://www.youtube.com/watch?v=bcQg8A68U3M)
Source Code 
```php
<?php
  $con = new mysqli('localhost','root','','dbupload');
  $fileUpl = $_FILES['xls']['name'];
  require_once 'Classes/PHPExcel/Reader/Excel5.php';
  $objReader = new PHPExcel_Reader_Excel5($fileUpl);
  $data = $objReader->load($_FILES['xls']['tmp_name']);
  $objData = $data->getActiveSheet();
  $dataArray = $objData->toArray();
    for ($i=1; $i < count($dataArray) ; $i++) {
        $objData = $data->getActiveSheet();
        foreach ($objData->getDrawingCollection() as $gbr) {
          $string = $gbr->getCoordinates();
          $coord = PHPExcel_Cell::coordinateFromString($string);
          $image = $gbr->getImageResource();
          $img = $gbr->getIndexedFilename();
          imagejpeg($image, 'gambar/'. $img);
        }
      $nama = $dataArray[$i]['1'];
      $sql = $con->query("INSERT INTO tbgambar VALUES('','$nama','$img')");
    }
    if ($sql) {
     echo"Berhasil";
    }
 ?>
```
