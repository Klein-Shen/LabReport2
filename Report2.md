# Report2

```
class ChatHandler implements URLHandler {
  String chatHistory = "";

  public String handleRequest(URI url) {

    // expect /chat?user=<name>&message=<string>
    if (url.getPath().equals("/chat")) {
      String[] params = url.getQuery().split("&");
      String[] shouldBeUser = params[0].split("=");
      String[] shouldBeMessage = params[1].split("=");
      if (shouldBeUser[0].equals("user") && shouldBeMessage[0].equals("message")) {
        String user = shouldBeUser[1];
        String message = shouldBeMessage[1];
        this.chatHistory += user + ": " + message + "\n\n";
        return this.chatHistory;
      } else {
        return "Invalid parameters: " + String.join("&", params);
      }
    }
    // expect /retrieve-history?file=<name>
    else if (url.getPath().equals("/retrieve-history")) {
      String[] params = url.getQuery().split("&");
      String[] shouldBeFile = params[0].split("=");
      if (shouldBeFile[0].equals("file")) {
        String fileName = shouldBeFile[1];
        // String fileName = shouldBeFileName[0]; // bug4: should be shouldBeFile[1]
        ChatHistoryReader reader = new ChatHistoryReader();
        try {
          String[] contents = reader.readFileAsArray("chathistory/" + fileName);
          for (String line : contents) {
            this.chatHistory += line + "\n\n";
          }
        } catch (IOException e) {
          System.err.println("Error reading file: " + e.getMessage());
        }
      }
      return this.chatHistory;
    }
    // expect /save?name=<name>
    else if (url.getPath().equals("/save")) {
      String[] params = url.getQuery().split("&");
      String[] shouldBeFileName = params[0].split("=");
      if (shouldBeFileName[0].equals("name")) {
        File directory = new File("chathistory");
        File file = new File(directory, shouldBeFileName[1]);

        try (BufferedWriter writer = new BufferedWriter(new FileWriter(file))) {
          writer.write(this.chatHistory);
          return "Data written to " + shouldBeFileName[1] + "in 'chat-history' folder.";
        } catch (IOException e) {
          e.printStackTrace();
          return "Error: Something wrong happen during file save, check StackTrace";
        }
      }
    }
    else if (url.getPath().equals("/")) {
      return this.chatHistory;
    }

    return "404 Not Found";
  }
}

class ChatServer {
  public static void main(String[] args) throws IOException {
    int port = Integer.parseInt(args[0]);
    Server.start(port, new ChatHandler());
  }
}

```

<img width="505" alt="20240424201038" src="https://github.com/Klein-Shen/LabReport2/assets/165833763/ae500688-0284-4eef-af1b-fec8dcccf05c">


<img width="523" alt="20240424201050" src="https://github.com/Klein-Shen/LabReport2/assets/165833763/30767ad1-db71-4e3d-ae5a-a1d0634087a8">

## Which methods in your code are called?
The code are called when java find the `query != null` is true.

## What are the relevant arguments to those methods, and the values of any relevant fields of the class?
The relevant arguments are `s` , `&` , `user` , `/add-message?` , `and `=` . 

The values of any relevant are the `parts.length`, the length of the split array for each parameter; and `query != null` when this is ture the java will run the code.
In the images the values may like "Hello", "Ningqi", and "Reese" etc.

The relevant fields of the class `chatMessages` is used to store the messages.

## How do the values of any relevant fields of the class change from this specific request? If no values got changed, explain why.
When a specific request is called, `chatMessages` adds the message and the user's name as a chat history.

If some requests do not change the value of the `chatMessages` field, it is usually because no new messages have been added.
Since this field stores all chat messages, if no new messages are added, the value of the `chatMessages` field will remain the same.

When the user refreshes the page, the browser resends the previous request, causing the same message to be added to `chatMessages` again.

## Part2
1. <img width="1038" alt="20240424211358" src="https://github.com/Klein-Shen/LabReport2/assets/165833763/2a2ef713-6b18-4260-beca-b834a99dbec5">

2. <img width="1027" alt="20240424211416" src="https://github.com/Klein-Shen/LabReport2/assets/165833763/993dd4f0-a73c-4664-94ee-5e4e39147b56">

3. <img width="478" alt="20240424211405" src="https://github.com/Klein-Shen/LabReport2/assets/165833763/ccde9f3f-8aa6-4d22-8b14-c86eccf504f9">

## Part3
When I was in the lab week2, I do not know why I still need the password after I used the `ssh-copy-id -i ~/.ssh/id_rsa.pub YOUR_TRITONLINK_USERNAME@ieng6.ucsd.edu` . 

But now, in my own computer, I do not need the password to get into the `ieng6` account.

When I write the PA from CSE12, I think it is easy, but for the code with url operations, I am still a rookie, and I could not do it myself, so I try to do it with lecture notes and what I learned in class and lab,
and asked help from ChatGPT, and moreover I do learn how this code worked, but I think I still need to study.
