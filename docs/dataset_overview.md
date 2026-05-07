# DATASET TIME SERIES - THONG TIN CHI TIET

## 1) Snapshot du lieu hien tai
- Snapshot common model-ready duoc tong hop tu 3 file tren Kaggle:
  - `TRAIN_PATH = '/kaggle/input/datasets/traanfddinhfkhair/datasetttt/data/train.csv'`
  - `VAL_PATH = '/kaggle/input/datasets/traanfddinhfkhair/datasetttt/data/val.csv'`
  - `TEST_PATH = '/kaggle/input/datasets/traanfddinhfkhair/datasetttt/data/test.csv'`
- Notebook hien tai dung path Kaggle co dinh, khong con fallback local.
- Kich thuoc snapshot hien tai: `103,344 x 10`
- Time column: `TIME`
- Target cot goc: `P_224`
- Time range: `2020-02-09 00:00:00` -> `2025-12-31 23:30:00`
- Frequency: `30 phut`
- Expected rows theo timeline: `103,344`
- Actual rows: `103,344`
- Missing timestamps: `0`
- Duplicate timestamp: `0`
- Missing values toan bo schema: `0`
- Tong so cot: `10`; trong do `1` cot thoi gian va `9` cot so
- Khong con schema drift giua train/val/test: ca ba split deu dung cung `10` cot

## 2) Cau truc bien
- Target cot goc: `P_224`
- Peer load features: `P_474 TÃ¢n HÆ°ng`, `P_476 TÃ¢n HÆ°ng`, `P_478 TÃ¢n HÆ°ng`, `P_480 TÃ¢n HÆ°ng`
- Weather features: `temp`, `rhum`, `prcp`, `wspd`
- Tong so cot so khong tinh `TIME`: `9`
- So input features neu khong tinh target: `8`
- `split_metadata.json` hien dang khop voi snapshot nay: tong `103,344` dong, source `TanHung_P224_6Years_With_Weather.csv`, split mode `8:1:1`
- Time-derived features co the tao them: `hour`, `day_of_week`, `month`, `is_weekend`, `sin/cos(hour)`, `sin/cos(day_of_week)`, `sin/cos(month)`

### Muc tieu du bao trong `script-base-24h-b (1).ipynb`
- Theo code dang chay trong notebook, bai toan du bao la `24h -> 24h`, khong phai `12h -> 12h`.
- `X` la cua so vao gom `48` moc lien tiep cua tat ca feature da xu ly, ung voi `24h` du lieu lich su o tan suat `30` phut.
- `y` la vector muc tieu gom `24` gia tri `P_224` trong `24h` tiep theo, lay o tan suat `1 gio`.
- Cach tao `y` trong ham `create_sequences_shift_based(...)`: bat dau ngay sau cua so input ket thuc va lay moi `2` buoc `30` phut mot lan.
- Cac moc dau ra tuong ung: `t+1h, t+2h, ..., t+24h`.
- Notebook danh gia rieng tung dau ra theo `24` moc: `t+1h` den `t+24h`.
- Muc tieu supervised thuc te vi vay la du bao da buoc `P_224` trong `24h` tuong lai, khong chi 1 diem don.
- Weather duoc shift thanh `temp_forecast`, `rhum_forecast`, `prcp_forecast`, `wspd_forecast` bang `shift(-48)` de mo phong thong tin du bao thoi tiet cua `24h` phia truoc horizon can du doan.
- Lag ngan han dua vao input: `P224_lag2`, `P224_lag4`, `P224_lag6`, `P224_lag12`.
- Lag mua vu / dai hon dua vao input: `P224_lag48`, `P224_lag336`.
- Trend / dong hoc dua vao input: `P224_roll24`, `P224_roll48`, `P224_std24`, `P224_std48`, `P224_delta1`, `P224_delta48`.
- So sequence tao ra sau windowing trong notebook: `train = 82,580`, `val = 10,239`, `test = 10,240`.

## 3) Split hien tai trong `data`
- Split mode thuc te: `temporal`
- Ratio thuc te hien tai: `train/val/test = 79.9998% / 9.9996% / 10.0006%`
- Cac split lien tuc theo thoi gian; khoang cach giua hai split ke nhau dung `30 phut`
| Split | Start | End | Rows | Ratio | Target mean | Target std | Zero ratio |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `train` | `2020-02-09 00:00:00` | `2024-10-27 09:00:00` | 82,675 | 79.9998% | 23.8334 | 9.0629 | 0.4318% |
| `val` | `2024-10-27 09:30:00` | `2025-05-30 16:00:00` | 10,334 | 9.9996% | 24.2231 | 11.8151 | 0.3097% |
| `test` | `2025-05-30 16:30:00` | `2025-12-31 23:30:00` | 10,335 | 10.0006% | 20.2354 | 8.5144 | 0.1838% |

- Drift mean target so voi train:
  - `val`: `+1.63%`
  - `test`: `-15.10%`

## 4) Kiem tra timeline va chat luong du lieu
- Median step: `0 days 00:30:00`
- Mode step: `0 days 00:30:00`
- Time overlap giua train/val: `0`
- Time overlap giua val/test: `0`
- Toan bo schema khong co gia tri missing va khong co cot diagnostic phat sinh rieng o test
| Column | Missing | Negative | Zero ratio |
| --- | --- | --- | --- |
| `TIME` | 0 | `n/a` | `n/a` |
| `P_474 TÃ¢n HÆ°ng` | 0 | 0 | 9.7016% |
| `P_476 TÃ¢n HÆ°ng` | 0 | 0 | 40.8268% |
| `P_478 TÃ¢n HÆ°ng` | 0 | 0 | 9.0426% |
| `P_480 TÃ¢n HÆ°ng` | 0 | 0 | 2.2275% |
| `P_224` | 0 | 0 | 0.3948% |
| `temp` | 0 | 0 | 0.0000% |
| `rhum` | 0 | 0 | 0.0000% |
| `prcp` | 0 | 0 | 70.2934% |
| `wspd` | 0 | 0 | 0.0619% |

