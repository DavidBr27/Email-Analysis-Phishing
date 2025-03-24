# 🛡️ Email Header & Attachment Analysis (Phishing Case Study)

---

## ✉️ Understanding Email Headers: Simplified Guide

Let’s break it down from the top:

---

![Header screenshot](https://github.com/user-attachments/assets/053c59fc-f259-4737-a101-d8a5488ddf55)



---

### 📥 Delivered To  
Indicates the recipient’s email address. In this case, it was **themajoronar@gmail.com**.

![image](https://github.com/user-attachments/assets/dd69d577-88b7-44c5-b6c7-fd563de53632)


---



### 🏤 Received By  
These lines track the path the email took between servers — like postal checkpoints:
- The **top "Received By"** line is closest to the destination.
- The **bottom "Received"** entry shows where it originated.

---

![Received-By](https://github.com/user-attachments/assets/bc0b9ada-2533-4749-96bf-de0b96acde0c)


![Bottom](https://github.com/user-attachments/assets/f9508d7c-5d39-4030-9343-486b94ab3e7a)


### ❌ X-Headers  
Optional headers that usually provide metadata added by filters, tools, or email gateways.

---

![X-Header](https://github.com/user-attachments/assets/880c0d07-4106-4406-b4f5-919646192df3)


### 🔐 ARC Headers (Authenticated Received Chain)  
Help authenticate emails passing through intermediate systems by chaining validation signatures.

![image](https://github.com/user-attachments/assets/a2978d3e-9110-4a91-af59-44fd99705204)

---


### 🔄 Return-Path  
This is where bounce-backs (delivery failures) are sent. Often used to trace sending problems.

![return-path](https://github.com/user-attachments/assets/b9d4aaf1-a646-49a1-bb7e-0bc7f5f41a9d)

---



### 🛡️ Authentication Status  
Key verification results:
- **SPF** failed — the domain **microapple.com** didn’t authorize the server **9399.104210** to send emails.
- ⚠️ A failed SPF is often an indicator of suspicious behavior.

![image](https://github.com/user-attachments/assets/68664afd-79fe-4f0b-b2cd-68a0a583e734)

---




### 📤 Sender & Recipient Details  
- **To**: Recipient’s address.  
- **From**: Sender’s address (e.g., **microapple.com**).  
- **Reply-To**: Address for replies — this was different than "From", which is suspicious!

![image](https://github.com/user-attachments/assets/f19c2189-7793-43d7-9520-9f7cd12bb57e)

---

### 🖍️ Content-Type  
Tells how the email should be displayed (plain text, HTML, attachments).  
- **Multipart/mixed**: Includes multiple sections or formats.  
- **Boundary**: Separator for those sections.

### 🆔 Message ID  
A globally unique tag used to trace or correlate email messages.

### 🕒 Date  
Timestamp of when the message was sent — remember, this can be forged.

![image](https://github.com/user-attachments/assets/57fa9831-d461-4391-ac8c-11b9bf3eb6f4)

---

## ⚠️ Red Flags in the Email  
- **Reply-To**: Set to **@pashter.com** while "From" claimed **@micro.apple.com**.  
- 🚩 Mismatched addresses raise suspicion of spoofing attempts.

![Spoofed email](https://github.com/user-attachments/assets/0d6f15f5-a392-421c-8bc0-9d4ac234232b)


---


## 📝 Exploring the Content-Type  
- **multipart/mixed** shows multiple parts (text + files).  
- Uses **boundary** to separate them.

![image](https://github.com/user-attachments/assets/eb214c99-f739-4bb4-9d22-c466f0db36fa)

---

## 🔍 Tracing Unique Identifiers & Spoofing Tactics  
- Message IDs help trace email routes.  
- **Date fields** can be faked, so don’t rely on them blindly.

![image](https://github.com/user-attachments/assets/b804a80a-4039-4af2-a175-3ca44507b25e)

---

## 🕵️‍♂️ Exploring the Email Body

### Step 1: View the Text  
- Text was encoded in **Base64**.  
- Decoded content:  
  > “Hi TheMajorOnEarth,
  >
  > The abducted CoCanDians are with me including the President’s daughter. Dont worry. They are safe in a secret location.
  > Send me 1 Billion CoCanDs🤑 in cash💸 with a spaceship🚀 and my autonomous bots will safely bring back your citizens.
  > I heard that CoCanDians have the best brains in the Universe. Solve the puzzle I sent as an attachment for the next steps.
  > I’m approximately 12.8 light minutes away from the sun and my advice for the puzzle is
  > “Don't Trust Your Eyes”
  >
  > Lol😂
  >
  > See you Major. Waiting for the Cassshhhh💰”

![image](https://github.com/user-attachments/assets/f2337b39-ac84-44cb-a5af-caf0da372fed)

---

### Step 2: Analyze the Attachment  
- Marked as `application/pdf`.  
- Base64-encoded, but decoded into a ZIP file instead!

![image](https://github.com/user-attachments/assets/6e767471-cfbf-47a1-ae85-079eee7e419a)


---

## 📂 Attachment Inspection

- Tool: **CyberChef** ([link](https://github.com/gchq/CyberChef)) and for inspection of file signature I used ([link](https://www.garykessler.net/library/file_sigs.html))
- Analysis: The attachment was encoded in Base64, so I converted it to hexadecimal to inspect the file signature. The first four bytes of the hex reveal the true file type, helping to identify the correct file extension. 
- Result: In this case, the first four bytes were 50 4B 03 04, which indicate a ZIP archive—contrary to the Content-Type field that claimed it was a PDF.

![image](https://github.com/user-attachments/assets/59f34f5c-83ea-44ff-ae96-e7da56863834)

![image](https://github.com/user-attachments/assets/44c22005-e9be-4a0f-9cff-47f076e29513)



---

### Contents of the ZIP File  
- **daughter’s_crown.jpg**  
- **money.xlsx**

---

## 🧩 Confirming File Types

- **daughter’s_crown.jpg**: JPEG (validated by signature).  
- **money.xlsx**: Verified as Excel.

---

## 🔐 Digging into Hidden Clues

- Used a viewer like **Square X**.  
- Found Base64 hidden in Sheet 3 of **money.xlsx**:  
  > “The Martian Colony 🛸”

---

## 🔧 Base64 Decoding in CyberChef

1. Copy encoded text.  
2. Open CyberChef.  
3. Use "From Base64" tool.  
4. View output and save it.

---

## 📎 Working With Email Attachments

### Step-by-step decoding:
1. Identify the boundary.  
2. Copy the Base64 data.  
3. Decode with CyberChef.

### 🧑‍💻 File Signature Checks  
- Don’t rely on file extensions.  
- Use first bytes (e.g., `50 4B 03 04`) and match with a [signature database](https://www.garykessler.net/library/file_sigs.html).

---

## 🛡️ Safe Extraction Process

- Always use a **VM** to open suspicious files.  
- Open in a hex editor like **HxD** to check the file type.

### Signature Examples:
- `FF D8 FF E0` → JPEG  
- `25 50 44 46` → PDF

---

## 🗂️ Decoding & Reviewing Files

### File: daughter’s_crown  
- Signature: `FF D8 FF E0` → rename to `.jpg` and open.

### File: good job major  
- Signature: `25 50 44 46` → PDF with this message:  
  > “Hey Candians are safe. The proof is in *daughter’s Crown*. Send candies as shown in *money.xlsx*.”

### File: money.xlsx  
- Signature: `50 4B 03 04` → ZIP or Office XML  
- Hidden Base64 decoded to:  
  > “The Martian Colony”

---

## 🔍 Investigating money.xlsx

- Used **Square X** to explore the file.  
- Message on Sheet 3 revealed:  
  > “Whatever you’ve seen is fake. This is a declaration of war. Find me on my planet!”

---

## 🔎 Summary of Key Header Fields

1. **From**: Actual sender (not just the name).  
2. **Reply-To**: Replies go here — mismatch = red flag.  
3. **Content-Type**: Reveals message formatting.  
4. **Message ID**: Useful for tracking.  
5. **Date**: Can be spoofed.

---

## 🧠 Practice Questions

1. **What email provider was used by the attacker?**  
   > Look at the domain part of the sender’s address.

2. **What’s the Reply-To address?**  
   > This shows where replies are routed.

3. **What was the attached file type?**  
   > Look beyond the declared type — verify with tools!

4. **Who might the attacker be?**  
   > Check metadata (e.g., with `ExifTool`).

5. **Where is the attacker located?**  
   > Hidden messages may hint at fictional locations like "Martian Colony".

6. **What’s the C2 (Command-and-Control) domain?**  
   > Look for domains associated with the payload or sender.

---

### 💡 Final Tips

Stay cautious, analyze each layer, and always validate metadata, signatures, and encoding. Use a virtual machine, trusted tools, and never trust extensions or sender names alone.

---

