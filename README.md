# phpForm
phpForm

<?php

use PHPMailer\PHPMailer\PHPMailer;
require __DIR__ . '/vendor/autoload.php';

$errors = [];
$errorMessage = '';

if (!empty($_POST)) {
    $name = $_POST['name'];
    $email = $_POST['email'];
    $message = $_POST['message'];

    if (empty($name)) {
        $errors[] = 'Name is empty';
    }

    if (empty($email)) {
        $errors[] = 'Email is empty';
    } else if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
        $errors[] = 'Email is invalid';
    }

    if (empty($message)) {
        $errors[] = 'Message is empty';
    }


    if (!empty($errors)) {
        $allErrors = join('<br/>', $errors);
        $errorMessage = "<p style='color: red;'>{$allErrors}</p>";
    } else {
        $mail = new PHPMailer();

        // specify SMTP credentials
        $mail->isSMTP();
        $mail->Host = 'smtp.mailtrap.io';
        $mail->SMTPAuth = true;
        $mail->Username = 'd5g6bc7a7dd6c7';
        $mail->Password = '27f211b3fcad87';
        $mail->SMTPSecure = 'tls';
        $mail->Port = 2525;

        $mail->setFrom($email, 'Mailtrap Website');
        $mail->addAddress('piotr@mailtrap.io', 'Me');
        $mail->Subject = 'New message from your website';

        // Enable HTML if needed
        $mail->isHTML(true);

        $bodyParagraphs = ["Name: {$name}", "Email: {$email}", "Message:", nl2br($message)];
        $body = join('<br />', $bodyParagraphs);
        $mail->Body = $body;

        echo $body;
        if($mail->send()){

            header('Location: thank-you.html'); // redirect to 'thank you' page
        } else {
            $errorMessage = 'Oops, something went wrong. Mailer Error: ' . $mail->ErrorInfo;
        }
    }
}

?>

<html>
<body>
  <form action="/swiftmailer_form.php" method="post" id="contact-form">
    <h2>Contact us</h2>

    <?php echo((!empty($errorMessage)) ? $errorMessage : '') ?>
    <p>
      <label>First Name:</label>
      <input name="name" type="text" value="dima"/>
    </p>
    <p>
      <label>Email Address:</label>
      <input style="cursor: pointer;" name="email" value="dima@dima.com" type="text"/>
    </p>
    <p>
      <label>Message:</label>
      <textarea name="message">dima</textarea>
    </p>

    <p>
      <input type="submit" value="Send"/>
    </p>
  </form>
  <script src="//cdnjs.cloudflare.com/ajax/libs/validate.js/0.13.1/validate.min.js"></script>
  <script>
      const constraints = {
          name: {
              presence: {allowEmpty: false}
          },
          email: {
              presence: {allowEmpty: false},
              email: true
          },
          message: {
              presence: {allowEmpty: false}
          }
      };

      const form = document.getElementById('contact-form');

      form.addEventListener('submit', function (event) {
          const formValues = {
              name: form.elements.name.value,
              email: form.elements.email.value,
              message: form.elements.message.value
          };

          const errors = validate(formValues, constraints);

          if (errors) {
              event.preventDefault();
              const errorMessage = Object
                  .values(errors)
                  .map(function (fieldValues) {
                      return fieldValues.join(', ')
                  })
                  .join("\n");

              alert(errorMessage);
          }
      }, false);
  </script>
</body>
</html>

-------------------------------------------------------------------------------


<form action="mail.php" method="post">
    <label for="name">Your Name</label>
    <input type="text"  name="name" placeholder="Your name..">

    <label for="lname">Email</label>
    <input type="email"  name="email" placeholder="Your email..">
   
    <label for="message">Message</label>
    <textarea  name="message" placeholder="Write something.." style="height:200px"></textarea>

    <input type="submit" value="Submit">
  </form>



  <?php
//get data from form  
$name = $_POST['name'];
$email= $_POST['email'];
$message= $_POST['message'];
$to = "youremail@mail.com";
$subject = "Mail From website";
$txt ="Name = ". $name . "\r\n  Email = " . $email . "\r\n Message =" . $message;
$headers = "From: noreply@yoursite.com" . "\r\n" .
"CC: somebodyelse@example.com";
if($email!=NULL){
    mail($to,$subject,$txt,$headers);
}
//redirect
header("Location:thankyou.html");
?>