## 5) Thong ke chi tiet cua tat ca bien so
| Variable | Mean | Std | Min | P01 | P05 | Q1 | Median | Q3 | P95 | P99 | Max | Zero ratio |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `P_224` | 23.5126 | 9.3874 | 0.0000 | 2.5279 | 7.3845 | 16.3916 | 24.7802 | 31.0449 | 36.7529 | 41.1610 | 60.0034 | 0.3948% |
| `P_474 TÃ¢n HÆ°ng` | 6.5668 | 3.7588 | 0.0000 | 0.0000 | 0.0000 | 3.7768 | 7.0068 | 9.7632 | 11.7930 | 13.3412 | 19.2432 | 9.7016% |
| `P_476 TÃ¢n HÆ°ng` | 1.5722 | 1.8693 | 0.0000 | 0.0000 | 0.0000 | 0.0000 | 0.6303 | 3.0493 | 4.0747 | 7.9154 | 16.7383 | 40.8268% |
| `P_478 TÃ¢n HÆ°ng` | 7.3649 | 4.4304 | 0.0000 | 0.0000 | 0.0000 | 3.3960 | 7.9956 | 10.9351 | 13.8623 | 15.9717 | 20.0932 | 9.0426% |
| `P_480 TÃ¢n HÆ°ng` | 8.0086 | 2.8115 | 0.0000 | 0.0000 | 1.4404 | 6.6748 | 8.7793 | 10.1294 | 10.9643 | 11.4844 | 18.3996 | 2.2275% |
| `temp` | 27.3212 | 3.0910 | 17.8000 | 21.1000 | 23.2000 | 25.0000 | 26.7000 | 29.5000 | 33.0000 | 35.0000 | 38.7000 | 0.0000% |
| `rhum` | 78.0810 | 16.5108 | 25.0000 | 37.0000 | 46.0000 | 67.0000 | 81.0000 | 93.0000 | 98.0000 | 99.0000 | 100.0000 | 0.0000% |
| `prcp` | 0.3043 | 1.1534 | 0.0000 | 0.0000 | 0.0000 | 0.0000 | 0.0000 | 0.1000 | 1.7000 | 6.0000 | 24.7000 | 70.2934% |
| `wspd` | 7.7696 | 4.0521 | 0.0000 | 1.0000 | 2.3000 | 4.8000 | 7.1000 | 10.1000 | 15.5000 | 19.5000 | 30.9000 | 0.0619% |

## 6) Thong ke nang cao cua target `P_224`
- IQR: `14.6533`
- Coefficient of variation: `0.3993`
- Skewness: `-0.3237`
- Kurtosis: `-0.6967`
- Zero count / ratio: `408` / `0.3948%`
- IQR outlier threshold: `[-5.5884, 53.0249]`
- IQR outlier count / ratio: `4` / `0.0039%`

## 7) Dong hoc chuoi cua target `P_224`
- Mean absolute change moi 30 phut: `1.4962`
- Median absolute change moi 30 phut: `0.8984`
- Std of delta: `2.2379`
- Max increase 1 step: `16.1151`
- Max decrease 1 step: `-31.0589`
- Mean absolute pct change: `9.9203%`
- Rolling mean 48-step min/max: `4.4492` / `39.3114`
- Rolling mean 336-step min/max: `5.1605` / `35.0903`
- Autocorr lag 1 (30 phut): `0.9716`
- Autocorr lag 48 (1 ngay): `0.8836`
- Autocorr lag 336 (1 tuan): `0.7609`

## 8) Seasonality chi tiet cua target `P_224`
### Theo gio trong ngay
| Hour | Mean | Std | Min | Max |
| --- | --- | --- | --- | --- |
| 00 | 28.9565 | 5.9693 | 5.3393 | 46.5429 |
| 01 | 28.4610 | 5.9818 | 3.1274 | 48.3920 |
| 02 | 28.1133 | 6.1003 | 4.8901 | 60.0034 |
| 03 | 27.8642 | 6.1259 | 4.8315 | 46.6259 |
| 04 | 27.6538 | 6.1510 | 0.0000 | 47.0472 |
| 05 | 27.3264 | 6.2567 | 0.0000 | 58.1026 |
| 06 | 26.3790 | 6.5313 | 0.0000 | 47.6416 |
| 07 | 21.6131 | 6.5206 | 0.0000 | 43.8997 |
| 08 | 16.5951 | 6.4383 | 0.0000 | 40.1245 |
| 09 | 13.7559 | 6.6267 | 0.0000 | 42.3046 |
| 10 | 12.2460 | 6.6744 | 0.0000 | 41.4379 |
| 11 | 11.6297 | 6.5409 | 0.0000 | 39.6679 |
| 12 | 12.2100 | 6.5632 | 0.0000 | 42.5146 |
| 13 | 13.5068 | 6.6990 | 0.0000 | 42.6269 |
| 14 | 15.7836 | 6.7371 | 0.0000 | 39.9731 |
| 15 | 19.1937 | 6.7113 | 0.0000 | 41.7724 |
| 16 | 23.6553 | 6.5308 | 0.0000 | 43.5571 |
| 17 | 27.5836 | 6.4630 | 0.0000 | 48.6169 |
| 18 | 30.0114 | 6.0706 | 2.2656 | 50.5785 |
| 19 | 30.0802 | 6.2384 | 0.0000 | 49.2334 |
| 20 | 30.5093 | 6.1570 | 5.4000 | 50.6249 |
| 21 | 31.0171 | 6.1726 | 5.7345 | 50.1152 |
| 22 | 30.4323 | 6.0129 | 7.1069 | 55.7460 |
| 23 | 29.7243 | 5.8767 | 6.7444 | 47.3406 |

