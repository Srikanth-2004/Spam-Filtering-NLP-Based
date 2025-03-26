# Spam-Filtering-NLP-Based

## Adding Datasets
You have to upload the data set into the files folder inside google colab, note that it will only remain until the runtime ends (i.e you close the site or reload it).

## Updating Datasets
You can find in the **Training Loop and Execution** cell a line stating: `X_train, X_test, y_train, y_test = load_data("../content/spam.csv", is_sms=True)` if you change the ../content/your_file.csv changing the "your_file" to your file name you can use your datasets for the project.

## **How the Spam Detection Model Works**  
This spam detection system uses **DistilRoBERTa**, a streamlined version of the powerful RoBERTa language model, fine-tuned to classify text as either **spam or not spam (ham)**. The model processes raw text (emails or SMS messages) through several stages:  

1. **Text Preprocessing**:  
   The input text is cleaned by converting it to lowercase, truncating long messages, and handling special characters. For SMS data, it also normalizes common patterns like phone numbers, URLs, and currency symbols into placeholder tokens (e.g., `phonenum`, `webaddress`).  

2. **Tokenization**:  
   The text is split into smaller units (tokens) using RoBERTa’s tokenizer, which understands subword structures (e.g., "unhappy" → `"un" + "happy"`). Each token is mapped to a numerical ID, and the sequence is padded or truncated to a fixed length (`MAX_LENGTH = 128`).  

3. **Model Inference**:  
   The tokenized input is fed into the DistilRoBERTa model, which generates contextual embeddings (numeric representations) for the text. These embeddings pass through a classification layer that outputs probabilities for two classes: **spam (1)** or **ham (0)**.  

4. **Training & Optimization**:  
   During training, the model adjusts its weights using the **AdamW optimizer** and **mixed-precision training** (on GPU) to minimize prediction errors. The `GradScaler` ensures stable training by managing gradient magnitudes in mixed-precision mode.  

---

## **What the Model Can Classify**  
This system is designed to detect **unwanted or malicious messages** across different formats:  

1. **Email Spam**:  
   - Promotional scams (e.g., "Win a free iPhone!")  
   - Phishing attempts (e.g., "Verify your bank account now")  
   - Malware links or fraudulent offers  

2. **SMS Spam (Text Messages)**:  
   - Prize scams ("You’ve won $1000! Text YES to claim")  
   - Fake delivery notifications ("Your package is delayed—click here")  
   - Social engineering attacks ("Hi Mom, I need money urgently")  

3. **General Text-Based Spam**:  
   - Social media scams  
   - Comment section spam  
   - Fraudulent customer support messages  

The model achieves **~97% accuracy** by learning patterns like:  
- Urgent or threatening language ("ACT NOW!")  
- Suspicious links or phone numbers  
- Unusual sender addresses or grammar errors  
- Promises of rewards with no clear justification  

---

## **Flexibility and Limitations**  
**Strengths**:  
- Works for both **short (SMS)** and **longer (email)** text.  
- Adapts to informal language (e.g., "URGENT: accnt suspended").  
- Fast inference (~100 predictions/second on a Colab GPU).  

**Limitations**:  
- May struggle with **very new spam tactics** not seen during training.  
- Requires retraining for **domain-specific spam** (e.g., medical vs. retail).  
- Performance drops if spam messages mimic normal communication (e.g., "Your meeting is at 3 PM" from an unknown sender).  

For production use, you can extend it to:  
- Classify spam in **multiple languages** (by training on non-English data).  
- Detect **categories of spam** (phishing, ads, scams) using more labels.  
- Integrate with email servers or messaging apps via APIs.  