------------------------------------------------------------------------------

<?php include 'sentMail.php';?>
 
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">  
    <form id="contact" action="" method="post">
      <h3>Quick Contact</h3>
      <h4>Contact us today, and get reply with in 24 hours!</h4>
 
      <fieldset>
        <input placeholder="Your name" name="name" type="text" tabindex="1" autofocus>
      </fieldset>
      <fieldset>
        <input placeholder="Your Email Address" name="email" type="email" tabindex="2">
      </fieldset>
      <fieldset>
        <input placeholder="Your Phone Number" name="tel" type="tel" tabindex="3" >
      </fieldset>
      <fieldset>
        <input placeholder="Type your subject line" type="text" name="subject" tabindex="4">
      </fieldset>
      <fieldset>
        <textarea name="message" placeholder="Type your Message Details Here..." tabindex="5"></textarea>
      </fieldset>
      <div>
         <p class="success"> <?php echo $success;  ?></p>
         <p class="failed"> <?php echo $failed;  ?></p>
      </div>
      <fieldset>
        <button type="submit" name="submit" id="contact-submit" data-submit="...Sending">Submit Now</button>
      </fieldset>
    </form>
    
     
  </div>
</body>
</html>










@import url(https://fonts.googleapis.com/css?family=Open+Sans:400italic,400,300,600);
 
* {
    margin:0;
    padding:0;
    box-sizing:border-box;
    -webkit-box-sizing:border-box;
    -moz-box-sizing:border-box;
    -webkit-font-smoothing:antialiased;
    -moz-font-smoothing:antialiased;
    -o-font-smoothing:antialiased;
    text-rendering:optimizeLegibility;
}
 
body {
    font-family:"Open Sans", Helvetica, Arial, sans-serif;
    font-weight:300;
    font-size: 12px;
    line-height:30px;
    color:#777;
    background:rgb(3, 153, 212);
}
 
.container {
    max-width:400px;
    width:100%;
    margin:0 auto;
    position:relative;
}
 
#contact input[type="text"], #contact input[type="email"], #contact input[type="tel"], #contact input[type="url"], #contact textarea, #contact button[type="submit"] { font:400 12px/16px "Open Sans", Helvetica, Arial, sans-serif; }
 
#contact {
    background:#F9F9F9;
    padding:25px;
    margin:50px 0;
}
 
#contact h3 {
    color: blue;
    display: block;
    font-size: 30px;
    font-weight: 700;
}
 
#contact h4 {
    margin:5px 0 15px;
    display:block;
    color: black;
    font-size:13px;
}
 
fieldset {
    border: medium none !important;
    margin: 0 0 10px;
    min-width: 100%;
    padding: 0;
    width: 100%;
}
 
#contact input[type="text"], #contact input[type="email"], #contact input[type="tel"], #contact textarea {
    width:100%;
    border:1px solid #CCC;
    background:#FFF;
    margin:0 0 5px;
    padding:10px;
}
 
#contact input[type="text"]:hover, #contact input[type="email"]:hover, #contact input[type="tel"]:hover, #contact input[type="url"]:hover, #contact textarea:hover {
    -webkit-transition:border-color 0.3s ease-in-out;
    -moz-transition:border-color 0.3s ease-in-out;
    transition:border-color 0.3s ease-in-out;
    border:1px solid #AAA;
}
 
#contact textarea {
    height:100px;
    max-width:100%;
  resize:none;
}
 
#contact button[type="submit"] {
    cursor:pointer;
    width: 100%;
    border:none;
    background:rgb(3, 153, 212);
    color:#FFF;
    margin:0 0 5px;
    padding:10px;
    font-size:15px;
}
 
#contact button[type="submit"]:hover {
    background:#09C;
    -webkit-transition:background 0.3s ease-in-out;
    -moz-transition:background 0.3s ease-in-out;
    transition:background-color 0.3s ease-in-out;
}
 
#contact button[type="submit"]:active { box-shadow:inset 0 1px 3px rgba(0, 0, 0, 0.5); }
 
#contact input:focus, #contact textarea:focus {
    outline:0;
    border:1px solid #999;
}
::-webkit-input-placeholder {
 color:#888;
}
:-moz-placeholder {
 color:#888;
}
::-moz-placeholder {
 color:#888;
}
:-ms-input-placeholder {
 color:#888;
}
 
