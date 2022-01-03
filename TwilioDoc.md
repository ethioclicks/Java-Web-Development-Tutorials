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
<dependency>
   <groupId>com.twilio.sdk</groupId>
   <artifactId>twilio</artifactId>
   <version>8.23.0</version>
</dependency>
•	Sample Code
import com.twilio.Twilio;
import com.twilio.rest.api.v2010.account.Message;
import com.twilio.type.PhoneNumber;
public class Example {
    // Find your Account SID and Auth Token at twilio.com/console
    // and set the environment variables. See http://twil.io/secure
    public static final String ACCOUNT_SID = System.getenv("TWILIO_ACCOUNT_SID");
    public static final String AUTH_TOKEN = System.getenv("TWILIO_AUTH_TOKEN");
    public static void main(String[] args) {
        Twilio.init(ACCOUNT_SID, AUTH_TOKEN);
        Message message = Message.creator(
                new com.twilio.type.PhoneNumber("whatsapp:to<phone number> "),
                new com.twilio.type.PhoneNumber("whatsapp:+14155238886"),
                "Hello there!")
            .create();
        System.out.println(message.getSid());
    }
}
