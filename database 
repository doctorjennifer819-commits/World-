CREATE EXTENSION IF NOT EXISTS pgcrypto;

CREATE TABLE shipments (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  tracking_id TEXT NOT NULL UNIQUE,
  status TEXT NOT NULL DEFAULT 'Order received',
  progress_percent INTEGER NOT NULL DEFAULT 0 CHECK (progress_percent BETWEEN 0 AND 100),
  delivery_fee NUMERIC(12,2) NOT NULL DEFAULT 0 CHECK (delivery_fee >= 0),
  currency CHAR(3) NOT NULL,
  origin_country TEXT NOT NULL,
  destination_country TEXT NOT NULL,
  current_location TEXT,
  estimated_delivery DATE,
  shipment_date DATE NOT NULL DEFAULT CURRENT_DATE,
  package_type TEXT NOT NULL,
  weight_kg NUMERIC(10,2) NOT NULL CHECK (weight_kg > 0),
  sender_name TEXT NOT NULL,
  sender_phone TEXT NOT NULL,
  sender_email TEXT NOT NULL,
  sender_address TEXT NOT NULL,
  receiver_name TEXT NOT NULL,
  receiver_phone TEXT NOT NULL,
  receiver_email TEXT NOT NULL,
  receiver_address TEXT NOT NULL,
  package_image_key TEXT,
  sender_image_key TEXT,
  receiver_image_key TEXT,
  created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
  updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE tracking_events (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  shipment_id UUID NOT NULL REFERENCES shipments(id) ON DELETE CASCADE,
  status TEXT NOT NULL,
  progress_percent INTEGER NOT NULL CHECK (progress_percent BETWEEN 0 AND 100),
  location TEXT,
  note TEXT,
  occurred_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE notification_log (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  shipment_id UUID NOT NULL REFERENCES shipments(id) ON DELETE CASCADE,
  recipient_email TEXT NOT NULL,
  provider_message_id TEXT,
  outcome TEXT NOT NULL CHECK (outcome IN ('sent','failed')),
  error_message TEXT,
  created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE INDEX tracking_events_shipment_time_idx ON tracking_events(shipment_id, occurred_at);
