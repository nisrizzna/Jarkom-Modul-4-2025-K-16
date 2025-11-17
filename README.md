# Jarkom-Modul-4-2025-K-16

Laporan Praktikum Modul 4 — Jaringan Komputer

## Anggota
- Muhammad Ardiansyah Tri Wibowo — 5027241091
- Nisrina Bilqis — 5027241054

## Akses Soal
[Link Soal Praktikum (Google Docs)](https://docs.google.com/document/d/1xewN7cx4zs8Ftzffe5wRnnWqxyhTv8jl42mrlUh_R_I/edit?tab=t.0)

---

## 1. Topologi Jaringan
Topologi yang digunakan untuk implementasi pada Cisco Packet Tracer (CPT) dan GNS3.

- Topologi CPT: <img width="1900" height="741" alt="image" src="https://github.com/user-attachments/assets/fefa1954-4d00-437d-8c1f-e6ad1b625565" />

- Topologi GNS3: <img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/8dee3f33-4ad3-4ad0-98ce-ca05b80ce6c6" />

---

## 2. Analisis Kebutuhan IP (Subnetting)
Analisis kebutuhan IP untuk setiap subnet berdasarkan rute yang didefinisikan pada soal. Data diurutkan dari host terbesar ke terkecil sebagai dasar perhitungan VLSM.

Tabel singkat kebutuhan IP (contoh):

| Nama Subnet | Rute (contoh) | Jumlah Host | Prefix yang Dibutuhkan | Blok/Ukuran |
|-------------|---------------|-------------:|-----------------------:|------------:|
| A12 | Amonsul → Eregion → Numenor → Arthedain (246), Mirdain (628) | 875 | /22 | 1024 |
| A19 | Amonsul → Eregion → Harad → Harondor (501), Umbar (412) | 914 | /22 | 1024 |
| A16 | Amonsul → Eregion → Harad → Minas Ithil (211), Osgiliath (298) | 510 | /23 | 512 |
| A1  | Amonsul → Fornost → Valnor → Shadow (99), Anarion (67), Lindon (132) | 299 | /23 | 512 |
| ... | ... | ... | ... | ... |

Data lengkap: [Modul 4 Jarkom - Rute.csv](https://docs.google.com/spreadsheets/d/16MMzYwmkU_ju2e1NikboRh4NICEWcja5ldpF_8W5x64/edit?usp=drivesdk)

---

## 3. Perhitungan VLSM (Implementasi CPT)
Prefix IP awal: `192.219.0.0`. Tabel alokasi VLSM (contoh):

| Subnet | Network ID | Netmask | Broadcast | Gateway | Range IP | Prefix |
|--------|------------:|--------:|----------:|--------:|---------:|-------:|
| A12 | 192.219.0.0 | 255.255.252.0 | 192.219.3.255 | 192.219.0.1 | 192.219.0.1 – 192.219.3.254 | /22 |
| A19 | 192.219.4.0 | 255.255.252.0 | 192.219.7.255 | 192.219.4.1 | 192.219.4.1 – 192.219.7.254 | /22 |
| A16 | 192.219.8.0 | 255.255.254.0 | 192.219.9.255 | 192.219.8.1 | 192.219.8.1 – 192.219.9.254 | /23 |
| A1  | 192.219.10.0 | 255.255.254.0 | 192.219.11.255 | 192.219.10.1 | 192.219.10.1 – 192.219.11.254 | /23 |
| A7  | 192.219.14.0 | 255.255.255.128 | 192.219.14.127 | 192.219.14.1 | 192.219.14.1 – 192.219.14.126 | /25 |
| A10 | 192.219.14.128 | 255.255.255.128 | 192.219.14.255 | 192.219.14.129 | 192.219.14.129 – 192.219.14.254 | /25 |
| A23 | 192.219.15.0 | 255.255.255.192 | 192.219.15.63 | 192.219.15.1 | 192.219.15.1 – 192.219.15.62 | /26 |
| A4  | 192.219.15.64 | 255.255.255.192 | 192.219.15.127 | 192.219.15.65 | 192.219.15.65 – 192.219.15.126 | /26 |
| ...  | ... | ... | ... | ... | ... | ... |

Data lengkap: [Modul 4 Jarkom - Rute.csv](https://docs.google.com/spreadsheets/d/16MMzYwmkU_ju2e1NikboRh4NICEWcja5ldpF_8W5x64/edit?usp=drivesdk)

---

## 4. Route Summarization / CIDR (Implementasi GNS3)
Untuk efisiensi tabel routing, digunakan route summarization (supernetting) pada GNS3.

Contoh tabel summarization:

| Subnet Baru | Gabungan dari | Netmask | Hasil Gabungan (Network ID) | Netmask Akhir |
|-------------|---------------|--------:|-----------------------------:|--------------:|
| G-A | A7 (192.219.14.0 /25) + A10 (192.219.14.128 /25) | /25 + /25 | 192.219.14.0 | /24 |
| G-B | A23 (192.219.15.0 /26) + A4 (192.219.15.64 /26) | /26 + /26 | 192.219.15.0 | /25 |
| G-C | A16 (192.219.8.0 /23) + A1 (192.219.10.0 /23) | /23 + /23 | 192.219.8.0 | /22 |
| G-D | A12 (192.219.0.0 /22) + A19 (192.219.4.0 /22) | /22 + /22 | 192.219.0.0 | /21 |
| ... | ... | ... | ... | ... |

Data lengkap: [Modul 4 Jarkom - Rute.csv](https://docs.google.com/spreadsheets/d/16MMzYwmkU_ju2e1NikboRh4NICEWcja5ldpF_8W5x64/edit?usp=drivesdk)

---

## 5. Bukti Implementasi & Verifikasi (CPT — VLSM)

### 5.1 Konfigurasi IP Address (CPT)
Contoh konfigurasi IP pada interface router:

- Router Amonsul: `show ip interface brief`  
  <img width="561" height="101" alt="image" src="https://github.com/user-attachments/assets/ca5f3e9c-bdd3-4865-8aa6-0b344d166529" />

- Router Eregion: `show ip interface brief`  
  <img width="571" height="106" alt="image" src="https://github.com/user-attachments/assets/e5bdda6f-ad74-4c5f-b3c7-9e158cba6816" />


### 5.2 Konfigurasi Static Routing (CPT)
Contoh static routing (non-summarized). Tampilkan routing table tiap router:

- Router Amonsul: `show ip route`  
  <img width="589" height="416" alt="image" src="https://github.com/user-attachments/assets/11363fdd-c2ab-4d99-8ae8-6f7440dfa46b" />


- Router Eregion: `show ip route`  
  <img width="565" height="332" alt="image" src="https://github.com/user-attachments/assets/bc73357e-7382-4656-a126-a1c82bf14cdf" />


### 5.3 Testing Koneksi (CPT)
Contoh pengujian ping antar-subnet:

- Ping dari PC (A1) → PC (A19)  
  <img width="611" height="37" alt="image" src="https://github.com/user-attachments/assets/cd579258-1533-4c13-b19f-ad210068f6d0" />


- Ping dari PC (A10) → PC (A2)  
  <img width="593" height="33" alt="image" src="https://github.com/user-attachments/assets/15fd5b38-e111-4dbd-8109-f6c5e8c84490" />


---
