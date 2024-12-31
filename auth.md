### **Fusion Auth Documentation**

#### **Overview**
Domain: https://auth.get-fusion.com

---

### **Endpoints**

#### **1. `/check-key`**
- **Method**: `POST`
- **Purpose**: Validates a license key and associates it with an HWID and username.
- **Request Body**:
  ```json
  {
    "Key": "string",
    "Hwid": "string",
    "Username": "string"
  }
  ```
- **Response**:
  - **Successful Validation**:
    ```json
    {
      "Valid": true
    }
    ```
  - **Failure**:
    ```json
    {
      "Valid": false
    }
    ```

#### **2. `/search-key`**
- **Method**: `POST`
- **Purpose**: Retrieves all keys associated with a specific HWID.
- **Request Body**:
  ```json
  {
    "Hwid": "string"
  }
  ```
- **Response**:
  - **Keys Found**:
    ```json
    {
      "Keys": [
        {
          "Key": "string",
          "Claimed": true,
          "ExpirationDate": "long",
          "Hwid": "string",
          "Username": "string"
        }
      ]
    }
    ```
  - **No Keys Found**:
    ```json
    {
      "Keys": []
    }
    ```

---

### **Client-Side Integration**

#### **Initialization**
Set up constants for the server URL and encryption key. Ensure the client handles encryption/decryption securely.

#### **Validating a Key**
1. Collect the user's license key, HWID (see HWID generation below), and username.
2. Send a `POST` request to `/check-key` with the collected data.
3. Handle the response to determine whether the key is valid.

#### **Searching for Keys**
1. Collect the HWID of the user’s system.
2. Optionally, provide a username.
3. Send a `POST` request to `/search-key` and process the returned list of associated keys.

#### **Error Handling**
Implement retries for network failures and error messages for invalid or expired keys.

---

### **HWID Generation**

HWIDs are unique hardware identifiers derived from system components. Fusion's Auth generates HWIDs using these properties:
1. **Processor Information**:
   - Processor ID, Name, Max Clock Speed.
2. **Motherboard Information**:
   - Serial Number, Manufacturer, Product.
3. **RAM Information**:
   - Capacity, Speed, Serial Number.
4. **System UUID**:
   - Unique identifier for the computer system.

#### **Steps to Generate HWID**
1. Query each system component using Windows Management Instrumentation (WMI).
2. Concatenate the retrieved values into a string.
3. Hash the string using SHA-256 for a consistent and secure HWID format.
