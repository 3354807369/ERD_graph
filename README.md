erDiagram
  TENANTS ||--o{ USERS : has
  TENANTS ||--o{ GOOGLE_ACCOUNTS : has
  GOOGLE_ACCOUNTS ||--|| OAUTH_TOKENS : has
  GOOGLE_ACCOUNTS ||--|| GMAIL_SYNC_STATE : has

  GOOGLE_ACCOUNTS ||--o{ EMAIL_THREADS : has
  EMAIL_THREADS ||--o{ EMAIL_MESSAGES : has

  EMAIL_MESSAGES ||--o{ ATTACHMENTS : has
  EMAIL_MESSAGES ||--o{ PRIVACY_HOLDING : flagged
  EMAIL_MESSAGES ||--o{ AI_ARTIFACTS : produces
  EMAIL_MESSAGES ||--o{ EMAIL_EMBEDDINGS : embeds

  GOOGLE_ACCOUNTS ||--o{ EMAIL_LABELS : has
  EMAIL_MESSAGES ||--o{ EMAIL_MESSAGE_LABELS : has
  EMAIL_LABELS ||--o{ EMAIL_MESSAGE_LABELS : links

  TENANTS ||--o{ AUDIT_LOGS : records
  USERS ||--o{ AUDIT_LOGS : acts

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
    text name
  }
  GOOGLE_ACCOUNTS {
    uuid id
    uuid tenant_id
    uuid user_id
    text google_email
  }
  OAUTH_TOKENS {
    uuid id
    uuid google_account_id
    text access_token_encrypted
    text refresh_token_encrypted
    datetime expires_at
  }
  GMAIL_SYNC_STATE {
    uuid id
    uuid google_account_id
    text history_id
    datetime last_sync_at
    text status
  }
  EMAIL_THREADS {
    uuid id
    uuid tenant_id
    uuid google_account_id
    text gmail_thread_id
    datetime last_message_at
  }
  EMAIL_MESSAGES {
    uuid id
    uuid tenant_id
    uuid google_account_id
    uuid thread_id
    text gmail_message_id
    datetime internal_date
    text subject
    text snippet
    text body_clean
    boolean needs_privacy_review
  }
  EMAIL_LABELS {
    uuid id
    uuid tenant_id
    uuid google_account_id
    text gmail_label_id
    text name
  }
  EMAIL_MESSAGE_LABELS {
    uuid id
    uuid tenant_id
    uuid message_id
    uuid label_id
  }
  ATTACHMENTS {
    uuid id
    uuid tenant_id
    uuid message_id
    text filename
    text mime_type
    int size_bytes
    text storage_key
    text sha256
    uuid extracted_text_artifact_id
  }
  PRIVACY_HOLDING {
    uuid id
    uuid tenant_id
    uuid message_id
    text status
    text matched_terms
    datetime created_at
    datetime reviewed_at
    uuid reviewed_by_user_id
  }
  AI_ARTIFACTS {
    uuid id
    uuid tenant_id
    text source_type
    uuid source_id
    text artifact_type
    text content_json
    text model_name
    text prompt_version
    datetime created_at
  }
  EMAIL_EMBEDDINGS {
    uuid id
    uuid tenant_id
    uuid message_id
    int chunk_index
    text model_name
    datetime created_at
  }
  TASKS {
    uuid id
    uuid tenant_id
    uuid user_id
    text title
    text description
    datetime due_at
    text status
  }
  SCHEDULED_JOBS {
    uuid id
    uuid tenant_id
    uuid user_id
    text job_type
    datetime run_at
    text status
    text payload_json
  }
  JOB_RUNS {
    uuid id
    uuid scheduled_job_id
    datetime started_at
    datetime finished_at
    text status
    text error_message
  }
  AUDIT_LOGS {
    uuid id
    uuid tenant_id
    uuid actor_user_id
    text action_type
    text object_type
    uuid object_id
    text metadata_json
    datetime created_at
  }
