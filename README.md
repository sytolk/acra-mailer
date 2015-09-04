#ACRA Mailer [![Flattr this git repo](http://api.flattr.com/button/flattr-badge-large.png)](https://flattr.com/submit/auto?user_id=dans&url=https://github.com/d-a-n/acra-mailer&title=acra-mailer&language=&tags=github&category=software) 

Easy to use mailer that sends you all crash reports from your Android apps.

## Features
- easy to setup
- supports custom data
- nice Bootstrap e-mail layout
- secured by a shared secret

## Requirements
- webserver with PHP support or PAAS like free small basic PHP gear in http://openshift.com
- [Application Crash Report for Android](https://github.com/ACRA/acra)
- 5 minutes for setup

## Setup
1. put **acra.php** on a PHP enabled webserver
2. open **acra.php**, set a secret and save the file
```
//CONFIG
$shared_secret = "<put your shared secret here>";
```

3. download the ACRA JAR and put it in the __libs__ directory in your Android project
4. add **ACRAPostSender.java** to our project
5. open **ACRAPostSender.java** and setup the base url and the email address
```
public class ACRAPostSender implements ReportSender {
  private final static String BASE_URL = "http://example.com/acra.php?email=<your email address>";
  private final static String SHARED_SECRET = "<your shared secret>";
```
(Note: don't put any secrets in the strings.xml! That file an be read by anyone very easily.)
6. setup **ACRA** with the **ACRAPostSender** reporter
```
@Override
public void onCreate() {
		ACRA.init(this);
		super.onCreate();
    HashMap<String,String> ACRAData = new HashMap<String,String>();
    ACRAData.put("my_app_info", "custom data");
		ACRA.getErrorReporter().setReportSender(new ACRAPostSender(ACRAData));
```

* If you use PAAS for hosting PHP function mail() is not supported. PAAS providers do not have any email software installed on the servers. 
You would have to use an external SMTP server (like http://www.mailgun.com). Once you have added your domain to Mailgun you need to add DNS records to verify your domain and you will have your secret API key. [mailgun-quickstart-sending](https://documentation.mailgun.com/quickstart-sending.html#how-to-start-sending-email)
To switch email service its need to have GET or POST parameter EMAIL_SERVICE=MAILGUN and mailgun api key and registered domain:
```
if(isset($_REQUEST['EMAIL_SERVICE']) && $_REQUEST['EMAIL_SERVICE']=='MAILGUN'){
    $mg = new Mailgun("key-xxx...");
    $domain = "my-mailgun-verified-domain.com";
```
## Sample E-Mail error report

<center>
<img src="https://raw.github.com/d-a-n/acra-mailer/assets/screen.png">
</center>



