# autoLM: AI-Driven Collective Response Synthesis

**autoLM** is an innovative web-based tool designed to synthesize responses from groups of participants using generative artificial intelligence (GenAI). It facilitates more efficient and inclusive group discussions by leveraging the OpenAI API to process and analyze multiple individual responses to open-ended questions. 

The tool is particularly valuable for educational settings, enabling instructors to quickly identify shared themes, unique contributions, and divergent perspectives. This document provides an overview of the project setup and highlights where configuration changes are necessary for deployment.

---

## **Setup Instructions**

There are two ways to use autoLM:

### **1. Hosted Option**
- Create an account at **[https://autolm.ugent.be](https://autolm.ugent.be)** to start using the platform immediately.

### **2. Local Setup (Custom Deployment)**
If you'd like to build your own instance of autoLM, follow these steps:

#### **Database Configuration**
Update the database connection parameters in `include/db.inc.php`:

```php
<?php
$conn_string = "host=localhost port=1234 dbname=your_dbname user=your_user_name password=yoursecretpassword";
$GLOBALS['dbconn'] = pg_connect($conn_string) or die("Could not connect");
?>
```
- Replace the placeholders:
  - `localhost`: Database host.
  - `1234`: Port for the database server.
  - `your_dbname`: Name of your database.
  - `your_user_name`: Database username.
  - `yoursecretpassword`: Database password.

#### **API Key Configuration**
Update the OpenAI API key in `include/startphp.inc.php`:

```php
<?php
session_start(); 
include('db.inc.php');
include('functions.inc.php');
$apiKey = 'YOUR_API_KEY';
?>
```
- Replace `YOUR_API_KEY` with your OpenAI API key.

#### **OpenAI API Parameters**
Configure the OpenAI API parameters in `ajax/admin/answer.php`:

```php
<?php
// The endpoint for the GPT model, adjust as necessary
$url = 'https://api.openai.com/v1/chat/completions';

// Prepare your prompt with the survey responses
$messages = [
    [
        "role" => "system",
        "content" => "You are a helpful assistant."
    ],
    [
        "role" => "user",
        "content" => $prompt
    ]
];

// Configure your request payload
$data = [
    "model" => "gpt-4o-mini", // Update model as needed
    "messages" => $messages,
    "max_tokens" => 200
];
?>
```
- Adjust the `model`, `max_tokens`, or other settings to suit your use case.

#### **CAS Authentication Configuration**
If you plan to build your own front-end and do not require the UGent CAS login integration, you can replace or remove the following files:

1. **`main/src/UGCAS_Simple.php`**:
   - This file handles CAS authentication for UGent users.
   - To disable CAS, you can remove any references to this file in your project or replace it with an alternative authentication mechanism.

2. **`main/logincas.php`**:
   - This script handles CAS-based login logic.
   - Modify this file if you are implementing your own login system, or remove it entirely if CAS authentication is not needed.

   If you still use CAS authentication, ensure to update the `MY_URL` constant in `logincas.php` to match your own application’s URL:

   ```php
   define('MY_URL','https://your-custom-url/logincas.php');
   ```

   For more details, refer to the `UGCAS_Simple.php` documentation comments on how CAS works and how it can be replaced.

---

#### **Server Deployment**
1. **Install Required Dependencies**:
   - Ensure your server has PHP and PostgreSQL installed.
   - Set up the database using your own schema.

2. **Deploy Files**:
   - Upload all project files to your server.

3. **Verify Configuration**:
   - Ensure the `db.inc.php` and `startphp.inc.php` files are correctly configured.
   - Test database and API connectivity.

4. **Access the Application**:
   - Open the application via the `index.php` entry point.
   - Example URL: `https://your-domain.com/index.php`

---

## **License**
This project is licensed under the [Creative Commons Attribution 4.0 International License (CC-BY)](https://creativecommons.org/licenses/by/4.0/).

You are free to:
- **Share**: Copy and redistribute the material in any medium or format.
- **Adapt**: Remix, transform, and build upon the material for any purpose, even commercially.

Under the following terms:
- **Attribution**: You must give appropriate credit, provide a link to the license, and indicate if changes were made. You may do so in any reasonable manner, but not in any way that suggests the licensor endorses you or your use.

For more details, see the [license text](https://creativecommons.org/licenses/by/4.0/).

### **Additional Terms and Conditions**

1. **OpenAI API Compliance**:
   This project uses the OpenAI API for generating responses. By using this project, you agree to comply with OpenAI’s [Terms of Use](https://openai.com/terms). Specifically:
   - You must not use the API to generate or distribute content that violates laws or ethical standards (e.g., harmful, illegal, or misleading content).
   - Responses generated by the OpenAI API are not guaranteed to be accurate, and the project authors disclaim all responsibility for the content of API-generated outputs.
   - You may not redistribute responses generated by the OpenAI API as part of a dataset or service unless explicitly allowed by OpenAI.

2. **Attribution to OpenAI**:
   If you use this project to generate outputs via the OpenAI API, you must provide attribution to OpenAI as required by their terms.

3. **No Warranty**:
   This tool is provided "as is" without any guarantees. Users are responsible for compliance with applicable laws and ethical guidelines.

---

## **Contact**
For any questions or issues, please contact the project maintainer:
- **Website**: [autoLM.ugent.be](https://autolm.ugent.be)
- **Email**: [bart.dewit@ugent.be](mailto:bart.dewit@ugent.be)

---
