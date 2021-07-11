
<?php
    $json = file_get_contents('php://input');
    $data = (array)json_decode($json, true);
    $returndata = array();
    $returndata["Account"] = "";
    $returndata["ShowLog"] = "";

    //Check Empty Space
    if($data["account"] == "" || $data["password"] == ""){
        $returndata["ShowLog"] = "Account Or password is Empty!";
        echo json_encode($returndata,true);
        return;
    }
    //Connect to server
    $con = mysqli_connect('localhost','gaia2222','gaia2222','mydb');
    if (!$con) {
        die('Could not connect: ' . mysqli_error($con));
    }
    //Check Repeat Name
    $sql= 'SELECT account FROM loginform WHERE account = "'.$data["account"].'";';
    $result = mysqli_query($con,$sql);
    if(mysqli_num_rows($result) > 0){
        $returndata["ShowLog"] = "Account name has been used";  
        echo json_encode($returndata,true);
        mysqli_close($con);
        return;
    }
    //Insert to jquery
    $sql = "INSERT INTO loginForm (account, password) VALUES ('".$data["account"]."', '".$data["password"]."')";

    if (mysqli_query($con, $sql)) {
    
    
        $returndata["Account"] = $data["account"];
        $returndata["ShowLog"] = "Oh! Your register success";

        echo json_encode($returndata,true);

    } else {
        echo "Error: " . $sql . "<br>" . mysqli_error($con);
    }
    mysqli_close($con);
?>
