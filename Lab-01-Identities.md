# Lab 01: Identity Administration in Azure (Microsoft Entra ID)

## 📌 Lab Objective
Create and configure user accounts and organizational profiles within Microsoft Entra ID, applying industry best practices for identity and access control.

## 🛠️ Steps Performed
1. **Portal Access:** Initial environment setup and exploration of active tenants in Microsoft Entra ID.
2. **User Creation:** 
   - **User Principal Name:** `az104-user1`
   - **Display Name:** `az104-user1`
   - **Additional Properties:** Configured Job Title as *IT Lab Administrator*, Department as *IT*, and Usage Location as *United States*.
3. **Validation:** Confirmed successful account creation and active status.

## 📸 Visual Evidence
<img width="1899" height="626" alt="CreateUsersEntraID" src="https://github.com/user-attachments/assets/bd6fec93-bdeb-4c1e-82fd-101e49c9bd0b" />

## 🧠 Key Learnings & Architecture Notes
- Microsoft Entra ID acts as the centralized identity directory for all Azure cloud services.
- Correctly configuring user attributes (such as department and location) is essential for future access control, compliance, and automated policies.

---

## 👥 Task 2: Inviting an External User (Microsoft Entra B2B)

### 📌 Objective
Learn how to securely collaborate with external partners, vendors, or contractors by inviting external identities into the organization's tenant.

### 🛠️ Steps Performed
1. **External User Invitation:** 
   - **Email:** Configured with a personal/external email address.
   - **Display Name:** Configured with user name.
   - **Invitation Message:** Sent a custom welcome message ("*Welcome to Azure and our group project*").
2. **Organizational Properties:** 
   - **Job Title:** *IT Lab Administrator*
   - **Department:** *IT*
   - **Usage Location:** *United States*
3. **Validation:** Confirmed the invitation status and verified the receipt of the external invitation email.

### 📸 Visual Evidence
<img width="1909" height="607" alt="CreateGuestUsersEntraID" src="https://github.com/user-attachments/assets/6b9e7fbe-ead8-4ab6-99f3-25067c2d654d" />

### 🧠 Key Learnings & Architecture Notes
- **Entra B2B Collaboration:** Allows organizations to safely share applications and services with external users while maintaining control over corporate data.
- **Controlled Access:** External users retain their own corporate or personal credentials; they do not require a separate managed account inside your directory.
