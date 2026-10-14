# 60-BLAST-RADIUS — titipo-data-pipeline

> Versi: v1.0.0 | Status: disetujui | Menggantikan: -
> Acuan: [TitipO-TownHall platform/versions/v1.0.0](https://github.com/Coding-Skuy/TitipO-TownHall/tree/main/platform/versions/v1.0.0) (BRD/PRD/FRD/FSD/SRS beku 2026-10-07).

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

## Boleh disentuh

Outbox pesanan, forwarder, validasi skema event, CSV rekonsiliasi Senin, agregat metrik, arsip 2 tahun.

## Dilarang

Tulis balik ke DB titipo, akses DB Lumbung, event di luar registry tanpa bump versi + 30 hari dual-read, meneruskan `pedaree.stok.v1` ke Titeny, PII mentah.

## Kontrak yang dikonsumsi

Outbox DB `titipo` (7 event pesanan).

## Kontrak yang diproduksi

`transaksi.dibuat` (flag grosir di atas 50 kg), `vendor.status_berubah`, `logistik.biaya_diperbarui` ke Titeny (`POST /api/v1/event/masuk`, `X-Idempotency-Key`); data latih anonim ke `titipo-ai-models`.

Sumber batas: [townhall-roadmap.md §2](https://github.com/Coding-Skuy/ChefGenie-TownHall/blob/main/townhall-roadmap.md) baris TitipO (pesan <=20.00, serah 06.00-08.00; sinyal `pedaree.stok.v1` anjuran saja; fee vendor 12%) dan [KONTEKS-LINTAS-DIVISI.md](https://github.com/Coding-Skuy/ChefGenie-TownHall/blob/main/KONTEKS-LINTAS-DIVISI.md) §3, §8, §9.
