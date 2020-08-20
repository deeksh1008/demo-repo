#Demo
<?php 
session_start();
ob_start(); ?>

<!DOCTYPE html>
<html lang="en">
<head>
  
  <?php include 'style.css'?>
  <?php include 'links.php'?>
  <title>validation</title>
  
</head>
<body>
       <?php 
       include 'dbcon.php';

       if (isset($_POST['submit'])){
        $email = $_POST['email'];
        $password = $_POST['password'];

        $email_search = "select * from registration where email = '$email' and status = 'active'";
        $query = mysqli_query($con,$email_search);
        $email_count = mysqli_num_rows($query);

        if($email_count){
           $email_pass = mysqli_fetch_assoc($query);

           $db_pass = $email_pass['password'];

           $_SESSION['username'] = $email_pass['username'];

           $pass_decode = password_verify($password, $db_pass);
            
           if($pass_decode){
             if (isset($_POST['rememberMe'])){
               setcookie('emailcookie',$email,time()+86400);
              setcookie('passwordcookie',$password,time()+86400);


                header('Location:home.php');
             }else{
              header('Location:home.php');
             }
            echo "Login successfully";
            ?>
            <script>
              location.replace("home.php");
            </script>

            <?php
          }else{
          echo "password  incorrect";
          }


        } else{
          echo "Invalid Email";
        }

       }

       ?>
  

     <div class="card bg-light">
     <article class="card-body mx-auto" style="max-width: 400px;">
     <h4 class="card-title mt-3 text-center">Create Account</h4>
     <p class="text-center">Get started with your free account</p>
     <p>
      <a href="https://accounts.google.com/signin/v2/identifier?service=mail&passive=true&rm=false&continue=https%3A%2F%2Fmail.google.com%2Fmail%2F&ss=1&scc=1&ltmpl=default&ltmplcache=2&emr=1&osid=1&flowName=GlifWebSignIn&flowEntry=ServiceLogin" class="btn btn-block btn-gmail"> <i class = "fa fa-google"></i> Login via Gmail</a>
      <a href="https://www.facebook.com/" class="btn btn-block btn-facebook"><i class = "fa fa-facebook-f"></i> Login via Facebook</a>
    </p>
    <div class="text-center">
    <p class ="diveder-text">
      <span class="bg-light">OR</span>
    </p>
  </div>

  <div>
    <p class="bg-success text-white px-4"> <?php echo(isset($_SESSION['msg'])); ?>
  </p>
  </div>
    
      <form  method="POST">
      <div class="form-group input-group">
        <div class="input-group-prepend">
          <span class="input-group-text"><i class="fa fa-envelope"></i></span>
        </div>
        <input name="email" class="form-control" placeholder="Email-ID" value="<?php if(isset($_COOKIE['emailcookie'])) { echo $_COOKIE['emailcookie'];} ?>" type="email" required>
      </div>

      

      <div class="form-group input-group">
        <div class="input-group-prepend">
          <span class="input-group-text"><i class="fa fa-lock"></i></span>
        </div>
        <input name="password" class="form-control" placeholder="Enter your Password" type="password" value="<?php if(isset($_COOKIE['passwordcookie'])) { echo $_COOKIE['passwordcookie']; } ?>" required>
      </div>

        <div class="form-group ">
        <input  type="checkbox" name="rememberMe"> Remember Me
      </div>

      
      <div class="form-group">
        <button type = "submit" name="submit" class="btn btn-primary btn-block">Login</button>
      </div>
      <p class="text-center">Forget Password? <a href="forgot.php">Click Here </a></p>
      <p class="text-center">Not have an Account? <a href="logins.php">Sign Up</a></p>
      </form>
    </article>
  </div>

    </body>
    </html>