### Theo thu trong tuan
| Day | Mean | Std | Min | Max |
| --- | --- | --- | --- | --- |
| `Mon` | 23.0167 | 9.3474 | 0.0000 | 47.6489 |
| `Tue` | 23.6378 | 9.3616 | 0.0000 | 47.5269 |
| `Wed` | 23.6842 | 9.3479 | 0.0000 | 50.6249 |
| `Thu` | 23.8155 | 9.3532 | 0.0000 | 60.0034 |
| `Fri` | 23.5671 | 9.3985 | 0.0000 | 46.7016 |
| `Sat` | 23.2115 | 9.4508 | 0.0000 | 55.7460 |
| `Sun` | 23.6554 | 9.4275 | 0.0000 | 58.1026 |

### Theo thang trong nam
| Month | Mean | Std | Min | Max |
| --- | --- | --- | --- | --- |
| 1 | 25.6598 | 10.6728 | 0.0000 | 44.7314 |
| 2 | 25.3450 | 11.3065 | 0.0000 | 58.1026 |
| 3 | 26.2660 | 9.9478 | 0.6703 | 47.0751 |
| 4 | 20.6951 | 9.0647 | 0.0000 | 47.5269 |
| 5 | 19.7745 | 7.6636 | 0.0000 | 39.5654 |
| 6 | 20.8300 | 7.9642 | 0.5005 | 42.0507 |
| 7 | 23.6392 | 8.4956 | 1.7432 | 55.7460 |
| 8 | 24.9716 | 9.1088 | 0.0000 | 43.3300 |
| 9 | 22.2716 | 8.7096 | 0.0000 | 42.5854 |
| 10 | 22.6208 | 8.4074 | 0.0000 | 60.0034 |
| 11 | 24.4663 | 9.2872 | 0.0000 | 50.6249 |
| 12 | 26.0185 | 8.8336 | 0.0000 | 48.6169 |

## 9) Tuong quan cua `P_224` voi cac bien dau vao
| Variable | Pearson corr voi `P_224` |
| --- | --- |
| `P_474 TÃ¢n HÆ°ng` | 0.8685 |
| `P_478 TÃ¢n HÆ°ng` | 0.8390 |
| `P_480 TÃ¢n HÆ°ng` | 0.5411 |
| `P_476 TÃ¢n HÆ°ng` | 0.4732 |
| `rhum` | 0.2299 |
| `prcp` | -0.0222 |
| `wspd` | -0.0310 |
| `temp` | -0.4092 |

## 10) Thong ke theo split cho peer load features
| Split | Variable | Mean | Std | Min | Max |
| --- | --- | --- | --- | --- | --- |
| `train` | `P_474 TÃ¢n HÆ°ng` | 6.4356 | 3.7206 | 0.0000 | 17.6861 |
| `train` | `P_476 TÃ¢n HÆ°ng` | 1.3199 | 1.5559 | 0.0000 | 12.8540 |
| `train` | `P_478 TÃ¢n HÆ°ng` | 7.8749 | 4.2166 | 0.0000 | 20.0932 |
| `train` | `P_480 TÃ¢n HÆ°ng` | 8.2030 | 2.6835 | 0.0000 | 17.2270 |
| `val` | `P_474 TÃ¢n HÆ°ng` | 6.7177 | 4.0532 | 0.0000 | 14.6167 |
| `val` | `P_476 TÃ¢n HÆ°ng` | 2.4265 | 2.5012 | 0.0000 | 16.7383 |
| `val` | `P_478 TÃ¢n HÆ°ng` | 8.3282 | 4.6527 | 0.0000 | 17.4902 |
| `val` | `P_480 TÃ¢n HÆ°ng` | 6.7507 | 3.3117 | 0.0000 | 11.3647 |
| `test` | `P_474 TÃ¢n HÆ°ng` | 7.4655 | 3.6264 | 0.0000 | 19.2432 |
| `test` | `P_476 TÃ¢n HÆ°ng` | 2.7366 | 2.5939 | 0.0000 | 15.0976 |
| `test` | `P_478 TÃ¢n HÆ°ng` | 2.3214 | 1.9886 | 0.0000 | 17.4268 |
| `test` | `P_480 TÃ¢n HÆ°ng` | 7.7119 | 2.9157 | 0.0000 | 18.3996 |

## 11) Thong ke theo split cho weather features
| Split | Variable | Mean | Std | Min | Max |
| --- | --- | --- | --- | --- | --- |
| `train` | `temp` | 27.4265 | 3.1221 | 17.9000 | 38.7000 |
| `train` | `rhum` | 77.9335 | 16.8461 | 25.0000 | 100.0000 |
| `train` | `prcp` | 0.3104 | 1.1628 | 0.0000 | 22.3000 |
| `train` | `wspd` | 7.5999 | 3.8826 | 0.0000 | 29.5000 |
| `val` | `temp` | 27.1640 | 3.3301 | 17.8000 | 36.7000 |
| `val` | `rhum` | 73.4457 | 15.5912 | 26.0000 | 100.0000 |
| `val` | `prcp` | 0.1325 | 0.7086 | 0.0000 | 15.7000 |
| `val` | `wspd` | 8.3569 | 4.3765 | 0.0000 | 25.3000 |
| `test` | `temp` | 26.6367 | 2.4272 | 19.1000 | 33.0000 |
| `test` | `rhum` | 83.8955 | 12.5395 | 35.0000 | 100.0000 |
| `test` | `prcp` | 0.4275 | 1.3932 | 0.0000 | 24.7000 |
| `test` | `wspd` | 8.5403 | 4.8248 | 0.0000 | 30.9000 |