.success{
    color: green;
    font-weight: 700;
    padding: 5px;
    text-align: center;
}
.failed{
    color: red;
    font-weight: 700;
    padding: 5px;
    text-align: center;
}




<?php  
 
if(isset($_POST['submit'])) {
 $mailto = "hmawebdesign@hotmail.com";  //My email address
 //getting customer data
 $name = $_POST['name']; //getting customer name
 $fromEmail = $_POST['email']; //getting customer email
 $phone = $_POST['tel']; //getting customer Phome number
 $subject = $_POST['subject']; //getting subject line from client
 $subject2 = "Confirmation: Message was submitted successfully | HMA WebDesign"; // For customer confirmation
 
 //Email body I will receive
 $message = "Cleint Name: " . $name . "\n"
 . "Phone Number: " . $phone . "\n\n"
 . "Client Message: " . "\n" . $_POST['message'];
 
 //Message for client confirmation
 $message2 = "Dear" . $name . "\n"
 . "Thank you for contacting us. We will get back to you shortly!" . "\n\n"
 . "You submitted the following message: " . "\n" . $_POST['message'] . "\n\n"
 . "Regards," . "\n" . "- HMA WebDesign";
 
 //Email headers
 $headers = "From: " . $fromEmail; // Client email, I will receive
 $headers2 = "From: " . $mailto; // This will receive client
 
 //PHP mailer function
 
  $result1 = mail($mailto, $subject, $message, $headers); // This email sent to My address
  $result2 = mail($fromEmail, $subject2, $message2, $headers2); //This confirmation email to client
 
  //Checking if Mails sent successfully
 
  if ($result1 && $result2) {
    $success = "Your Message was sent Successfully!";
  } else {
    $failed = "Sorry! Message was not sent, Try again Later.";
  }
 
}
 
?>
------------------------------------------------------------------------------

<form action="test.php" method="post">
<table width="400" border="0" cellspacing="2" cellpadding="0">
<tr>
<td width="29%" class="bodytext">Your name:</td>
<td width="71%"><input name="name" type="text" id="name" size="32"></td>
</tr>
<tr>
<td class="bodytext">Email address:</td>
<td><input name="email" type="text" id="email" size="32"></td>
</tr>
<tr>
<td class="bodytext">Comment:</td>
<td><textarea name="comment" cols="45" rows="6" id="comment" class="bodytext"></textarea></td>
</tr>
<tr>
<td class="bodytext"> </td>
<td align="left" valign="top"><input type="submit" name="Submit" value="Send"></td>
</tr>
</table>
</form>

<?php
$ToEmail = 'youremail@site.com';
$EmailSubject = 'Site contact form';
$mailheader = "From: ".$_POST["email"]."\r\n";
$mailheader .= "Reply-To: ".$_POST["email"]."\r\n";
$mailheader .= "Content-type: text/html; charset=iso-8859-1\r\n";
$MESSAGE_BODY = "Name: ".$_POST["name"]."";
$MESSAGE_BODY .= "Email: ".$_POST["email"]."";
$MESSAGE_BODY .= "Comment: ".nl2br($_POST["comment"])."";
mail($ToEmail, $EmailSubject, $MESSAGE_BODY, $mailheader) or die ("Failure");
?>
-----------------------------------------------------------------------------------
w3


<?php
if(isset($_POST["SubmitBtn"])){

$to = "someone@example.com";
$subject = "Contact mail";
$from=$_POST["email"];
$msg=$_POST["msg"];
$headers = "From: $from";

mail($to,$subject,$msg,$headers);
echo "Email successfully sent.";
}
?>

<form id="emailForm" name="emailForm" method="post" action="" >
<table width="100%" border="0" align="center" cellpadding="4" cellspacing="1">
<tr>
  <td colspan="2"><strong>Online Contact Form</strong></td>
</tr>
<tr>
  <td>E-mail :</td>
  <td><input name="email" type="text" id="email"></td>
</tr>
<tr>
  <td>Message :</td>
  <td>
  <textarea name="msg" cols="45" rows="5" id="msg"></textarea>
  </td>
</tr>
<tr>
  <td>&nbsp;</td>
  <td><input name="SubmitBtn" type="submit" id="SubmitBtn" value="Submit"></td>
</tr>
</table>
</form>
--------------------------------------------------


<!DOCTYPE html>
<html lang="en">
<head>
    <title>How To Create PHP Contact Form Send Email With Validation</title>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
