# Class 2 — Linux commands for log files

## Counting lines
- `wc -l module-01/data/orders_oct2026.csv` — CSV ফাইলে মোট কয়টা লাইন আছে গোনে
- `wc -l module-01/data/app_2026-10.log` — log ফাইলে মোট কয়টা লাইন আছে গোনে

## Searching with grep
- `grep -c "| WARN |" module-01/data/app_2026-10.log` — কতগুলো লাইনে WARN আছে গোনে
- `grep -c "| DEBUG |" module-01/data/app_2026-10.log` — কতগুলো লাইনে DEBUG আছে গোনে
- `grep "^2026-10-10" module-01/data/app_2026-10.log | head -5` — যেসব লাইন 2026-10-10 দিয়ে শুরু, তার প্রথম ৫টা দেখায়
- `grep "^2026-10-10" module-01/data/app_2026-10.log | grep -c "| ERROR |"` — ওই তারিখে কয়টা ERROR হয়েছে গোনে
- `grep -c "bKash" module-01/data/orders_oct2026.csv` — "bKash" লেখা আছে এমন লাইন গোনে (বড়-ছোট হাতের অক্ষর মেলাতে হয়)
- `grep -ic "bkash" module-01/data/orders_oct2026.csv` — একই কাজ, তবে -i থাকায় বড়-ছোট হাতের অক্ষর নিয়ে মাথা ঘামায় না

## Cutting columns
- `cut -c1-10` — প্রতিটা লাইনের প্রথম ১০টা অক্ষর কাটে, এখানে সেটাই তারিখ
- `cut -d'|' -f3 module-01/data/app_2026-10.log` — `|` দিয়ে ভাগ করে ৩ নম্বর কলামটা নেয়

## Counting groups
- `cut -d'|' -f3 module-01/data/app_2026-10.log | sort | uniq -c` — ৩ নম্বর কলামের প্রতিটা মান কতবার এসেছে গোনে
- `grep "| ERROR |" module-01/data/app_2026-10.log | grep -c "| auth"` — ERROR লাইনগুলোর মধ্যে কয়টায় auth আছে গোনে

## The 5-step chain (grep | cut | sort | uniq -c | sort -rn)
```
grep "| WARN" module-01/data/app_2026-10.log | cut -c1-10 | sort | uniq -c | sort -rn | head -1
```
1. `grep "| WARN"` — শুধু WARN-এর লাইনগুলো রাখে
2. `cut -c1-10` — প্রতিটা লাইন থেকে শুধু তারিখটা কেটে নেয়
3. `sort` — তারিখগুলো পাশাপাশি সাজায়, যাতে একই তারিখ একসাথে থাকে
4. `uniq -c` — পাশাপাশি থাকা একই তারিখ গুনে একটা লাইনে দেখায়
5. `sort -rn` — সংখ্যা ধরে বড় থেকে ছোট সাজায়
6. `head -1` — সবার উপরের লাইন, অর্থাৎ সবচেয়ে বেশি WARN হওয়া তারিখ

একই চেইনে `sort -n | head -1` দিলে উল্টো ফল আসে, অর্থাৎ সবচেয়ে কম ERROR হওয়া তারিখ (শুধু যেসব তারিখে অন্তত একটা ERROR আছে তার মধ্যে)।
