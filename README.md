# Studi Kasus

Aplikasi ini bertujuan simulasi pembelian makanan dan minuman dengan menggunakan promo/voucher.

## Scope and Functionalities

- User dapat melihat daftar makanan dan minuman
- User dapat melihat voucher yang ditawarkan
- User bisa menggunakan voucher untuk diskon makanan dan minuman yang dipesan
- User dapat menghapus pesanan yang udah di pesan
- User dapat melihat potongan harga dengan menggunakan salah satu voucher yang tersedia

## How Does It Works?

Dalam kasus yang diberikan oleh saya, ada 4 buah model dan 3 buah controller yang bertujuan untuk menjalankan program ini.

Dimulai dari 'Penawaran.xaml.cs'

```csharp
private void generateContentPenawaran()
        {
            Item coffeLate = new Item("Coffe Late", 30000);
            Item blackTea = new Item("BlackTea", 20000);
            Item milkShake = new Item("Milk Shake", 15000);
            Item watermelonJuice = new Item("Watermelon Juice", 25000);
            Item lemonSquash = new Item("Lemon Squash", 30000);
            Item pizza = new Item("Pizza", 75000);
            Item friedRice = new Item("Fried Rice Special", 45000);

            Penawarancontroller.addItem(coffeLate);
            Penawarancontroller.addItem(blackTea);
            Penawarancontroller.addItem(milkShake);
            Penawarancontroller.addItem(watermelonJuice);
            Penawarancontroller.addItem(lemonSquash);
            Penawarancontroller.addItem(pizza);
            Penawarancontroller.addItem(friedRice);

            listPenawaran.Items.Refresh();
        }
```
