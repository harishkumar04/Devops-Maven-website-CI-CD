
# 🚀 Setting Up My AWS CodePipeline with GitHub Integration

## 🔧 Step 1: Creating the Custom Pipeline

First, I opened the **AWS CodePipeline** console and selected **“Build custom pipeline”**.

Then, I chose **"Superseded"** as the **Execution Mode**.
This mode helps ensure that if multiple commits are pushed quickly, only the most recent one gets deployed, and all older ones are automatically skipped.

<img width="579" height="500" alt="Screenshot 2025-07-25 at 10 55 41 AM" src="https://github.com/user-attachments/assets/5b27571f-6a32-41fc-bc05-eb35facafab8" />

---

## 🔁 Step 2: Build and Deploy Stage

I kept the **WebHook option checked** because I wanted the pipeline to react to changes in real time.
This means whenever I push code to GitHub, the pipeline is triggered automatically without any manual intervention.

<img width="591" height="527" alt="Screenshot 2025-07-25 at 10 56 42 AM" src="https://github.com/user-attachments/assets/fc5a18d6-1251-4176-afa2-22097c6d5393" />

Now, when it comes to Deloy Stage make sure to choose the Deployment group correctly. And, make sure the **Automatic Rollback on Stage failure** on.

<img width="590" height="529" alt="Screenshot 2025-07-25 at 10 58 22 AM" src="https://github.com/user-attachments/assets/e4453445-1d3f-4157-affb-42fd99c035f9" />

---

## 🚫 Step 3: Skipping the Test Stage

Since I hadn’t set up any automated tests at this point, I simply **skipped the test stage** during pipeline configuration.

---

## ✅ Step 4: Creating the Pipeline

After I completed all the configuration steps for source, build, and deploy stages, I clicked **“Create Pipeline”** to finalize everything.

<img width="592" height="231" alt="Screenshot 2025-07-25 at 11 00 11 AM" src="https://github.com/user-attachments/assets/4758e050-ec98-45bd-93ab-0920146fe06d" />

---

## 📝 Step 5: Committing Changes to GitHub

To test if the pipeline was working correctly, I made a change in my `index.jsp` file and committed it to GitHub.

Here’s what I did:

```bash
git add index.jsp
git commit -m "Updated UI in index.jsp"
git push origin main
```
<img width="595" height="28" alt="Screenshot 2025-07-25 at 11 00 43 AM" src="https://github.com/user-attachments/assets/af3a277a-749f-4df4-94ba-7fb4d73d8362" />

---

## ✅ Step 6: Watching the Pipeline in Action

Once I pushed the code, the pipeline picked it up automatically.
Each stage of the pipeline executed as expected, and I was able to see the **commit message** I had used in GitHub reflected in the pipeline UI.

This confirmed that my pipeline was successfully connected and automated.

<img width="594" height="237" alt="Screenshot 2025-07-25 at 11 00 59 AM" src="https://github.com/user-attachments/assets/9e00083d-8307-4a25-80d1-2ef6a2b7bb43" />

---

## Result

With this setup, I now have a fully functional AWS CodePipeline that deploys my code automatically every time I push changes to GitHub. It's efficient, fast, and reduces manual work.

---