## 12) Xu huong theo nam cua target `P_224`
| Year | Rows | Mean | Std | Min | Max | Zero ratio |
| --- | --- | --- | --- | --- | --- | --- |
| 2020 | 15,696 | 24.6374 | 5.9331 | 0.0000 | 48.4155 | 0.0828% |
| 2021 | 17,520 | 23.0605 | 8.7576 | 0.0000 | 42.6123 | 0.4795% |
| 2022 | 17,520 | 23.4312 | 8.6437 | 0.0000 | 39.4115 | 0.3539% |
| 2023 | 17,520 | 23.1613 | 9.5511 | 0.0000 | 58.1026 | 0.8390% |
| 2024 | 17,568 | 25.3506 | 11.5188 | 0.0000 | 60.0034 | 0.3017% |
| 2025 | 17,520 | 21.5464 | 10.1928 | 0.0000 | 55.7460 | 0.2797% |

## 13) Tin hieu ngay le va dip dac biet
- Doi chieu theo lich nghi le Viet Nam giai doan `2020-2025` cho thay snapshot hien tai chua `76` ngay holiday.
- Trung binh `P_224` tren holiday = `13.5069`, trong khi ngay khong holiday = `23.8787`; thap hon xap xi `43.43%`.
- Neu so voi baseline local theo rolling median `35` ngay, `daily_mean` tren holiday giam trung binh `-43.18%`; `daily_std` cung giam trung binh `-18.97%`. Nghia la holiday khong chi keo muc tai xuong ma con lam profile trong ngay "phang" hon.
- Cum `Tet Nguyen Dan` la holiday effect manh nhat trong bo du lieu; day la nguon dip load lon nhat va lap lai ro rang qua nhieu nam.

| Holiday cluster | Days | Mean daily mean | Mean anomaly vs local baseline |
| --- | --- | --- | --- |
| `29 Tet` | 4 | `10.2710` | `-62.77%` |
| `Giao thua` | 5 | `7.8390` | `-71.39%` |
| `Mung 1 Tet` | 5 | `6.7950` | `-75.43%` |
| `Mung 2 Tet` | 5 | `6.7740` | `-75.38%` |
| `Mung 3 Tet` | 5 | `6.9520` | `-74.72%` |
| `Mung 4 Tet` | 5 | `8.5640` | `-68.71%` |
| `Mung 5 Tet` | 3 | `10.0160` | `-64.20%` |
| `30/4` | 6 | `13.3110` | `-32.18%` |
| `1/5` | 6 | `12.4990` | `-35.91%` |
| `2/9` | 11 | `17.4460` | `-28.82%` |
| `Tet duong lich` | 5 | `19.8250` | `-26.56%` |
| `Gio To Hung Vuong` | 6 | `17.4280` | `-23.34%` |

- Diem holiday cuc manh nhat trong toan bo snapshot la `2023-01-22` (`Mung 1 Tet`): `daily_mean = 4.5920`, lech `-83.06%` so voi baseline local.
- Tinh hieu Tet khong chi nam trong ngay nghi chinh thuc; cac ngay sat le va ngay ke le cung con dip keo dai, vi du `2023-01-27` van con `-66.43%` du khong con la holiday label.
- Khong phai moi ngay duoc danh dau "day off" deu giam tai. Truong hop `Day off (substituted from 05/04/2024)` co `daily_mean anomaly = +25.60%`.

## 14) Ngay bat thuong va dao dong bat thuong
### Muc tai bat thuong ngoai le
| Date | Daily mean | Anomaly vs local baseline | Ghi chu |
| --- | --- | --- | --- |
| `2023-01-27` | `9.0640` | `-66.43%` | Non-holiday, nhung van nam trong duoi dip sau Tet |
| `2021-10-17` | `9.9260` | `-59.28%` | Non-holiday, co zero-run lien tuc `12.5h` |
| `2023-04-16` | `8.2250` | `-54.86%` | Non-holiday, co zero-run lien tuc `11.5h` |
| `2024-08-04` | `13.2570` | `-50.46%` | Non-holiday, co zero-run lien tuc `11.5h` |
| `2025-12-03` | `28.2940` | `+41.66%` | Cum surge manh o cuoi test |
| `2024-04-26` | `28.4540` | `+38.29%` | Bat dau cum tang cao `26-28/04/2024` |
| `2025-09-04` | `27.3900` | `+36.62%` | Tang cao bat thuong ngay sau Quoc khanh |
| `2025-06-23` | `24.6520` | `+35.65%` | Surge ngoai holiday |

- Muc tai rolling `28` ngay cao nhat dat quanh `2020-03-02` (`32.7330`) va thap nhat quanh `2023-05-10` (`16.5668`). Nghia la bo du lieu co regime shift dai hanh trinh, khong chi dao dong ngan han theo le.
- Nhung pha giam theo thang lon nhat xuat hien o `2025-04` (`-31.25%` so voi thang truoc), `2023-03` (`-26.19%`), `2020-04` (`-25.23%`), `2021-02` (`-25.15%`), `2022-09` (`-25.06%`). Mot phan trong so nay gan holiday cluster, nhung khong phai tat ca.

### Dao dong noi ngay va step change bat thuong
- Cac ngay co `daily_std` bat thuong cao nhat deu khong trung holiday:
  - `2020-10-10`: `daily_std anomaly = +323.17%`
  - `2020-05-23`: `+232.68%`
  - `2020-10-17`: `+167.70%`
  - `2025-08-09`: `+74.41%`
