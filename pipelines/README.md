# pipelines/ — Event pesanan TitipO → Titeny

Jadwal awal: batch harian 01:00 WIB + streaming near-real-time untuk `pesanan_dibuat`.

## Daftar

1. `order_to_titeny` — kirim event pesanan ke Titeny (lihat `order_to_titeny.md`).
2. `subscription_snapshot` (direncanakan) — snapshot langganan aktif harian.

## Konvensi

- Format: JSON, `event`, `occurred_at` (RFC3339), `payload`.
- Idempotensi via `event_id` (UUID).
- Auth ke Titeny memakai service key TitipO, rotasi 90 hari.
