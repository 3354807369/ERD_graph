erDiagram
  TENANTS ||--o{ USERS : has
  TENANTS ||--o{ GOOGLE_ACCOUNTS : has
  GOOGLE_ACCOUNTS ||--|| OAUTH_TOKENS : has
  GOOGLE_ACCOUNTS ||--|| GMAIL_SYNC_STATE : has

  GOOGLE_ACCOUNTS ||--o{ EMAIL_THREADS : has
  EMAIL_THREADS ||--o{ EMAIL_MESSAGES : has

  EMAIL_MESSAGES ||--o{ PRIVACY_HOLDING : flagged
  EMAIL_MESSAGES ||--o{ ATTACHMENTS : has
  EMAIL_MESSAGES ||--o{ AI_ARTIFACTS : produces
  USERS ||--o{ TASKS : owns
  TASKS ||--o{ SCHEDULED_JOBS : schedules
  SCHEDULED_JOBS ||--o{ JOB_RUNS : runs

  TENANTS {
    uuid id
    text name
  }
  USERS {
    uuid id
    uuid tenant_id
    text email
  }
  GOOGLE_ACCOUNTS {
    uuid id
    uuid tenant_id
    uuid user_id
    text google_email
  }
  EMAIL_THREADS {
    uuid id
    uuid tenant_id
    uuid google_account_id
    text gmail_thread_id
  }
  EMAIL_MESSAGES {
    uuid id
    uuid tenant_id
    uuid google_account_id
    uuid thread_id
    text gmail_message_id
  }