- Cac one-step move lon nhat (`30 phut`) hau het cung khong gan holiday:
  - `2023-11-26 17:30`: `-31.0589`
  - `2024-10-24 02:30`: `-24.8420`
  - `2025-07-19 23:00`: `-24.1690`
  - lon nhat chieu tang: `2024-07-17 14:30`: `+16.1151`
- Cac zero-run dai nhat ngoai holiday:
  - `2021-10-17 04:30 -> 16:30`: `12.5h`
  - `2023-04-16 06:30 -> 17:30`: `11.5h`
  - `2024-08-04 05:30 -> 16:30`: `11.5h`
  - `2025-08-09 07:30 -> 16:30`: `9.5h`
- Ngoai holiday, nhung zero-run dai va jump mot buoc rat lon nhu tren co mau hinh khac voi seasonality thong thuong va giong nhom su kien van hanh / outage / curtailment / sensor glitch hon.

## 15) Nhan xet chi tiet
- Snapshot trong `data` hien tai chay lien tuc tu `2020-02-09 00:00:00` den `2025-12-31 23:30:00`, khong thieu moc `30` phut va khong trung timestamp.
- Ba split da tro ve cung mot schema on dinh `10` cot; khong con cac cot diagnostic chi xuat hien o test.
- `split_metadata.json` hien phu hop voi snapshot CSV hien tai; mo ta cu ve bo `2026`/`107,664` dong khong con dung nua.
- `P_224` van co seasonality ngay rat ro: mean cao nhat o `21h` (31.0171) va thap nhat o `11h` (11.6297).
- Hieu ung theo thu trong tuan yeu: mean weekend thap hon weekday khoang `0.47%`.
- Seasonality theo thang ro: mean cao nhat o thang `3` (26.2660) va thap nhat o thang `5` (19.7745).
- Target co do nho thoi gian manh: autocorrelation lag `1` = `0.9716`, lag `48` = `0.8836`, lag `336` = `0.7609`.
- Phan phoi target lech trai nhe (`skewness = -0.3237`) va outlier theo IQR van rat it (`4` diem, `0.0039%`).
- Drift muc tai giua cac split da thay doi ro: `val` cao hon `train` `1.63%`, `test` thap hon `train` `15.10%`.
- Nguon thong tin ngoai sinh manh nhat hien tai la `P_474 TÃ¢n HÆ°ng` (0.8685) va `P_478 TÃ¢n HÆ°ng` (0.8390).
- Dataset hien tai khong co gia tri am o bat ky cot nao; neu pipeline co buoc xu ly negatives thi hien tai khong can kich hoat cho snapshot nay.
- `prcp` van la bien sparse nhat voi `70.29%` gia tri bang `0`; cac bien weather con lai day du va on dinh, trong do `temp` co tuong quan am muc vua (-0.4092) voi target.
- Holiday effect la tin hieu bat thuong ngoai sinh ro nhat: Tet co the keo `daily_mean` xuong muc `-75%` den `-83%` so voi baseline local.
- Ngoai holiday, van ton tai mot nhom ngay co zero-run dai, day range/day std rat cao, va one-step jump rat lon.

## 16) Tong hop Trend va Seasonality
### Trend theo nam
- Nhu cau dien khong tang deu theo nam.
- Neu bo qua `2020` vi day la nam partial, mean nam cua `P_224` di theo mau:
  - `2021 = 23.0605`
  - `2022 = 23.4312` (`+1.61%`)
  - `2023 = 23.1613` (`-1.15%`)
  - `2024 = 25.3506` (`+9.45%`)
  - `2025 = 21.5464` (`-15.01%`)
- Nam `2024` la dinh cao nhat trong cac nam full-year, sau do `2025` giam manh. Vi vay trend dai han khong phai tang dan on dinh ma mang tinh regime shift.

### Mua vu nam
- Mua vu nam ro, nhung khong theo kieu "mua he cao hon mua dong".
- Trung binh theo thang cho thay:
  - cao nhat: `thang 3 = 26.2660`
  - tiep theo: `thang 12 = 26.0185`, `thang 1 = 25.6598`, `thang 2 = 25.3450`
  - thap nhat: `thang 5 = 19.7745`
- Neu gom theo cum thang:
  - `Nov-Mar = 25.5604`
  - `Apr-Sep = 22.0429`
- Nghia la nhom `Apr-Sep` thap hon `Nov-Mar` khoang `13.76%`. Duong trung binh thang co dang cao o dau/cuoi nam va tao day ro o `Apr-Jun`.

### Mua vu tuan
- Mua vu tuan co, nhung yeu hon mua vu ngay va nam.
- Mean theo thu:
  - `Mon = 23.0167` la thap nhat
  - `Thu = 23.8155` la cao nhat
  - chenhlech `Thu` so voi `Mon` = `+3.47%`
- Trung binh:
  - `Mon-Fri = 23.5441`
  - `Sat-Sun = 23.4338`
- Weekend thap hon weekday khoang `0.47%`, nen hieu ung tuan ton tai nhung khong manh. `Sun` (`23.6554`) thuc te khong thap hon nhieu ngay trong tuan.

### Mua vu ngay
- Mua vu ngay rat ro, nhung khong phai hinh chu `M` dien hinh tren toan bo mau.
- Profile trung binh `30 phut` cho thay:
  - dinh chinh: `21:00 = 31.0855`
  - dinh phu / shoulder: `18:30 = 30.3307`
  - day thap nhat: `11:30 = 11.5835`
- Theo khung gio:
  - `00-05h = 28.0625`
  - `06-09h = 19.5858`
  - `10-14h = 13.0752`
  - `15-17h = 23.4775`
  - `18-22h = 30.4100`
