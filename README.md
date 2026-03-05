# 🔊 Text Narrator using Amazon Polly

A serverless text-to-speech application built with AWS services as part of the **Tech with Lucy** cloud course.

---

## 📌 Overview

This project converts text input into lifelike speech using **Amazon Polly** and stores the generated MP3 audio file in an **Amazon S3** bucket — all orchestrated by an **AWS Lambda** function.

---

## 🏗️ Architecture

```
User Input (Text)
       │
       ▼
  AWS Lambda  ──────►  Amazon Polly  (Text → MP3 Audio Stream)
       │
       ▼
  Amazon S3  (Stores MP3 file)
```

---

## ☁️ AWS Services Used

| Service | Purpose |
|---|---|
| **Amazon Polly** | Converts text to speech using the Joanna (Neural) voice |
| **AWS Lambda** | Serverless function to orchestrate the workflow |
| **Amazon S3** | Stores the generated MP3 audio files |
| **IAM** | Role & permissions for Lambda to access Polly and S3 |

---

## 🚀 How It Works

1. A Lambda function is triggered with a JSON payload containing a `text` field
2. The text is sent to Amazon Polly via the `SynthesizeSpeechCommand`
3. Polly returns an audio stream in **MP3 format** using the **Joanna** voice
4. The audio stream is converted to a buffer and uploaded to an **S3 bucket**
5. The file is saved with a timestamp-based name (e.g., `audio-1234567890.mp3`)

---

## 📁 Project Structure

```
text-narrator-amazon-polly/
├── lambda/
│   └── index.mjs          # Lambda function (Node.js 20.x)
├── screenshots/
│   ├── iam-role-setup.png           # IAM Role configuration
│   └── lambda-function-config.png   # Lambda function setup
└── README.md
```

---

## ⚙️ Lambda Function Details

- **Runtime:** Node.js 20.x
- **Architecture:** x86_64
- **Handler:** `index.handler`
- **IAM Role:** Custom role (`PollyTranslationRole`) with Polly and S3 permissions

### Test Event

Use this payload to test the Lambda function:

```json
{
  "text": "Hello! This is a test of Amazon Polly text to speech."
}
```

### Expected Response

```json
{
  "statusCode": 200,
  "body": "{\"message\": \"Audio stored as audio-1234567890.mp3\", \"key\": \"audio-1234567890.mp3\"}"
}
```

---

## 🛠️ Setup Instructions

### Prerequisites
- An active **AWS Account**
- IAM permissions to create Lambda functions, S3 buckets, and use Amazon Polly

---

### Step 1 — Create an S3 Bucket

1. Go to **S3** in the AWS Console
2. Click **Create bucket**
3. Enter a unique bucket name (e.g., `pollyaudiofilestorageproject3`)
4. Choose your preferred region
5. Leave other settings as default and click **Create bucket**

---

### Step 2 — Create an IAM Role

1. Go to **IAM → Roles → Create Role**
2. Trusted entity type: **AWS Service**
3. Use case: **Lambda**
4. Attach the following policies:
   - `AmazonPollyFullAccess`
   - `AmazonS3FullAccess`
5. Name the role: `PollyTranslationRole`
6. Click **Create role**

> 📸 See `<img width="624" height="373" alt="image" src="https://github.com/user-attachments/assets/2ddf677e-06d5-4fe0-b852-cc2f23ae0d27" />
` for reference.

---

### Step 3 — Create the Lambda Function

1. Go to **Lambda → Create Function**
2. Choose **Author from scratch**
3. Settings:
   - **Function name:** `TTSTranslatorfunction`
   - **Runtime:** Node.js 20.x
   - **Architecture:** x86_64
   - **Execution role:** Use an existing role → select `PollyTranslationRole`
4. Click **Create function**
5. In the code editor, paste the contents of `lambda/index.mjs`
6. Click **Deploy**

> 📸 See `<img width="763" height="378" alt="{066AA8E1-16DF-421D-9450-5E82AAB0C460}" src="https://github.com/user-attachments/assets/5f9333dc-8887-4bc5-a01e-4d27e7809a10" />
` for reference.

---

### Step 4 — Test the Function

1. In the Lambda console, click **Test**
2. Create a new test event with this payload:
   ```json
   { "text": "Hello from Amazon Polly!" }
   ```
3. Click **Test** — you should see a `200` status code response
4. Navigate to your **S3 bucket** to find the generated `.mp3` file

---

## 📸 Screenshots

### IAM Role Setup
<img width="630" height="316" alt="image" src="https://github.com/user-attachments/assets/eeefd592-b280-4521-9a25-0d48f6ee5a0a" />


### Lambda Function Configuration
<img width="763" height="378" alt="{066AA8E1-16DF-421D-9450-5E82AAB0C460}" src="https://github.com/user-attachments/assets/376032a3-764b-46cf-a562-aa98d13b5651" />


---

## 📚 Course

Built as part of the **[Tech with Lucy](https://www.techwithlucy.com/)** cloud computing course.

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
