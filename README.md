# Task and Payment Modules Implementation

As part of my contributions to the freelancing platform, I developed two essential modules: **Task Management** and **Payment Processing**, both integrated seamlessly into the existing Laravel project.

## Task Management Module

This module enables project owners to create, assign, and manage tasks within projects. Freelancers can also view and update tasks assigned to them.

### Features
- **Task Creation**: Project owners can create tasks associated with specific projects and assign them to freelancers working under active contracts.
- **Task Assignment**: Tasks are linked to freelancers based on contracts, ensuring clarity in responsibilities.
- **Task Management**: Tasks can be updated with new statuses, descriptions, or due dates, and users can delete tasks when necessary.
- **Freelancer-Specific Tasks**: Freelancers can view all tasks assigned to them via a dedicated interface.

### Key Files
- `app/Http/Controllers/TaskController.php`: Contains logic for creating, assigning, viewing, editing, and deleting tasks.
- `app/Models/Task.php`: Defines the database model and relationships for tasks.
- **Views**: 
  - `resources/views/frontoffice/tasks/`: Includes Blade templates for task management.

---

## Payment Processing Module

This module facilitates secure payment handling using Stripe, ensuring a smooth transaction experience for clients and freelancers.

### Features
- **Stripe Integration**: Payments are processed through Stripe's secure API, supporting promotion codes and metadata storage.
- **Contract-Based Payments**: Payments are tied to specific contracts, ensuring financial tracking and accountability.
- **Payment Success and Error Pages**: Custom success and error messages guide users after payment attempts.
- **Admin Payment Management**: Admins can view all payments in a dedicated dashboard.

### Key Files
- `app/Http/Controllers/PaymentController.php`: Manages payment initiation, success handling, and admin payment listing.
- `app/Models/Payment.php`: Defines the database model for storing payment details.
- **Stripe Integration**: Configured in `config/services.php` and uses Stripe's PHP SDK.
- **Views**: 
  - `resources/views/admin/paying`: Templates for admin payment dashboard.
  - `resources/views/frontoffice/payment/`: Templates for payment pages.

---

## Database Changes

The implementation required several database updates, including:

- **Tasks Table**: 
  - Migration file: `database/migrations/2020_10_15_181526_create_tasks_table.php` defines the structure for task management.
- **Payments Table**: 
  - Migration file: `database/migrations/2008_10_19_203417_create_payments_table.php` defines the structure for payment tracking.

---

## Routing

Several new routes were added to facilitate these modules:

### Task Management Routes
- `GET /tasks/create{projectId}`: Render the task creation form.
- `POST /tasks/create`: Create a new task.
- `GET /tasks`: List all tasks.
- `GET /tasks/{id}`: View task details.
- `GET /tasks{id}/edit`: Edit an existing task.
- `PUT /tasks/{id}`: Update a task.
- `DELETE /tasks/{id}`: Delete a task.

### Payment Routes
- `POST /stripe/session/{contract_id}`: Initiate a payment session via Stripe.
- `GET /success`: Display the payment success page.
- `GET /cancel`: Display the payment cancellation page.
- `GET /admin/payments`: Admin dashboard to view all payments.

### Example Routes Code
```php
// Task Routes
Route::get('/tasks/create{projectId}', [TaskController::class, 'create'])->name('tasks.create');
Route::post('/tasks/create', [TaskController::class, 'store'])->name('frontoffice.tasks.store');
Route::get('/tasks', [TaskController::class, 'index'])->name('frontoffice.tasks.index');

// Payment Routes
Route::post('/stripe/session/{contract_id}', [PaymentController::class, 'session'])->name('stripe.session');
Route::get('/success', [PaymentController::class, 'success'])->name('success');
Route::prefix('admin')->group(function () {
    Route::get('/payments', [PaymentController::class, 'index'])->name('payment.index');
});
