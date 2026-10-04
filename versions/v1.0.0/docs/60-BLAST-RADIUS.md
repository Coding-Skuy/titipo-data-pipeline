# 60-BLAST-RADIUS — titipo-data-pipeline

## Konsumen

- Konsumen langsung: Titeny intake (11 event) dan `titipo-ai-models` (latih anonim).
- Hulu: `titipo-backend-service` + DB titipo (outbox pesanan selesai).
- Hilir: tidak ada tulis balik ke pesanan atau ledger.

## Failure

- Forwarder macet: event menumpuk di outbox; pesanan dan fee tetap jalan; keterlambatan analitik maksimal 24 jam.
- Skema event berubah tanpa koordinasi: Titeny menolak 422; tahan rilis dan kembalikan ke skema v1.
- Duplikat kirim: kunci ULID sama ditolak tujuan; tandai terkirim tanpa kirim ulang.
- Kunci service key kedaluwarsa: forwarder server-ke-server 403; rotasi 90 hari dengan 2 kunci 7 hari.

## Rollback

- Rollback berarti hentikan forwarder baru dan jalankan ulang versi lama dari offset terakhir tersimpan.
- Tidak ada migrasi skema merusak; versi event v1 tetap didukung selama STUB_UNTIL.
- Offset tidak dimundurkan melewati event yang sudah dipakai latih tanpa catatan audit.

## Batasan

- Dampak maksimal keterlambatan analitik; tidak menyentuh cutoff 20.00, serah 06.00–08.00, fee, atau auth pengguna.
- Insiden skema dieskalasi ke pemilik Titeny dan TownHall, bukan patch payload diam-diam.