</head>
<body>

    <?php if (isset($_POST) && !empty($_POST)) {
     $error = 1;
     $message = '';
     $sucmsg = '';
     if ($_POST['name'] = '') {
         $error = 0;
     }
     $mail = $_POST['email'];
     if (!filter_var($_POST['email'], FILTER_VALIDATE_EMAIL)) {
     }
     if ($_POST['subject'] == ' ') {
         $error = 0;
     }
     if ($_POST['message'] == ' ') {
         $error = 0;
     }
     if ($error == 1) {
         $name = $_POST['name'];
         $headers = "From:$name <$mail>" . "\r\n";
         $sbj = $_POST['subject'];
         $msg = $_POST['message'];
         if (mail("yourmail@gmail.com", "$sbj", $msg, $headers)) {
             $sucmsg = "Thank you for Contact Us !!!";
         }
     }
    } ?>

    <!-- Contact Us Form PHP Section -->
    <section class="contact-us"> 
        <div class="success-msg">
            <?php if(isset($message) && $message!=''){?>
                <?=$message; ?>
            <?php } ?>
            <?php 
             if(isset($sucmsg) && !empty($sucmsg)){
                 echo $sucmsg;
             } ?> 
        </div>

        <h1>Contact us</h1>                
        <form action="" method="post">
            <input type="text" name="name" required  class="form-control" id="name" placeholder="Your Name" />
            <input type="email" class="form-control" required name="email" id="email" placeholder="Your Email" />
            <input type="text" class="form-control" required name="subject" id="subject" placeholder="Subject" />
            <textarea class="form-control" name="message" id="message"  required  placeholder="Type Message"></textarea>
            <button type="submit">Contact Now</button>
        </form>
    </section>
    <!-- End Contact Us Form PHP Section -->
</body>
</html>




* {
    margin: 0px;
    padding: 0px;
    box-sizing: border-box;
    font-family: arial;
}
.contact-us {
    padding: 30px 20px;
    background: #efefef;
    max-width: 500px;
    margin: 100px auto 0px;
    border-radius: 10px;
    box-shadow: 0px 0px 10px 0px #bdbdbd;
}
.contact-us h1 {
    color: #000000;
    font-size: 30px;
    letter-spacing: 5px;
    margin-bottom: 20px;
}
.contact-us input, .contact-us textarea {
    background: #fafafa;
    border: 1px solid #ddd;
    border-radius: 4px;
    padding: 10px 10px;
    width: 100%;
    margin-bottom: 15px;
}
.contact-us textarea {
    max-width: 100%;
    min-height: 180px;
}
.contact-us .success-msg {
    background-color: #4CAF50;
    text-align: center;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    color: #fff;
    padding: 10px;
}
.contact-us button {
    padding: 10px 15px;
    border: 1px solid #ddd;
    background-color: #333;
    color: #fff;
    border-radius: 4px;
    margin: 0 auto;
    display: table;
    cursor: pointer;
}
-----------------------------------------------------------
<?php 
if(isset($_POST['submit'])){
    $to = "email@example.com"; // this is your Email address
    $from = $_POST['email']; // this is the sender's Email address
    $first_name = $_POST['first_name'];
    $last_name = $_POST['last_name'];
    $subject = "Form submission";
    $subject2 = "Copy of your form submission";
    $message = $first_name . " " . $last_name . " wrote the following:" . "\n\n" . $_POST['message'];
    $message2 = "Here is a copy of your message " . $first_name . "\n\n" . $_POST['message'];

    $headers = "From:" . $from;
    $headers2 = "From:" . $to;
    mail($to,$subject,$message,$headers);
    mail($from,$subject2,$message2,$headers2); // sends a copy of the message to the sender
    echo "Mail Sent. Thank you " . $first_name . ", we will contact you shortly.";
    // You can also use header('Location: thank_you.php'); to redirect to another page.
    }
?>

<!DOCTYPE html>
<head>
<title>Form submission</title>
</head>
<body>

<form action="" method="post">
First Name: <input type="text" name="first_name"><br>
Last Name: <input type="text" name="last_name"><br>
Email: <input type="text" name="email"><br>
Message:<br><textarea rows="5" name="message" cols="30"></textarea><br>
<input type="submit" name="submit" value="Submit">
</form>

</body>
</html> 
