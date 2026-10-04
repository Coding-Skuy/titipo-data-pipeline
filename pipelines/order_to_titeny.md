# order_to_titeny — kontrak event pesanan → Titeny

Sumber: DB `titipo` tabel `pesanan` (lihat titipo-backend-service/migrations/0001_init.sql).
Tujuan: Titeny analitik pola titip harian.

## Contoh event

```json
{
  "event_id": "3f9a8b1c-0000-4000-8000-000000000001",
  "event": "titipo.pesanan_dibuat",
  "occurred_at": "2026-10-04T07:00:00+07:00",
  "payload": {
    "id_pesanan": "3f9a8b1c-0000-4000-8000-000000000001",
    "id_vendor": "vendor-001",
    "jadwal_titip": "2026-10-04",
    "jumlah_item": 3,
    "status": "dibuat"
  }
}
```

Tanpa nama, alamat, atau nomor kontak. Titeny hanya menerima agregat + id.