- Nghia la chuoi co mot day trua rat sau, sau do leo manh len mot cum dinh toi rong; overall gan voi dang `1 day giua ngay + 1 plateau buoi toi` hon la `2 dinh sang-toi` tach biet ro.
- Theo quy, dinh van on dinh o `21h` va day van o `11h`:
  - `Q1`: peak `21h = 33.4320`, trough `11h = 12.3435`
  - `Q2`: peak `21h = 28.2372`, trough `11h = 10.2963`
  - `Q3`: peak `21h = 31.2896`, trough `11h = 12.2786`
  - `Q4`: peak `21h = 31.2938`, trough `11h = 11.6494`
- Trong `Q1` va `Q4`, moc `18h` gan nhu cao ngang `21h`, nen co dang "lac da" nhe. Trong `Q2-Q3`, profile nghieng ve mot dinh toi ro hon.

## 17) Do nhay voi thoi tiet
- Snapshot hien tai chi co `4` bien thoi tiet: `temp`, `rhum`, `prcp`, `wspd`.
- Khong co `dew point`, `solar radiation`, `cloud cover`, nen khong the kiem tra truc tiep cac hieu ung do am cam nhan, buc xa mat troi, may che, hay anh huong chieu sang / PV tren snapshot nay.

### Tuong quan cung thoi diem
| Variable | Pearson corr voi `P_224` |
| --- | --- |
| `temp` | `-0.4092` |
| `rhum` | `0.2299` |
| `prcp` | `-0.0222` |
| `wspd` | `-0.0310` |

- `temp` la bien thoi tiet manh nhat trong snapshot hien tai, nhung dau cua moi lien he la `am`, khong phai `duong`.
- Theo split, do manh cua weather effect tang dan ve cuoi timeline:
  - `train`: `temp = -0.4010`, `rhum = 0.2218`
  - `val`: `temp = -0.4771`, `rhum = 0.3311`
  - `test`: `temp = -0.5620`, `rhum = 0.4410`
- Khi tach `10%` diem nhiet do thap nhat va `10%` diem nhiet do cao nhat:
  - `temp <= Q10 (24.0C)`: `load mean = 28.0311`
  - `temp >= Q90 (31.7C)`: `load mean = 18.3444`
  - nhom nhiet do cao co load thap hon nhom nhiet do thap khoang `34.56%`
- Nghia la tren snapshot tong hop nay, quan he mua vu/khong gian thuc te dang nghieng theo huong `nong hon -> load tong the thap hon`, trai voi gia thuyet dieu hoa tang tai dien kieu he thong tong quat.

### Tuong quan tre (lag correlation) cua weather
- Tuong quan tho theo lag `30 phut` den `72h` cho thay:
  - `temp(t)` voi `load(t)`: `-0.4092`
  - `temp(t-24h)`: `-0.4089`
  - `temp(t-48h)`: `-0.4132`
  - `temp(t-72h)`: `-0.4166`
  - `temp(t-6h)`: `+0.4667`
- `rhum` co mau nguoc dau voi `temp`:
  - `rhum(t)`: `0.2299`
  - `rhum(t-24h)`: `0.2223`
  - `rhum(t-48h)`: `0.2182`
  - `rhum(t-72h)`: `0.2144`
  - `rhum(t-6h)`: `-0.4251`
- `prcp` va `wspd` co tuong quan tre yeu:
  - `prcp` manh nhat o `t-6h`: `0.0864`
  - `wspd` manh nhat o `t-6h`: `0.1300`
- Moc `6h` cho `temp/rhum` doi dau rat manh, day la dau hieu chu ky ngay bi lech pha voi load, khong nen doc nhu quan he nhan qua thuan tuy.

### Weather effect sau khi khu seasonality
- Khi tru mean theo `month + hour` de lay anomaly:
  - `temp_anom` voi `load_anom`: `0.0381`
  - `rhum_anom`: `0.0276`
  - `prcp_anom`: `0.0104`
  - `wspd_anom`: `-0.0154`
- Neu xet lag anomaly cho `temp`:
  - `0h`: `0.0381`
  - `24h`: `0.0479`
  - `48h`: `0.0394`
  - `72h`: `0.0339`
- Neu xet rolling mean anomaly cua `temp`:
  - `1 ngay`: `0.1127`
  - `2 ngay`: `0.1073`
  - `3 ngay`: `0.1016`
- Nhung con so nay cho thay sau khi loai chu ky thang-va-gio, weather effect van co nhung khong con "cuc manh"; `temp` van la bien quan trong nhat, va nhiet do trung binh `1-3` ngay gan day co lien he duong nhe voi load anomaly.

### Goc nhin daily weather summary
- Tren muc daily aggregate:
  - `corr(load_mean, temp_mean) = -0.0076`
  - `corr(load_mean, temp_max) = 0.0576`
  - `corr(load_mean, temp_min) = -0.1565`
  - `corr(load_mean, rhum_mean) = -0.1949`
  - `corr(load_mean, prcp_sum) = -0.1257`
  - `corr(load_mean, wspd_mean) = 0.0699`
- Nghia la khi len daily level, moi lien he weather-load yeu hon ro so voi khi nhin truc tiep theo moc `30 phut`, va phan lon signal bi tron voi seasonality / holiday / regime shift.

## 18) Da chu ky va seasonality thay doi theo thoi gian
### Tach seasonality theo nhieu chu ky cung luc
- Bo du lieu co dong thoi `3` chu ky chinh:
  - `chu ky ngay`: dao dong `11h -> 21h`
  - `chu ky tuan`: chenhlech nhe giua weekday va weekend
  - `chu ky nam`: muc tai cao o dau/cuoi nam, thap o `Apr-Jun`
- Khi nhin dong thoi, mau hinh tong quat la:
  - day giua ngay (`10-14h`)
  - dinh toi (`18-22h`, dinh ro nhat quanh `21h`)
  - muc trung binh cua profile nay lai thay doi theo thang/quy/nam

