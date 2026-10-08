

## Experiment 4: Building and Running an Apex Program

**Aim:** Create a simple Apex class and run it to print a message in the debug log.

### Steps
1. Sign in to your Salesforce Developer account.
2. Click the **Gear icon (Setup)** at the top right and select **Developer Console**.
3. In the Developer Console, go to **File → New → Apex Class**.
4. Name the class `HelloWorldApp` and click **OK**.
5. Add the following code:
   ```apex
   public class HelloWorldApp {
       public static void sayHello() {
           System.debug('Welcome to Salesforce Apex');
       }
   }
   ```
6. Press `Ctrl + S` to save the class.
7. Go to **Debug → Open Execute Anonymous Window**.
8. Enter the call:
   ```apex
   HelloWorldApp.sayHello();
   ```
9. Tick **Open Log** and click **Execute**.
10. In the log tab, tick **Debug Only** to see the output message.

**Result:** The debug log shows `Welcome to Salesforce Apex`.

---

## Experiment 5: Email Service with Attachment in Apex

**Aim:** Write an Apex class that sends an email with a generated text file attached.

### Steps
1. Open the **Developer Console** from the Setup menu.
2. Go to **File → New → Apex Class**.
3. Name the class `EmailService` and click **OK**.
4. Add the following code:
   ```apex
   public class EmailService {
       public static void sendEmailWithAttachment(String recipientAddress, String subject, String bodyMessage) {
           Messaging.SingleEmailMessage mail = new Messaging.SingleEmailMessage();
           String[] toAddresses = new String[] { recipientAddress };
           mail.setToAddresses(toAddresses);
           mail.setSubject(subject);
           mail.setPlainTextBody(bodyMessage);

           Messaging.EmailFileAttachment textAttachment = new Messaging.EmailFileAttachment();
           textAttachment.setFileName('SalesforceReport.txt');
           textAttachment.setContentType('text/plain');
           String fileContentStr = 'This is the internal content of your generated text file attachment.\nCreated on: ' + System.now();
           Blob fileBodyBlob = Blob.valueOf(fileContentStr);
           textAttachment.setBody(fileBodyBlob);

           mail.setFileAttachments(new Messaging.EmailFileAttachment[] { textAttachment });
           Messaging.SendEmailResult[] results = Messaging.sendEmail(new Messaging.SingleEmailMessage[] { mail });

           if (results[0].isSuccess()) {
               System.debug('Email with attachment sent successfully to: ' + recipientAddress);
           } else {
               System.debug('Email sending failed: ' + results[0].getErrors()[0].getMessage());
           }
       }
   }
   ```
5. Press `Ctrl + S` to save the class.
6. Go to **Debug → Open Execute Anonymous Window**.
7. Enter the following, replacing the address with your own email:
   ```apex
   EmailService.sendEmailWithAttachment(
       'your_actual_email@example.com',
       'Apex Mailer with Attachment!',
       'Please check the attached text file generated directly from Salesforce.'
   );
   ```
8. Tick **Open Log** and click **Execute**.
9. In the log window, tick **Debug Only** to confirm the success message.
10. Check your inbox to verify the email arrived and download the attached text file.

**Result:** The email is delivered with `SalesforceReport.txt` attached, and the debug log confirms success.