# 40-TECHSTACK — titipo-data-pipeline

STATUS: defined pending — STUB_UNTIL event produksi pertama disetujui TownHall.

Mengacu: TitipO-TownHall v1.0.0

## 1. Target stack

| Lapisan | Teknologi + versi | Keterangan |
|---|---|---|
| Orkestrasi | Bun 1.4.x job terjadwal | Tarik event pesanan TitipO |
| Bahasa | TypeScript 5.9.x strict | Validasi skema event |
| Antre | Postgres titipo outbox + forwarder | Sumber event pesanan selesai |
| Tujuan | Titeny intake 11 event | Tanpa tulis balik ke DB titipo |
| Serving selaras | Rust axum 0.8.4 + sqlx + uuid v7 | Pin selaras backend bila forwarder naik Rust |
| Klien baca | Kotlin 2.2.20 + Compose 1.8.2 + Navigation3 1.0.0 | Tidak membaca pipeline langsung |

STUB_UNTIL berarti event order_to_titeny masih dokumen; pengiriman produksi menunggu persetujuan skema.

## 2. Konfigurasi kunci

- Event membawa ULID, waktu UTC, versi; kirim FIFO sekali dengan kunci idempoten.
- Retry 30 detik maksimal 24 jam; setelah itu tandai gagal dan tombol kirim ulang manual.
- Tanpa PII mentah selain id anonim; foto tidak lewat pipeline.

## Batasan

- Hanya pengirim event baca; bukan pemilik pesanan, fee 12 persen, atau repeat 40 persen.
- Dilarang akses DB Lumbung langsung dan dilarang memakai JWT selain beraudien titipo di forwarder.
