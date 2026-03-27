# Traffic Enforcement System Checklist

## 1. Project Setup
- [ ] Initialize frontend application (React or Next.js)
- [ ] Initialize backend application (Python FastAPI)
- [ ] Set up Database (SQLite/PostgreSQL) and SQLAlchemy ORM

## 2. AI/ML Module (Python)
- [ ] Set up YOLOv8 for vehicle, rider, and helmet detection
- [ ] Set up Tesseract OCR for number plate reading
- [ ] Create video processing pipeline to extract frames and run inference
- [ ] Implement logic to filter detections > 80% confidence
- [ ] Save frame snapshots for detected violations

## 3. Backend Implementation (FastAPI)
- [ ] Database Models: User, Vehicle, Violation, Payment
- [ ] Authentication API: User Registration, Login (JWT)
- [ ] Admin Video Upload API: Process video, run AI script, save violations to DB
- [ ] Admin Violations API: Fetch list of violations with snapshot URLs
- [ ] Admin Penalty API: Approve penalty (assign ₹50 fine), link user via Number Plate
- [ ] Email Notification System: Send email on penalty assignment (with limits)
- [ ] User Violations API: Fetch user's own violations & profile
- [ ] Razorpay Integration: Create Order API, Verify Payment API

## 4. Admin Frontend
- [ ] Video Upload UI
- [ ] Manual Verification Table: Columns for Number Plate, Helmet Status, Confidence, Action
- [ ] Snapshot Preview Modal/Component
- [ ] Penalty Approval Logic (fetch user, assign fine, disable button if helmet=yes)

## 5. User Frontend
- [ ] Registration Page (Full Name, DOB, Email, ERP ID, Password, Number Plate)
- [ ] Login Page
- [ ] User Dashboard: Profile Info, Violations Table, Proof Images
- [ ] Payment UI: Razorpay "Pay Now" flow
- [ ] Status/Receipt Generation UI

## 6. Integration and Testing
- [ ] Connect Frontend and Backend
- [ ] E2E Testing with video
- [ ] Validate Email sending and Razorpay flows