### Bien do chu ky ngay thay doi theo quy
| Quarter | Peak hour | Peak mean | Trough hour | Trough mean | Amplitude |
| --- | --- | --- | --- | --- | --- |
| `Q1` | `21h` | `33.4320` | `11h` | `12.3435` | `21.0885` |
| `Q2` | `21h` | `28.2372` | `11h` | `10.2963` | `17.9409` |
| `Q3` | `21h` | `31.2896` | `11h` | `12.2786` | `19.0111` |
| `Q4` | `21h` | `31.2938` | `11h` | `11.6494` | `19.6444` |

- Bien do ngay-dem manh nhat o `Q1`, yeu nhat o `Q2`.
- Chenhlech amplitude giua `Q1` va `Q2` la `+3.1476`, tuong duong khoang `17.54%`.
- Nghia la seasonality ngay khong co bien do co dinh; no thay doi theo mua / giai doan trong nam.

### Bien do chu ky ngay thay doi theo nam
| Year | Peak hour | Peak mean | Trough hour | Trough mean | Amplitude |
| --- | --- | --- | --- | --- | --- |
| `2020` | `21h` | `27.5122` | `11h` | `21.9491` | `5.5631` |
| `2021` | `21h` | `30.6637` | `11h` | `11.5802` | `19.0834` |
| `2022` | `21h` | `30.9214` | `11h` | `11.4448` | `19.4767` |
| `2023` | `21h` | `31.0065` | `11h` | `10.5691` | `20.4375` |
| `2024` | `21h` | `35.6782` | `11h` | `8.7165` | `26.9617` |
| `2025` | `21h` | `29.9430` | `11h` | `6.6010` | `23.3419` |

- Bo qua `2020` vi day la nam partial, bien do chu ky ngay tang ro tu `2021-2024`, roi giam lai o `2025` nhung van cao hon `2021-2023`.
- `2024` la nam co seasonality ngay-dem manh nhat trong snapshot.
- Dinh va day rat on dinh theo nam: peak gan nhu luon o `21h`, trough gan nhu luon o `11h`.

### Chu ky nam khong co hinh dang co dinh giua cac nam
- Ma tran `year x month` cho thay seasonality nam bien doi theo tung nam, khong chi la mot duong thang lap lai.
- Mot vai diem ro:
  - `thang 4` thap o tat ca cac nam full-year, nhung muc do giam khac nhau:
    - `2021 = 19.0200`
    - `2022 = 22.1040`
    - `2023 = 17.5470`
    - `2024 = 24.5060`
    - `2025 = 19.4360`
  - `thang 3` thuong cao, nhung do manh thay doi rat lon:
    - `2023 = 19.5250`
    - `2024 = 30.6210`
    - `2025 = 28.2680`
- Nghia la chu ky nam ton tai, nhung bi bien dang theo nam do holiday effect, drift, va regime shift.

### Chu ky tuan cung thay doi theo thoi gian
| Quarter | Weekday mean | Weekend mean | Weekend vs weekday |
| --- | --- | --- | --- |
| `Q1` | `25.6691` | `26.0655` | `+1.54%` |
| `Q2` | `20.5812` | `20.0378` | `-2.64%` |
| `Q3` | `23.6384` | `23.6517` | `+0.06%` |
| `Q4` | `24.4460` | `24.1717` | `-1.12%` |

- Weekend effect khong on dinh theo quy; no tham chi doi dau:
  - `Q1`: weekend cao hon weekday
  - `Q2`, `Q4`: weekend thap hon weekday
  - `Q3`: gan nhu khong co weekend effect
- Theo thang, do manh weekend effect thay doi tu `+2.64%` (`thang 3`) den `-4.55%` (`thang 4`).

### Tuong tac giua chu ky ngay va chu ky nam
- Peak hour theo thang khong hoan toan co dinh:
  - `thang 1`: peak `18h`
  - `thang 2`: peak `18h`
  - `thang 3-11`: peak chu yeu `21h`
  - `thang 12`: peak `18h`
- Nhung thang co bien do ngay-dem manh nhat:
  - `thang 1 = 22.8163`
  - `thang 3 = 22.2053`
  - `thang 12 = 21.7339`
- Nhung thang yeu nhat:
  - `thang 5 = 17.6601`
  - `thang 9 = 17.7183`
  - `thang 4 = 17.9363`
- Nghia la khong chi muc tai trung binh thay doi theo thang, ma hinh dang cua profile trong ngay cung thay doi theo thang.

## 19) Bay kieu ngay dien hinh trong du lieu
- Neu chi chia `weekday/weekend` thi van con qua tho. Khi tach du lieu theo quy trinh `holiday -> non-holiday anomaly -> Saturday -> Sunday -> cluster weekday sach`, toan bo `2153` ngay day du `48` moc duoc phan thanh `7` kieu ngay ro rang hon.
- Quy tac tach `non-holiday anomaly` la:
  - `zero_ratio >= 8%`, hoac
  - `max_down <= -15`, hoac
  - `load_mean` thap hon median cua cung `month x weekday` tu `35%` tro len.

