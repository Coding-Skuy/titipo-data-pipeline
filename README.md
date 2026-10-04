# TitipO Data Pipeline — Divisi TitipO (Community Commerce)

> Aliran event pesanan TitipO → Titeny (analitik).
> TownHall: [TitipO-TownHall](https://github.com/Coding-Skuy/TitipO-TownHall).
> Kontrak auth pola Lumbung: [Lumbung-TownHall](https://github.com/Coding-Skuy/Lumbung-TownHall) — JWT `aud=titipo`, service key `titipo→lumbung`.

## Alur

```text
titipo-backend-service (DB titipo: pesanan, langganan)
  → outbox event pesanan dibuat/diantar/selesai
  → pipelines/order_to_titeny
  → Titeny (analitik pola titip)
```

## Event

| Nama | Kapan | Kunci |
|------|-------|-------|
| `titipo.pesanan_dibuat` | pesanan dibuat | id pesanan, id vendor, jadwal |
| `titipo.pesanan_selesai` | pesanan selesai | id pesanan, durasi |
| `titipo.langganan_diubah` | langganan aktif/nonaktif | id rumah tangga, pola |

Tanpa PII mentah — hanya id + agregat.

## Struktur

```text
pipelines/README.md          # daftar pipeline + jadwal
pipelines/order_to_titeny.md # kontrak event → Titeny
```

## Tautan

- TownHall: https://github.com/Coding-Skuy/TitipO-TownHall
- Backend DB: https://github.com/Coding-Skuy/titipo-backend-service
- AI konsumen: https://github.com/Coding-Skuy/titipo-ai-models
