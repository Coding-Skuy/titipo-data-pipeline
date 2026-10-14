# ADR-0001 — Fondasi teknis titipo-data-pipeline

Status: Diterima | Tanggal: 2026-10-14

## Konteks

titipo-data-pipeline adalah bagian divisi TitipO (Epic E2 #3, milestone v1.0.0). Spesifikasi v1.0.0 beku sejak 2026-10-07 di [TitipO-TownHall](https://github.com/Coding-Skuy/TitipO-TownHall/blob/main/platform/versions/v1.0.0/FSD/30-kontrak.md). Repo ini memerlukan fondasi teknis yang tetap agar pengembangan oleh AI dan manusia konsisten.

## Keputusan

Keputusan kunci CEO yang berlaku untuk repo ini:

1. **Event berversi dan idempoten.** Setiap event membawa amplop KONTEKS §3 dan kunci idempoten; duplikat diabaikan.
2. **Outbox sebagai sumber.** Event pesanan ditulis ke outbox Postgres `titipo`; forwarder mengirim FIFO satu kali dengan retry lalu DLQ.
3. **Titeny read-only.** Pipeline tidak menulis balik ke DB titipo dan Titeny hanya menerima lewat endpoint intake.
4. **pedaree.stok.v1 di luar registry.** Kontrak bilateral dikonsumsi `titipo-backend-service`; Titeny melihat efeknya lewat `transaksi.dibuat`.

Keputusan lintas repo: satu DB logis per service, JWT beraudien divisi, kunci idempoten ULID klien pada semua POST, uang integer rupiah, waktu timestamptz UTC.

## Konsekuensi

Perubahan skema event butuh bump versi + ADR baru dan koordinasi dengan pemilik Titeny. Backlog event menumpuk di outbox saat forwarder macet tanpa memengaruhi pesanan.
ADR ini immutable; perubahan keputusan ditulis sebagai ADR baru yang menggantikan.
