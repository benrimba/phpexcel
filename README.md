# Upload Data File Excel ke dalam php dan mysql
## Silahkan download library PHPExcelnya
Silahkan cek videonya di [RudiEdukasi](https://www.youtube.com/watch?v=bcQg8A68U3M)
### Source Code 
index.php
```php
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title></title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css">
  </head>
  <body>
    <br>
  <div class="row">
     <div class="col"></div>
     <div class="col">
       <div class="card">
          <div class="card-header card bg-success text-white">INPUT DATA Excel</div>
          <div class="card-body">
            <form action="proses1.php" method="POST" enctype="multipart/form-data">
               <div class="form-group">
                 <label for="email">Upload Excel</label>
                 <input type="file" name="xls">
               </div>
               <button type="submit" class="btn btn-success">Upload</button>
              </form>
          </div>
          <div class="card-footer card bg-success text-white">Footer</div>
        </div>
     </div>
     <div class="col"></div>
   </div>
  </body>
</html>
```
proses.php

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
