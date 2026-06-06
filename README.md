# Custom Wishlist App (Offline-First)

A highly customizable, multi-user wishlist application designed with a **Local-First / Offline-First** architecture. Users can manage custom item structures, view images, and compare technical specs across multiple devices without losing access when internet connectivity is dropped.

---

## Key Features

*   **Offline-First Experience:** Full application functionality is maintained offline. Users can browse their wishlists, view cached images, and add or modify items without an active internet connection.
*   **Multi-Device Persistent Login:** Session-management utilizing long-lived, secure tokens (JWT) stored locally on each client device, ensuring users stay logged in across mobile and desktop environments.
*   **Dynamic Spec Templates:** Create custom category structures (e.g., Laptops, Smartphones) with user-defined attributes and data types.
*   **Background Sync Engine:** A local synchronization queue captures all offline changes and pushes them to the central backend server automatically when an internet connection is re-established.
*   **Conflict Resolution:** Built-in "Last Write Wins" protocol tracking modifications using precise `updated_at` timestamps to smoothly handle data updates from different devices.

---

## Architecture & Data Flow

### 1. Client-Side (Frontend / PWA)
*   **Local UI Storage:** Uses **IndexedDB** in the browser to act as a local shadow database, mimicking the backend structure for instant data retrieval.
*   **Service Worker:** Intercepts network requests to serve application layouts and cached images instantly.
*   **Sync Queue:** Stores offline CRUD actions (Create, Update, Delete) chronologically as a queue array, waiting for a network heartbeat to trigger an automated sync payload.

### 2. Server-Side (Backend & Database)
*   **REST API:** Python-based API engine (FastAPI / Flask) that authenticates sync batches, validates incoming item payloads, and pushes absolute server state down to matching devices.
*   **Relational Database Schema (SQLite):**
    *   **Users Table:** Standard secure credential store (`id`, `username`, `password_hash`).
    *   **Templates & TemplateFields:** Houses the blueprints for dynamic categories.
    *   **Items Table:** Wishlist metadata with an essential synchronization tracker (`id`, `user_id`, `name`, `target_price`, `image_path`, `updated_at`).
    *   **ItemValues Table:** Individual specification values mapped directly to template configurations (`item_id`, `field_id`, `value`, `updated_at`).

Please remember: This is a sceduled project!