| Kieu ngay | So ngay | Ty trong | Load mean | Amplitude | Temp mean | Peak | Dac diem chinh |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `Ngay lam viec muc tai vua / mua thap diem` | `759` | `35.25%` | `23.0700` | `23.5640` | `26.6890` | `21:00` | Nhom weekday pho bien nhat, tai muc vua, xuat hien nhieu o `May-Nov` |
| `Ngay lam viec cao diem nong` | `540` | `25.08%` | `25.6080` | `25.2890` | `28.4480` | `21:00` | Weekday tai cao, nong hon nhom weekday co ban, tap trung o `Jan-Apr`, `Dec` |
| `Thu bay nua lam viec` | `287` | `13.33%` | `23.8790` | `23.7390` | `27.2860` | `21:30` | Thu bay khong le, profile gan weekday nhung peak muon hon |
| `Chu nhat` | `284` | `13.19%` | `24.4230` | `23.5710` | `27.2250` | `18:30` | Chu nhat khong le, peak toi som hon va khong thap sau nhu holiday |
| `Ngay lam viec profile phang / regime dau chuoi` | `157` | `7.29%` | `23.2250` | `20.9100` | `26.7230` | `00:00` | Weekday co profile phang hon, peak lech ve dau ngay, xuat hien nhu mot regime rieng |
| `Ngay le / Tet` | `76` | `3.53%` | `13.5069` | `21.6070` | `27.5340` | `18:30` | Holiday/Tet rat thap tai, dip giua ngay sau va trough rat sau |
| `Ngay sut giam bat thuong / nghi co su co` | `50` | `2.32%` | `16.4360` | `23.6750` | `27.0460` | `22:30` | Khong phai holiday nhung muc tai bi nen xuong manh, co dau hieu zero-run hoac step drop |

### 1) Ngay lam viec muc tai vua / mua thap diem
- Day la nhom lon nhat: `759` ngay, chiem `35.25%` snapshot.
- Peak trung binh o `21:00 = 31.0640`, trough o `11:00 = 11.2970`.
- Tap trung manh o `May-Nov`, dac biet `thang 7 = 105` ngay va `thang 10 = 109` ngay.
- Day la "weekday co ban" cua dataset, muc tai khong qua cao nhung van giu hinh dang ngay-dem ro.

### 2) Ngay lam viec cao diem nong
- Co `540` ngay, chiem `25.08%`.
- So voi nhom weekday co ban, nhom nay:
  - `load_mean` cao hon `11.00%`
  - `temp_mean` cao hon `1.76 C`
  - amplitude cung lon hon (`25.2890` so voi `23.5640`)
- Peak trung binh o `21:00 = 34.0540`, trough o `11:30 = 12.0700`.
- Tap trung nhieu o `Jan-Apr` va `Dec`, rieng `thang 3 = 123` ngay.

### 3) Thu bay nua lam viec
- Co `287` ngay, chiem `13.33%`.
- `load_mean = 23.8790`, chi thap hon weekday sach (`24.0280`) khoang `0.62%`.
- Peak o `21:30 = 31.3750`, muon hon weekday co ban.
- Nhom nay khong thap sau nhu holiday, va profile thuc te gan weekday hon la Sunday/Tet.

### 4) Chu nhat
- Co `284` ngay, chiem `13.19%`.
- `load_mean = 24.4230`, cao hon weekday sach khoang `1.64%`.
- Peak o `18:30 = 31.6260`, som hon ro so voi weekday (`21:00-21:30`).
- Nghia la trong dataset nay, `Chu nhat` khong trung voi `holiday/Tet`; no tach thanh mot kieu ngay rieng.

### 5) Ngay lam viec profile phang / regime dau chuoi
- Co `157` ngay, chiem `7.29%`.
- Peak dai dien bi lech ve `00:00 = 30.1030`, trough o `11:00 = 12.9190`.
- Amplitude chi `20.9100`, thap hon nhom weekday co ban khoang `11.26%`.
- Day khong giong mot kieu `cool workday` ro rang, ma giong mot regime profile phang hon, xuat hien roi rac nhieu nam nhung rat khac hinh so voi phan con lai.

### 6) Ngay le / Tet
- Co `76` ngay, chiem `3.53%`.
- `load_mean = 13.5069`, thap hon weekday sach (`24.0280`) khoang `43.79%`.
- Peak trung binh o `18:30 = 23.2190`, trough o `11:30 = 5.0820`.
- Phan bo theo thang tap trung o `Jan-Feb`, `Apr-May`, `Sep`, dung voi cum holiday trong snapshot.
- Day la nhom de tach ra de nhat va giam tai manh nhat.

### 7) Ngay sut giam bat thuong / nghi co su co
- Co `50` ngay, chiem `2.32%`.
- Day la nhom `khong phai holiday` nhung:
  - co `zero_ratio = 5.9%`
  - `max_down` trung binh sau hon (`-8.1440`)
  - `load_mean` thap hon baseline `month x weekday` trung binh `33.27%`
- Peak dai dien o `22:30 = 24.4680`, trough o `11:30 = 6.4730`.
- Cac ngay nhu `2021-10-17`, `2023-04-16`, `2024-08-04` nam rat gan nhom nay.

### Ket luan tu viec nhom ngay
- Neu ep du lieu vao `weekday/weekend` thi mat rat nhieu cau truc. Trong snapshot nay co `7` kieu ngay tach biet du ro de thong ke rieng.
- Ba nhom lon nhat van la cac `weekday` binh thuong, nhung ngay lam viec khong chi co mot kieu: co nhom `muc tai vua`, nhom `cao diem nong`, va nhom `profile phang`.
- `Saturday`, `Sunday`, `holiday/Tet`, va `ngay sut giam bat thuong` nen duoc xem la `4` loai ngay rieng, khong nen gop chung.
- Trong cac vi du user neu, dataset nay tach ra rat ro:
  - `Ngay lam viec nong cao diem`
  - `Ngay lam viec bi sut giam bat thuong`
  - `Thu bay nua lam viec`
  - `Ngay le / Tet`
- Rieng `Ngay lam viec mat troi` khong noi len thanh mot cum rieng ro rang; du lieu tach manh hon theo `muc tai + hinh dang profile + bat thuong` hon la chi theo nhiet do thap/cao.
