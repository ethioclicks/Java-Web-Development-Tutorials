<h4>Twilio WhatsApp</h4>
<p>The Twilio Sandbox for WhatsApp is a pre-configured environment available through the Twilio Console in which you can prototype sending outbound messages, replying to incoming messages, and configuring things like message delivery callbacks. 
The Sandbox is pre-provisioned with a Twilio phone number (+1-415-523-8886) that is shared across all sandbox users. However, other users who share the same sandbox number won't receive your messages, only the ones who have opted in to your sandbox.</p>

<h4>How to join a Twilio Sandbox</h4>
<p>To send or receive WhatsApp messages to an end user from the Sandbox, you must first have any user join the Sandbox.
First, send the message "join <your Sandbox keyword>" to your Sandbox number in WhatsApp to join your sandbox. Your Sandbox keyword is a unique string; you can find it in the Twilio Console.</p>
<p>When Twilio receives the join message, we will reply with a confirmation to that user that they have joined the Sandbox.
Upon joining your sandbox, end users will only receive messages from your specific sandbox. To disconnect from the Sandbox, they can reply to the message from WhatsApp with the word "stop". You can switch to a different Sandbox by messaging "join <other sandbox keyword>".</p>

 


![twilio](https://user-images.githubusercontent.com/80669589/147925506-4d5818a5-6fb5-4834-bcaf-8a36d0e2347f.png)



<h4>Programmable Messaging for WhatsApp</h4>
<h4>•	Get Maven Dependency</h4>
<p><dependency><br>
   <groupId>com.twilio.sdk</groupId><br>
   <artifactId>twilio</artifactId><br>
   <version>8.23.0</version><br>
</dependency></p>
<h4>•	Sample Code</h4>
<p>import com.twilio.Twilio;<br>
import com.twilio.rest.api.v2010.account.Message;<br>
import com.twilio.type.PhoneNumber;<br>
public class Example {<br>
    // Find your Account SID and Auth Token at twilio.com/console<br>
    // and set the environment variables. See http://twil.io/secure<br>
    public static final String ACCOUNT_SID = System.getenv("TWILIO_ACCOUNT_SID");<br>
    public static final String AUTH_TOKEN = System.getenv("TWILIO_AUTH_TOKEN");<br>
    public static void main(String[] args) {<br>
        Twilio.init(ACCOUNT_SID, AUTH_TOKEN);<br>
        Message message = Message.creator(<br>
                new com.twilio.type.PhoneNumber("whatsapp:to<phone number> "),<br>
                new com.twilio.type.PhoneNumber("whatsapp:+14155238886"),<br>
                "Hello there!")<br>
            .create();<br>
        System.out.println(message.getSid());<br>
    }<br>
}<br>
 </p>
