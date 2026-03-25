# Case Study: Fundi254 (Artisan Marketplace) 🛠️

> **Status:** Private Repository (Production)
> **Role:** Project Manager & Lead Mobile Developer
> **Tech Stack:** React Native, Expo, Node.js, PostgreSQL

---

## <img width="30" height="30" alt="image" src="https://github.com/user-attachments/assets/bac7b848-2055-41e1-a40d-27995dd1cf00" /> Project Overview

Fundi254 is a **dual-role mobile marketplace** designed to connect skilled artisans (plumbers, electricians, etc.) with clients across Kenya.

The platform is built around a **trust-first architecture**, where artisans must go through identity verification before participating in the job marketplace.

The application supports:

* Role-based onboarding (Client vs Artisan)
* OTP-based authentication
* Secure identity verification workflows
* Job posting, bidding, and awarding system

---

## <img width="30" height="30" alt="image" src="https://github.com/user-attachments/assets/5781fd4a-422c-49ee-bc14-172b8781b7c1" /> My Role & Responsibilities

As the **Project Manager and Lead Mobile Developer**, I:

* Designed and implemented the **mobile application architecture**
* Led development of **authentication and onboarding flows**
* Built systems to ensure **frontend-backend state consistency**
* Translated product requirements into **scalable UI/UX flows**
* Coordinated integration with backend APIs

---

## <img width="30" height="30" alt="image" src="https://github.com/user-attachments/assets/6ec35040-eb83-42a4-9211-2fa4e5b5180b" />  Key Technical Contributions

---

### 1. Secure ID Verification System

I designed and implemented a **multi-step identity verification system** for artisans.

#### Problem

Users could drop off during onboarding, especially when uploading documents, leading to incomplete registrations.

#### Solution

* Built a **step-based verification flow** with:

  * National ID upload (front + back)
  * Liveness/selfie capture
* Implemented **local draft persistence** using `AsyncStorage`

  * Users can leave and resume without losing progress

#### Technical Highlights

* Parallel uploads using `Promise.all` for improved performance
* Structured document typing (`national_id_front`, `national_id_back`, etc.)
* Fail-safe handling for partial uploads

#### Impact

* Reduced onboarding friction
* Improved completion rates for artisan verification

---

### 2. Backend State Synchronization (Critical System)

This was one of the **core engineering challenges** in the app.

#### Problem

After submitting verification documents, the UI could show outdated status (e.g., still “not submitted”), causing confusion.

#### Solution

Implemented a **“Sync-on-Action” + “Sync-on-Mount” strategy**:

* Immediately refresh user state after critical actions
* Rehydrate state from backend when screens load

#### Code Concept

```ts
const handleSubmit = async () => {
  setSubmitting(true);

  try {
    const uploads = [];

    if (idFront) uploads.push(uploadDoc(idFront, "national_id_front"));
    if (idBack) uploads.push(uploadDoc(idBack, "national_id_back"));

    // Run uploads in parallel for performance
    await Promise.all(uploads);

    // 🔥 Critical step: Immediately sync with backend
    // Prevents stale UI state and ensures correct verification status
    await refreshUser();

    // Clean up local draft after successful sync
    await clearLocalDraft();

    setShowSuccessModal(true);
  } catch (err) {
    Alert.alert("Upload Error", "Please check your connection and try again.");
  } finally {
    setSubmitting(false);
  }
};
```

#### Why This Matters

* Ensures UI always reflects **true backend state**
* Eliminates inconsistencies between client and server
* Creates a **real-time feeling system without websockets**

---

### 3. Intelligent Rejection Handling

#### Problem

When verification failed, users didn’t know what to fix.

#### Solution

* Parsed backend rejection reasons
* Mapped them to specific UI steps
* Automatically redirected users to the exact step needing correction

#### Impact

* Reduced user frustration
* Improved re-submission success rate

---

### 4. Dynamic Category Integration

Developed a **dynamic service category system** for artisans.

#### Features

* Fetches categories from API (`/api/v1/categories`)
* Displays them as selectable UI “pills”
* Supports multi-selection

#### Technical Highlights

* Responsive UI layout
* Custom validation logic enforcing minimum selection
* Fully backend-driven (no hardcoding)

#### Impact

* Keeps app flexible as categories evolve
* Improves job matching accuracy

---

### 5. Authentication & Role-Based Access

Implemented a secure authentication system with:

* OTP-based verification (phone/email)
* PIN-based login
* Session persistence using `AsyncStorage`
* Role-based routing (Client vs Artisan)

#### Special Logic

* Artisans are restricted from bidding until verified
* Clients have full access immediately after onboarding

#### Impact

* Enforces trust within the marketplace
* Protects clients from unverified service providers

---

## 🧠 Key Engineering Principles Applied

* **State Consistency First**
  Always syncing frontend with backend to avoid stale UI

* **Resilience & Recovery**
  Users can resume interrupted flows without losing progress

* **Performance Optimization**
  Parallel uploads and minimal blocking operations

* **User-Centric Design**
  Every technical decision tied to reducing friction

---

## 📈 Overall Impact

* Built a **production-ready mobile system**
* Solved real-world problems around:

  * Identity verification
  * State synchronization
  * Multi-role user flows
* Delivered a scalable foundation for a **service marketplace platform**

---

## 🔮 Future Improvements

* Real-time updates (WebSockets)
* In-app messaging between clients and artisans
* Payment integration (escrow model)
* Ratings & review system

---

## 🧩 Summary

Fundi254 is more than a marketplace — it is a **trust-driven system** that balances:

* Secure onboarding
* Real-time system consistency
* Role-based user experiences

This project demonstrates my ability to build **production-grade mobile systems** with strong attention to **architecture, UX, and reliability**.
