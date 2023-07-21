# POS

### Form Update Master
``` html
<?php
session_start();
$Lang = $_SESSION["lang"];
$ID = trim($_POST['id']);


?>

<style>
    label {
        height: 30px;
    }

    .pagination {
        display: flex;
        justify-content: center;
    }

    ul {
        margin: 0px;
    }

    td {
        vertical-align: middle;
    }

</style>



<div id="loading-spinner" class="loading-spinner">
    <div class="spinner"></div>
</div>

<input type="hidden" id="RunID" value="<?= $ID ?>">
<script>
  $(function () {
      document.title = "Edit Customer";
      document.getElementById('title_page').innerText = <? if ($Lang == 'en') { ?> "Edit Customer"; <? } else { ?> "แก้ไขลูกค้า"; <? } ?>;
      PermissionPage();
      QueryData()

  })

  function swalshow(text, type) {
      if (type == 'success') {
          return Swal.fire({
              text: text,
              icon: type,
              background: '#f5f5f5'
          })
      } else {
          return Swal.fire({
              text: text,
              icon: "warning",
              background: '#ffdddd'
          })
      }
  }

  document.getElementById('my_Form').addEventListener('keydown', function (e) {
      // Check if Enter key is pressed
      if (e.key === 'Enter') {
          e.preventDefault(); // Prevent form submission

          var inputs = Array.from(this.querySelectorAll('input, select'));
          var currentInput = e.target;
          var currentIndex = inputs.indexOf(currentInput);

          if (currentIndex < inputs.length - 1) {
              var nextInput = inputs[currentIndex + 1];
              nextInput.focus();
          } else {
              //Edit();
          }
      }
  });

  document.getElementById('my_Form').addEventListener('submit', function (e) {
      e.preventDefault(); // Prevent form submission
      //Edit();

  });

  function PermissionPage() {
      var url = "page_master/product/ProductPerm.php";
      $.ajax({
          url: url,
          type: "GET",
          dataType: 'json',
          headers: {
              'Authorization': 'Bearer ' + localStorage.getItem('token')
          },
          success: function (result, textStatus, xhr) {
              if (xhr.status === 200) {

              } else {
                  <? if ($Lang == 'en') {
                      ?>
                      swalshow('Please check the information as it is incorrect.');
                  <?
                  } else {
                      ?>
                      swalshow('กรุณาตรวจสอบข้อมูลเนื่องจากไม่ถูกต้อง');
                  <?
                  }
                  ?>
              }
          },
          error: function (xhr, textStatus, errorThrown) {
              var errorResponse = xhr.responseText;
              var jsonReponse = JSON.parse(errorResponse);
              var errorMessage = jsonReponse.message;
              if (xhr.status === 403) {
                  swalshow('Invalid request method');
              } else if (xhr.status === 401) {
                  swalshow('Invalid token');
              } else if (xhr.status === 400) {
                  if (errorMessage == 'Error Permission') {
                      window.location.href = "Home.php";
                  }
              } else {
                  swalshow('Status Error');
              }

          }
      });
  }

  function QueryData() {
      var ID = document.getElementById('RunID').value;
      var dataSet = {'ID': ID};
      var url = "page_master/customer/CustomerUpdateData.php";
      showLoadingSpinner();
      $.ajax({
          url: url,
          type: "POST",
          dataType: 'json',
          data: dataSet,
          headers: {
              'Authorization': 'Bearer ' + localStorage.getItem('token')
          },
          success: function (result, textStatus, xhr) {
              if (xhr.status === 200) {
                   document.getElementById('DiscountName').value = result['data']['Name'];
                   document.getElementById('Values').value = result['data']['Value'];
                   var Options =  result['data']['Options'];
                   var SubType =  result['data']['SubType'];
                   if(Options == 'Percent'){
                      document.getElementById("RadioType2").checked  = true;
                   }else{
                      document.getElementById("RadioType1").checked  = true;
                   }
                   if(SubType == 'true'){
                      document.getElementById("AccessRights").checked  = true;
                   }else{
                      document.getElementById("AccessRights").checked  = false;
                   }

                  hideLoadingSpinner();
              } else {
                  <? if ($Lang == 'en') {
                      ?>
                      swalshow('Please check the information as it is incorrect.');
                  <?
                  } else {
                      ?>
                      swalshow('กรุณาตรวจสอบข้อมูลเนื่องจากไม่ถูกต้อง');
                  <?
                  }
                  ?>
                  hideLoadingSpinner();
              }
          },
          error: function (xhr, textStatus, errorThrown) {
              var errorResponse = xhr.responseText;
              var jsonReponse = JSON.parse(errorResponse);
              var errorMessage = jsonReponse.message;
              if (xhr.status === 403) {
                  swalshow('Invalid request method');
              } else if (xhr.status === 401) {
                  swalshow('Invalid token');
              } else if (xhr.status === 400) {
                  if (errorMessage == 'Error Permission') {
                      window.location.href = "Home.php";
                  }
              } else {
                  swalshow('Status Error');
              }
              hideLoadingSpinner();
          }
      });
  }


  
</script>
```
