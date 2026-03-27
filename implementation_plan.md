# Razorpay Payment Flow and Receipt Implementation Plan

## Goal Description
Implement a complete end-to-end payment flow using Razorpay for traffic violation penalties. The system will automatically detect successful payments, generate a downloadable PDF receipt, and send the receipt to the user's registered email address.

## User Review Required
> [!IMPORTANT]
> 1. Do you have **Razorpay Test Keys** (Key ID and Key Secret) ready to use, or should I proceed with dummy placeholders for now?
> 2. I plan to use the `reportlab` Python library to generate the PDF receipts. Is it okay to install `reportlab` via `pip`?
> 3. Do you want the Frontend (Next.js) part of the Razorpay UI implemented now as well, or just the Backend logic first?

## Proposed Changes

### Backend (FastAPI)

#### [MODIFY] [backend/api.py](file:///c:/Users/HP/OneDrive/Desktop/projects/number%20plate%20fine/backend/api.py)
- Update `POST /user/pay/{violation_id}`: Use the official Razorpay Python client to create an order (`rzp_client.order.create`). Return the `order_id` to the frontend.
- Update `POST /user/pay-verify/{violation_id}`: Change the endpoint to accept Razorpay payment details (`razorpay_payment_id`, `razorpay_order_id`, `razorpay_signature`).
- In `pay-verify`, use `rzp_client.utility.verify_payment_signature` to automatically verify the payment authenticity.
- Upon successful verification, call a new receipt generation function.
- Update the violation status to "paid" and save the generated receipt path in the DB.
- Trigger the email sending function with the receipt attached.
- Add an endpoint `GET /receipts/{filename}` to serve the receipt file for download.

#### [MODIFY] [backend/email_sender.py](file:///c:/Users/HP/OneDrive/Desktop/projects/number%20plate%20fine/backend/email_sender.py)
- Add a new function `send_payment_receipt_email(user_email, full_name, violation_details, receipt_file_path)`.
- Use `smtplib` and `EmailMessage` to attach the PDF receipt and send a confirmation email thanking the user for their payment.

#### [NEW] `backend/receipt_generator.py`
- Create a new module using Python's `reportlab` library.
- Function `generate_receipt(violation_id, amount, date, user_name, plate_number, tx_id)` that creates a professional PDF receipt and saves it to `uploads/receipts/`.

#### [MODIFY] [backend/schemas.py](file:///c:/Users/HP/OneDrive/Desktop/projects/number%20plate%20fine/backend/schemas.py)
- Add `PaymentVerifyRequest` schema to accept Razorpay signature payloads.

### Frontend (Next.js)

#### [NEW/MODIFY] `Payment Component`
- Integrate Razorpay Checkout Script `https://checkout.razorpay.com/v1/checkout.js`.
- On clicking "Pay Now", call the backend to create an order.
- Open the Razorpay UI with the `order_id`.
- On success callback, send the signature details to `POST /user/pay-verify/{violation_id}`.
- If verified, show a success message and a "Download Receipt" button pointing to the backend's receipt URL.

## Verification Plan

### Automated Tests
- None currently set up (we will use manual verification).

### Manual Verification
1. Open the User Dashboard and click "Pay Now" on an unpaid violation.
2. Complete the Razorpay test payment flow using a dummy card.
3. Verify that the backend successfully receives and verifies the signature.
4. Check the `uploads/receipts/` directory to ensure a PDF is generated.
5. Check the user's email inbox to confirm the receipt was sent with the PDF attached.
6. Verify that the UI reflects the "paid" status and provides a working download link.
