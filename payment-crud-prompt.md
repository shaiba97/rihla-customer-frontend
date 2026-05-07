# Payment CRUD Operations Implementation Prompt

## Task: Implement CRUD Operations for Payment Service

You need to implement complete CRUD operations in the Payment Service with the following requirements:

## Files to Work With:
- **Primary Service**: `@/backend/apps/customer/src/booking/service/payment.service.ts`
- **DTOs for Validation**: `@/backend/apps/customer/src/booking/dto/booking.dto.ts` (use CreatePaymentDto, UpdatePaymentDto)
- **Database Model**: Payment model from `@schema.prisma`
- **PDF Service**: `@/backend/apps/customer/src/booking/service/pdf.service.ts` (for generateTicket method)

## Required Methods in Payment Service:

### 1. Create Payment
```typescript
async create(createPaymentDto: CreatePaymentDto) {
  // Create payment record in database
  // If payment is successful:
  // - Update booking status to CONFIRMED
  // - Generate ticket using pdfService.generateTicket()
  // - Return payment and ticket details
}
```

### 2. Get All Payments
```typescript
async getPayments() {
  // Return all payments with related booking data
}
```

### 3. Get Payments by Properties
```typescript
async getPaymentsByProperties(property1: string, value1: string, property2: string, value2: string) {
  // Filter payments by two properties using AND condition
  // Include related booking data
}
```

### 4. Get Payments by Single Property
```typescript
async getPaymentsByProperty(property: string, value: string) {
  // Filter payments by single property
  // Include related booking data
}
```

### 5. Get Single Payment
```typescript
async getPayment(property: string, value: string) {
  // Get single payment by property
  // Include related booking data
  // Throw NotFoundException if not found
}
```

### 6. Get Payment by Properties
```typescript
async getPaymentByProperties(property1: string, value1: string, property2: string, value2: string) {
  // Get single payment by two properties using AND condition
  // Include related booking data
  // Throw NotFoundException if not found
}
```

### 7. Update Payment
```typescript
async update(id: string, updatePaymentDto: UpdatePaymentDto) {
  // Check if payment exists
  // Update payment with provided data
  // If payment status changes to SUCCESS:
  //   - Update booking status to CONFIRMED
  //   - Generate ticket using pdfService.generateTicket()
  // Return updated payment
}
```

### 8. Delete Payment
```typescript
async delete(id: string) {
  // Check if payment exists
  // Delete payment record
  // Return success message
}
```

## Controller Endpoints to Add:

Add these endpoints to `@/backend/apps/customer/src/booking/controller/booking.controller.ts` underneath the existing booking endpoints:

```typescript
@Get('get-payments-by-property/property/:property/value/:value')
async getPaymentsByProperty(
  @Param('property') property: string,
  @Param('value') value: string,
) {
  return await this.paymentService.getPaymentsByProperty(property, value);
}

@Get('get-payment/property/:property/value/:value')
async getPayment(
  @Param('property') property: string,
  @Param('value') value: string,
) {
  return await this.paymentService.getPayment(property, value);
}

@Get(
  'get-payment-by-properties/property1/:property1/value1/:value1/property2/:property2/value2/:value2',
)
async getPaymentByProperties(
  @Param('property1') property1: string,
  @Param('value1') value1: string,
  @Param('property2') property2: string,
  @Param('value2') value2: string,
) {
  return await this.paymentService.getPaymentByProperties(
    property1,
    value1,
    property2,
    value2,
  );
}

@Put('update-payment/:id')
async updatePayment(@Param('id') id: string, @Body() body: UpdatePaymentDto) {
  return await this.paymentService.update(id, body);
}

@Delete('delete-payment/:id')
async deletePayment(@Param('id') id: string) {
  return await this.paymentService.delete(id);
}
```

## Important Requirements:

### Database Operations:
- Use PrismaService for all database operations
- Include related Booking data in all get operations
- Use proper error handling with NotFoundException and BadRequestException

### Payment Success Flow:
When payment is successful (status = SUCCESS):
1. Update the related booking status to `BookingStatus.CONFIRMED`
2. Generate ticket using `pdfService.generateTicket(bookingId)`
3. Store ticket in TicketPDF model
4. Return payment and ticket details

### Validation:
- Use DTOs from booking.dto.ts for input validation
- Validate required fields before processing
- Check if records exist before operations

### Error Handling:
- All error messages must be in Arabic:
  - "Payment not found" → "الدفعة غير موجودة"
  - "Booking not found" → "الحجز غير موجود"
  - "Payment created successfully" → "تم إنشاء الدفعة بنجاح"
  - "Payment updated successfully" → "تم تحديث الدفعة بنجاح"
  - "Payment deleted successfully" → "تم حذف الدفعة بنجاح"
  - "Invalid payment data" → "بيانات الدفعة غير صالحة"

### Database Model Reference:
Use the Payment model from schema.prisma with these key fields:
- id, bookingId, customerId, totalAmount, companyAmount, commissionAmount
- currency, status, transactionId, receiptFile, createdAt, updatedAt
- Relations: Booking, users

### Return Format:
- All get methods should include related Booking data
- Create/update methods should return the created/updated payment
- Delete methods should return success message
- Include ticket details when payment is successful

## Implementation Notes:
- Follow the existing code style and patterns in the service
- Use proper TypeScript typing
- Include proper error handling and validation
- Ensure all messages are in Arabic
- Test all CRUD operations work correctly
- Handle edge cases like duplicate payments, invalid booking IDs, etc.
